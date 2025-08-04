---
layout: post
title: "Rosetta Stones and Mazes: The Premise"
date: 2025-08-01
series: "Rosetta Stones and Mazes"
---

## Prerequisites

Before starting, readers should

- [have a basic understanding of at least one programming language](https://en.wikipedia.org/wiki/Programming_language).
- [understand basic file input and output for a programming language](https://en.wikipedia.org/wiki/Input/output).
- [understand how a programming language processes command line arguments](https://en.wikipedia.org/wiki/Command-line_argument_parsing).

## Outline

By the end of this post, readers will

- [know what “Rosetta Stone” means in the context of programming](#rosetta-stone-programs). 
- [understand how a word counting program meets the needs of a “Rosetta Stone” program](#word-counting).
- [have the information needed to attempt maze building and solving](#a-maze-program).

## Rosetta Stone Programs

The first time I heard the phrase “Rosetta Stone” in the context of programming was in an introductory programming course. Stanford University Professor [Nick Parlante](https://cs.stanford.edu/people/nick/) used this phrase in a class full of burgeoning computer scientists taking his CS106A course to describe a program that helps one learn a new programming language quickly. The program should never change as one seeks to implement it with different languages, and it should attempt to cover a decent range of features from the language one seeks to learn.

### Word Counting

Professor Parlante's Rosetta Stone was a word count program. The requirements were roughly as follows:

- Accept any text file as input.
- Gather all counts of individual words in the text.
- Words should be cleaned up and normalized to lowercase characters if applicable.
- Report various traits of this data as specified by command line parameters.
  - Report top N most frequent words where N is given.
  - Report last N most frequent words where N is given.
  - Report words and their counts in alphabetic order.
  - Report words and their counts in reverse alphabetic order.
  - The programmer may add further requirements. 
- Print the results of these queries to a stream (preferably the terminal output).

If programmer experience is a spectrum then this task is great for those at the top and bottom of that spectrum. For those at the bottom this may be slightly tricky but will ultimately be a manageable, repeatable task that introduces popular data structures and operating system interactions. For those at the top, this is a high level tour of a language and its implementation of the data structures the programmer is already deeply familiar with; they will be able to get a sense for the constructs of the language and patterns it uses without wasting too much of their time. 

However, I think for those in the middle of that spectrum, those of us who need to improve our programming skills and are seeking more complex tasks that stretch our abilities, there exists a better Rosetta Stone. Let's enumerate what the word count program covers and what it does not.


| Hit                    | Miss                       |
| ---------------------- | -------------------------- |
| Basic Control Flow     | Common Textbook Algorithms |
| Associative Containers | Recursion                  |
| Sorting                | Bit Manipulation           |
| File IO                | Composition                |
| Standard Output        | Multithreading/Processes   |
| Build Systems          | 3rd Party Libraries        |
| Command Line Arguments | Type Creation              |
| Functions              | Interface Design           |
| Tests                  | Trees                      |
| Strings                | Stacks                     |
|                        | Heaps                      |
|                        | Queues                     |
|                        | Graphs                     |
|                        | Multidimensional Arrays    |
|                        | Fun!                       |

The last point is important only in that the program described is somewhat simple. I am sure there are ways to spice it up but for a Rosetta Stone program I think there should be some entertaining and immediate feedback for the programmer. Importantly, I also think there should be a very high ceiling of complexity such that a programmer can take their Rosetta Stone program as far as they wish, depending on their needs.

Enter Mazes.

## Maze Building and Solving

Wikipedia gives the following definition for a [Maze](https://en.wikipedia.org/wiki/Maze).

> A maze is a path or collection of paths, typically from an entrance to a goal. The word is used to refer both to branching tour puzzles through which the solver must find a route, and to simpler non-branching (“unicursal”) patterns that lead unambiguously through a convoluted layout to a goal.

For the purposes of this article, and those that follow, we will consider unicursal mazes. Specifically, we will be most interested in perfect mazes: mazes for which there are no loops and a path exists between any two locations. 

### A Maze Program

These types of mazes allow us to implement ideas from tree and graph theory which further increases the value of the program for learning purposes. The problem statement for this program is as follows.

- Generate a perfect maze.
  - The maze dimensions may be provided at the command line if they differ from the defaults.
  - Minimum and maximum sizes may be chosen by the programmer.
  - The maze paths and walls should be discernible and aesthetically pleasing from the terminal. Choose characters wisely.
- Output the maze building process to the terminal, ideally in real time.
- Place a start and finish square within the built maze.
- Output the solving process to the terminal, ideally in real time.

I pick the terminal as the display medium for this program because it is essential to most programming languages' input and output systems. One could decide to create an application with a window for this task, more in the style of game development, but the goal is to quickly get up and running with immediate feedback on the program. The terminal is the simplest tool for such a goal. However, this program could be easily extended to pursue a windowed implementation (remember a high complexity ceiling is a good thing for a Rosetta Stone program).

## Try for Yourself

In the remainder of this series we will explore how to approach this Rosetta Stone program in such a way that we hit all the points from the earlier table. Before continuing the series, now would be a good time to attempt this program. The problem statement is somewhat open-ended which leaves plenty of opportunity for creativity and research. 

## Conclusion

If you would rather read on, the remainder of articles will not spoil how to implement the algorithms and logic of building and solving a maze. Future articles will simply discuss one possible design approach to maze paths, walls, builders, and solvers such that your implementations are fun and efficient.
