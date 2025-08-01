---
title: Rosetta Stones and Mazes
date: 2025-08-01
---

## Rosetta Stones and Mazes

Professor Nick Parlante at Stanford University once described his "Rosetta Stone" program to a class full of burgeoning programmers in his CS106A course taught in Python. He claimed such a program aims to help one learn a new programming language quickly. The program should always remain the same and it should attempt to cover a decent range of the language one seeks to learn.

Professor Parlante's Rosetta Stone was a word count program. The requirements were roughly as follows:

- Accept any text file as input.
- Gather all counts of individual words in the text.
- Report various traits of this data as specified by command line parameters.
  - Report top N most frequent words where N is given.
  - Report last N most frequent words where N is given.
  - Report words and their counts in alphabetic order.
  - Report words and their counts in reverse alphabetic order.
  - Various other tasks with the data that the programmer may specify.
- Print the results of these queries to a stream (preferably the terminal output).

If programmer experience is a spectrum then this task is great for those at the top and bottom of that spectrum. For those at the bottom this may be slightly tricky but will ultimately be a manageable, repeatable task that introduces popular data structures, algorithms, and operating system interactions. For those at the top, this is a high level tour of a language and its implementation of the data structures the programmer is already deeply familiar with; they will be able to get a sense for the style of the language and patterns it uses without wasting too much of their time. 

However, I think for those in the middle of that spectrum, those of us who need to improve our programming skills and are seeking more complex tasks that stretch our abilities, there exists a better Rosetta Stone. Let's enumerate what the word count program covers and what it does not.


| Hit | Miss |
|---------|-------------|
|Basic Control Flow|Common Textbook Algorithms |
|Associative Containers|Recursion |
|Sorting|Bit Manipulation|
|File IO|Composition|
|Standard Output|Multithreading/Processes|
|Build Systems|3rd Party Libraries|
|Command Line Arguments|Creating Types|
||Trees|
||Stacks|
||Heaps|
||Queues|
||Graphs|
||Multi-Dimensional Arrays|
||Fun!|

The last point is important only in that the program described is somewhat simple. I am sure there are ways to spice it up but for a Rosetta Stone program I think there should be some entertaining and immediate feedback for the programmer. Importantly, I also think there should be a very high ceiling of complexity such that a programmer can take their Rosetta Stone program as far as they wish, depending on their needs.

Enter Mazes.

### Maze Building and Solving


```cpp
int main(void)
{
    return 0;
}
```
