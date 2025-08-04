---
layout: post
title: "Rosetta Stones and Mazes: Advanced Design"
date: 2025-08-04
series: Rosetta Stones and Mazes
---
## Prerequisites

Before starting, readers should

- [be familiar with immediate mode rendering](https://en.wikipedia.org/wiki/Immediate_mode_(computer_graphics)). 
- [know the basics of incorporating an external library into a programming language](https://en.wikipedia.org/wiki/Library_(computing)).

## Outline

By the end of this post, readers will

- [understand a basic render loop for the computer terminal](#terminal-rendering).
- [understand the render loop provided by ratatui.rs](#rendering-with-ratatui).
- [have directions to pursue for more advanced maze building](#conclusion).

## Checklist

| Hit                        | How                                                                                                                                                                                                             |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Basic Control Flow         | Sequentially building a maze and solving it.                                                                                                                                                                    |
| Associative Containers     | Maze building and solving algorithms.                                                                                                                                                                           |
| Sorting                    | Maze building and solving algorithms.                                                                                                                                                                           |
| File IO                    | Output to terminal with stdout.                                                                                                                                                                                 |
| Standard Output            | Printing maze walls and thread paths to terminal.                                                                                                                                                               |
| Build Systems              | Setting up the project.                                                                                                                                                                                         |
| Command Line Arguments     | Maze size input to program and algorithm selection.                                                                                                                                                             |
| Functions                  | Any maze building or solving function.                                                                                                                                                                          |
| Tests                      | Immediate feedback from terminal.                                                                                                                                                                               |
| Strings                    | Command line argument and maze character printing.                                                                                                                                                              |
| Common Textbook Algorithms | Maze building and solving algorithms.                                                                                                                                                                           |
| Recursion                  | Maze building and solving algorithms.                                                                                                                                                                           |
| Bit Manipulation           | Maze wall and solver design.                                                                                                                                                                                    |
| Composition                | Separating the program into three parts, maze design, maze building, and maze solving is an example of composition. Consider creating one modules for each part and passing the maze to the builder and solver. |
| Multithreading/Processes   | Maze solver design.                                                                                                                                                                                             |
| 3rd Party Libraries        | Incorporate a library to help with maze rendering.                                                                                                                                                              |
| Type Creation              | Maze wall design.                                                                                                                                                                                               |
| Interface Design           | Keeping the maze object simple and passing it to a builder and solver interface that accept a maze is a good idea. This means the builders and solvers expose their own interfaces.                             |
| Trees                      | Maze building and solving algorithms.                                                                                                                                                                           |
| Stacks                     | Maze building and solving algorithms.                                                                                                                                                                           |
| Heaps                      | Maze building algorithms.                                                                                                                                                                                       |
| Queues                     | Maze solving algorithms.                                                                                                                                                                                        |
| Graphs                     | Maze building and solving algorithms.                                                                                                                                                                           |
| Multidimensional Arrays    | Maze square storage.                                                                                                                                                                                            |
| Fun!                       | All of it!                                                                                                                                                                                                      |

## Terminal Rendering 

If you have made it through the Rosetta Stone series you likely have implemented your own rendering system or have some idea for how you would implement it if you haven't started. While I will still leave the core maze building and solving logic to you, I will discuss different techniques I used to render the process to the screen.

### Narcoleptic Threads

A very easy way to render the maze building and solving process to the screen in real time is with the following steps.

1. Update the in memory representation of your maze either with the bit manipulations we have covered or other methods.
2. Move the terminal cursor to the position of this update.
3. Print the current state of that square to the screen.
4. Sleep.

Here is what that looks like in my C++ version of this program.

```cpp
void
build_path_animated(Maze::Maze &maze, const Maze::Point &p,
                    Speed::Speed_unit speed)
{
    maze[p.row][p.col] |= Maze::path_bit;
    flush_cursor_maze_coordinate(maze, p);
    std::this_thread::sleep_for(std::chrono::microseconds(speed));
    if (p.row - 1 >= 0 && !(maze[p.row - 1][p.col] & Maze::path_bit))
    {
        maze[p.row - 1][p.col] &= ~Maze::south_wall;
        flush_cursor_maze_coordinate(maze, {p.row - 1, p.col});
        std::this_thread::sleep_for(std::chrono::microseconds(speed));
    }
    if (p.row + 1 < maze.row_size()
        && !(maze[p.row + 1][p.col] & Maze::path_bit))
    {
        maze[p.row + 1][p.col] &= ~Maze::north_wall;
        flush_cursor_maze_coordinate(maze, {p.row + 1, p.col});
        std::this_thread::sleep_for(std::chrono::microseconds(speed));
    }
    if (p.col - 1 >= 0 && !(maze[p.row][p.col - 1] & Maze::path_bit))
    {
        maze[p.row][p.col - 1] &= ~Maze::east_wall;
        flush_cursor_maze_coordinate(maze, {p.row, p.col - 1});
        std::this_thread::sleep_for(std::chrono::microseconds(speed));
    }
    if (p.col + 1 < maze.col_size()
        && !(maze[p.row][p.col + 1] & Maze::path_bit))
    {
        maze[p.row][p.col + 1] &= ~Maze::west_wall;
        flush_cursor_maze_coordinate(maze, {p.row, p.col + 1});
        std::this_thread::sleep_for(std::chrono::microseconds(speed));
    }
}
```

Anyone familiar with common render pipelines may be scoffing at this technique. We will get to something better. But before you knock it too much, consider the pros to this approach.

- You are visually debugging your algorithms in real time. If a piece of logic in your code breaks, you will see the visualization break after the update and flush.
- You are sipping CPU resources on your system. Depending on how fast the animation setting is, the thread could be sleeping for thousands of microseconds, which is a long time for the CPU to handle its other tasks. 
- You can reason about your program sequentially. Everything that is printed to the screen happens in the order you specify, and you observe how the operating system handles your system calls.

However, to be fair, there are significant downsides to this approach.

- You must wait in real time for your algorithms to finish. You can do nothing else in your program while this process runs.
- You have limited what one thread is capable of. It has a very simple task, and then it sleeps becoming useless until it wakes again.
- You are adding code bloat and room for error every time you need to insert the flush and sleep calls. 

If you instead choose to render the entire maze as frequently as your terminal allows, updating squares when they are changed, you fall into a more common pattern of frame rendering.

### Rendering Frames

Let's take a step back and separate the logic and algorithms we use to build and solve mazes from our desire to show this process on the screen. If you have followed my design cues through this series, then we know two things.

1. Our representation of maze squares is extremely compact (16-32 bits, depending on your approach).
2. Every operation we perform on our bits for the building or solving process is [idempotent](https://en.wikipedia.org/wiki/Idempotence). We add bits via a logical OR operation and removing bits with the logical AND operation using the inverse of the bits of interest. These operations can be applied multiple times with no inconvenient side effects. We even ensured that the 24-bit colors we used for multiple threads have this idempotence.

This allows to get more creative with how we build and solve the maze versus how we display it. The key insight is as follows. 

**We can record the history of our building and solving process in memory before we display anything. The state of a square can be set to any visual state by only modifying the bits of that specific `u32`, or `Square`.**

As we progress our algorithms we will record how every square in the maze changes over time. Every action we take will be recorded as a before state and an after state. Here are the types we use to represent this abstraction.

```rust
#[derive(Debug, Default, Clone, Copy)]
pub struct Delta {
    pub id: Point,
    pub before: Square,
    pub after: Square,
    // We can enter deltas into our Tape but then 
    // specify how many squares to change in one frame.
    pub burst: usize,
}

#[derive(Debug, Default, Clone)]
pub struct Tape {
    steps: Vec<Delta>,
    i: usize,
}
```

We create the concept of a `Delta`, or change, of a square and where that change occurred. We also record all of these changes in a `Tape`; it has a simple index that acts as our iterator for where we are in this playback. A burst is helpful if we want to tell the frame render code to update multiple `Delta` locations before the next frame. 

Now, we update the maze object by placing the core structure of the maze in a `Blueprint` and adding a `Tape` type for both the builders and solvers. 

```rust
#[derive(Debug, Clone, Default)]
pub struct Blueprint {
    pub buf: Vec<Square>,
    pub rows: i32,
    pub cols: i32,
}

#[derive(Debug, Default, Clone, Copy)]
pub struct Delta {
    pub id: Point,
    pub before: Square,
    pub after: Square,
    pub burst: usize,
}

#[derive(Debug, Default, Clone)]
pub struct Tape {
    steps: Vec<Delta>,
    i: usize,
}

#[derive(Debug, Clone, Default)]
pub struct Maze {
    pub maze: Blueprint,
    pub build_history: Tape,
    pub solve_history: Tape,
}
```

The maze builders now have no interaction with system level IO calls. They are only responsible for recording their work in the tape. Recording these before and after states provides many benefits.

- We can play the building and solving forward.
- We can play the building and solving in reverse.
- We can step through the algorithms one `Delta` at a time, freely toggling forward and backward at will.
- We can render maze changes as quickly or as slowly as we wish, separate from the frame rate of our terminal.

Our next task is to figure out the render loop.

## Rendering with Ratatui

Rust is known for robust Terminal User Interface (TUI) programs. This is in large part to its language features and the crates programmers have shared with the ecosystem that organize these language features for us.

A notable crate in the TUI ecosystem is [ratatui.rs](https://ratatui.rs/). From their welcome page, here is the "what?" behind Ratatui.

> Ratatui is a Rust library for cooking up delicious TUIs (terminal user interfaces). It is a lightweight library that provides a set of widgets and utilities to build simple or complex rust TUIs.

Ratatui uses an immediate mode rendering abstraction. Various functions create visual elements on the terminal screen, and these functions are executed on every iteration of the core render loop. This is in contrast to the more object-oriented [retained mode](https://en.wikipedia.org/wiki/Retained_mode) rendering, in which only elements that change are updated.

Now, we must create the render thread and loop ourselves. Because this is a simple maze program that will interact with a user selecting maze builders and solvers, we do not have any need for an `async/await` abstraction with the help of a crate like `tokio`. Instead, we create the following event handler.

```rust
#[derive(Debug)]
pub enum Pack {
    Press(KeyEvent),
    Resize((), ()),
    Render,
}

#[derive(Debug)]
pub struct EventHandler {
    pub receiver: crossbeam_channel::Receiver<Pack>,
}
```

Then we set up our core loop with a sender and receiver with the help of [crossbeam](https://docs.rs/crossbeam/latest/crossbeam/channel/index.html) channels.

```rust
impl EventHandler {
    pub fn new(delta_rate: f64) -> Self {
        let mut deltas = Duration::from_secs_f64(1.0 / delta_rate);
        let (sender, receiver) = unbounded();
        let sender = sender.clone();
        thread::spawn(move || {
            let mut last_delta = Instant::now();
            loop {
                if event::poll(MIN_POLL).expect("poll error") {
                    match event::read().expect("event error") {
                        CtEvent::Key(e) => {
                            send_key_press(e);
                        }
                        CtEvent::Resize(_, _) => {
                            send_resize_refresh();
                        }
                        _ => {}
                    }
                } else if last_delta.elapsed() >= deltas {
                    sender.send(Pack::Render).expect("delta error");
                    last_delta = Instant::now();
                }
            }
        });
        Self { receiver }
    }

    pub fn next(&self) -> Option<Pack> {
        self.receiver.recv().ok()
    }
}
```

I have cut out some code and shortened things to make the core logic clearer. There are two important details.

1. The loop monitors for key presses. Because this is immediate mode rendering, the internal maze render code will have to change the functions it calls on each loop to display what is needed based on user input.
2. We have an adjustable speed for rendering the pace of maze changes. The user can send key presses that speed up or slow down the `deltas` duration. If enough time has passed we send a signal to the maze rendering code to update the maze squares as specified in the playback `Tape` we make during building and solving.

Finally, this allows the internal `render_maze` loop to only worry about rendering a frame when told to do so.

```rust
fn render_maze(this_run: tables::HistoryRunner, tui: &mut tui::Tui) -> tui::Result<()> {
    let render_space = tui.inner_maze_rect();
    let mut play = new_tape(&this_run);
    'rendering: loop {
        'building: while let Some(ev) = tui.events.next() {
            match ev {
                tui::Pack::Press(ev) => {
                    if !handle_press(
                        tui,
                        ev.code,
                        tui::Process::Building,
                        &this_run,
                        &mut play,
                        &render_space,
                    ) {
                        break 'rendering;
                    }
                }
                tui::Pack::Render => {
                    if !play.build_delta() {
                        break 'building;
                    }
                    tui.render_maze_frame(
                        tui::BuildFrame {
                            maze: &mut play.maze,
                        },
                        &render_space,
                        play.forward,
                        play.pause,
                    )?;
                }
                tui::Pack::Resize(_, _) => break 'rendering,
            }
        }
        'solving: while let Some(ev) = tui.events.next() {
            match ev {
                tui::Pack::Press(ev) => {
                    if !handle_press(
                        tui,
                        ev.code,
                        tui::Process::Solving,
                        &this_run,
                        &mut play,
                        &render_space,
                    ) {
                        break 'rendering;
                    }
                }
                tui::Pack::Render => {
                    if !play.solve_delta() {
                        break 'solving;
                    }
                    tui.render_maze_frame(
                        tui::SolveFrame { maze: &play.maze },
                        &render_space,
                        play.forward,
                        play.pause,
                    )?;
                }
                tui::Pack::Resize(_, _) => break 'rendering,
            }
        }
    }
    Ok(())
}
```

Every time a new maze is requested by the user, we generate its playback tape first. Then, we have the build phase and solve phase loops that the user can interact with.

There are many more details to manage with Ratatui. But after the core event and render loops are taken care of, those details are the fun widget design and user interaction choices that make the program personalized to one's style.

Overall, my experience with the Ratatui library was great. Please check out their site. If you want to see how all the pieces come together in a functional program, check out my [maze-tui](https://github.com/agl-alexglopez/maze-tui/tree/main) project on GitHub.

## Conclusion

That's it! We have checked off all the language feature boxes we said we would. Choosing a maze building program as your Rosetta Stone even allows you to practice incorporating third party libraries into your code. This is an important step in learning how other people write code in your chosen language, how including their code works, and how to use build systems effectively.

Let's briefly discuss some directions you could take this program if you really wanted to advance your skills.

- Use a graphics library to create a window for your maze rather than using the built-in terminal. I think I will be trying this for the [Zig](https://ziglang.org/) programming. This will also require incorporation of a 3rd party library.
- Bring the maze into three dimensions. You might try a graphics framework in your language, but a [terminal is capable of 3D rendering](https://github.com/leonmavr/retrocube), believe it or not.
- Make the maze an interactive game or race between players over a network. Start up to four players in the four corners of the maze and have them race to the center. This will hit on networking, a language feature that this program did not hit, unfortunately.

I hope you consider using a maze building program the next time you want to learn a programming language. It is easy to feel pressure to produce something “productive” or something that creates “user-value” every time you spend personal time programming. However, I firmly believe it is essential to find time to remember how fun and entertaining programming can be just for **you**. Mazes can help make learning a joy, rather than a chore.
