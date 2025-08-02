---
title: Rosetta Stones and Mazes (0)
date: 2025-08-01
---

## Rosetta Stones and Mazes (0) 

*This is 0th article in the "Rosetta Stones and Mazes" series.*

Stanford University Professor Nick Parlante once described his "Rosetta Stone" program to a class full of burgeoning computer scientists in his CS106A course taught in Python. He claimed such a program aims to help one learn a new programming language quickly. The program should always remain the same and it should attempt to cover a decent range of the language one seeks to learn.

Professor Parlante's Rosetta Stone was a word count program. The requirements were roughly as follows:

- Accept any text file as input.
- Gather all counts of individual words in the text.
- Report various traits of this data as specified by command line parameters.
  - Report top N most frequent words where N is given.
  - Report last N most frequent words where N is given.
  - Report words and their counts in alphabetic order.
  - Report words and their counts in reverse alphabetic order.
  - The programmer may add further requirements. 
- Print the results of these queries to a stream (preferably the terminal output).

If programmer experience is a spectrum then this task is great for those at the top and bottom of that spectrum. For those at the bottom this may be slightly tricky but will ultimately be a manageable, repeatable task that introduces popular data structures and operating system interactions. For those at the top, this is a high level tour of a language and its implementation of the data structures the programmer is already deeply familiar with; they will be able to get a sense for the constructs of the language and patterns it uses without wasting too much of their time. 

However, I think for those in the middle of that spectrum, those of us who need to improve our programming skills and are seeking more complex tasks that stretch our abilities, there exists a better Rosetta Stone. Let's enumerate what the word count program covers and what it does not.


| Hit | Miss |
|---------|-------------|
|Basic Control Flow|Common Textbook Algorithms |
|Associative Containers|Recursion |
|Sorting|Bit Manipulation|
|File IO|Composition|
|Standard Output|Multithreading/Processes|
|Build Systems|3rd Party Libraries|
|Command Line Arguments|Type Creation|
|Functions|Interface Design|
|Tests|Trees|
||Stacks|
||Heaps|
||Queues|
||Graphs|
||Multi-Dimensional Arrays|
||Fun!|

The last point is important only in that the program described is somewhat simple. I am sure there are ways to spice it up but for a Rosetta Stone program I think there should be some entertaining and immediate feedback for the programmer. Importantly, I also think there should be a very high ceiling of complexity such that a programmer can take their Rosetta Stone program as far as they wish, depending on their needs.

Enter Mazes.

## Maze Building and Solving

Wikipedia gives the following definition for a [Maze](https://en.wikipedia.org/wiki/Maze).

> A maze is a path or collection of paths, typically from an entrance to a goal. The word is used to refer both to branching tour puzzles through which the solver must find a route, and to simpler non-branching ("unicursal") patterns that lead unambiguously through a convoluted layout to a goal.

For the purposes of this article, and those that follow, we will consider unicursal mazes. Specifically, we will be most interested in perfect mazes: mazes for which there are no loops and a path exists between any two locations. 

These types of mazes allow us to implement ideas from tree and graph theory which further increases the value of the program. The problem statement for this program is as follows.

- Generate a perfect maze.
  - The maze dimensions should be allowed to be specified as arguments to the program.
  - Minimum and maximum sizes may be chosen by the programmer.
  - The maze paths and walls should be discernible from the terminal. Choose characters wisely.
- Output the maze building process to the terminal, ideally in real time.
- Place a start and finish square within the built maze.
- Output the solving process to the terminal, ideally in real time.

I choose the terminal as the display medium for this program because it is essential to most programming languages' input and output systems. One could choose to create an application with a window for this task, more in the style of game development, but the goal is to quickly get up and running with immediate feedback on the program. The terminal is the simplest tool for such a goal. However, this program could be easily extended to pursue a windowed implementation (remember a high complexity ceiling is a good thing).

In the remainder of this series we will explore how to approach this Rosetta Stone program in such a way that we hit all the points from the earlier table. Before continuing the series, now would be a good time to attempt this program. The problem statement is somewhat open ended which leaves plenty of opportunity for creativity and research. After attempting to tackle the problem, explore the remainder of the series for tips on program design choices that maximize the programming language features we encounter.

