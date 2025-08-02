---
layout: post
title: "Rosetta Stones and Mazes 1"
date: 2025-08-01
series: "Rosetta Stones and Mazes"
---

## Rosetta Stones and Mazes 1 

Previously, I proposed that building and solving a maze is the ideal Rosetta Stone program. If you attempted to implement the program before arriving here as suggested, great work! Perhaps you are wondering how an implementation is supposed to check off the wide range of language features mentioned in the Hit and Miss table. This post will discuss some design choices you can make to check off as many language features as possible with a maze program. 

For this series I will choose C++ as my sample language when discussing design decisions. I would suggest choosing a language you already know well as a starting point for this program so that you can try to take the best-practice designs you are familiar with to new languages when you try to re-implement the program.

## Maze Architecture

First, we will design the maze itself. Because we are operating in the terminal we should become familiar with the grid available to use.

*Image showing how terminal grid cells are roughly twice as tall as they are wide goes here.*

The above grid represents the cells of a terminal. Each cell is roughly rectangular, being slightly stretched vertically. This is important to consider when trying to make mazes that look good in the terminal. For example, a common first attempt at a maze will often look something like this.

```txt
+-+-+-+-+-+-+
|S      |   |
+-+-+-+ +-+ +
|     |     |
+ +-+ + +-+-+
|     |     |
+-+ +-+ + + +
| |     | | |
+ +-+ +-+ +-+
|   | |     |
+-+ + + +-+-+
|
+-+-+-+-+-+-+
```

Using a combination of ASCII characters allows us to create a somewhat recognizable maze structure. However, if we look to Unicode we can discovers some characters that are tailor made to our task of building paths and walls. 

