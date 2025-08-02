---
layout: post
title: "Rosetta Stones and Mazes: Design Tips"
date: 2025-08-02
series: "Rosetta Stones and Mazes"
---

## Rosetta Stones and Mazes: Design Tips 

## Outline

By the end of the article the reader will

- [understand the grid of most terminals](#the-terminal-grid).
- [be familiar with helpful Unicode characters for maze building](#box-drawing-characters).
- [understand basic bit encoding within integers as it relates to maze building](#bit-encoding).

## Choose a Language

Previously, I proposed that building and solving a maze is the ideal Rosetta Stone program. If you attempted to implement the program before arriving here as suggested, great work! Perhaps you are wondering how an implementation is supposed to check off the wide range of language features mentioned in the Hit and Miss table from the first post. This post will discuss some design choices you can make to check off as many language features as possible with a maze program. 

For this series I will choose Rust as my sample language when discussing design decisions. If you select another language you will inevitably run into some language features that force you to change the design and that is OK. 

## Maze Architecture

First, we will design the maze itself. Because we are operating in the terminal we should become familiar with the grid available to use.

### The Terminal Grid

![terminal-grid](/assets/images/terminal-grid.png)

The above grid represents the cells of a terminal. Each cell is rectangular, being slightly stretched vertically. We can think of the height as roughly double the width. This will become important to consider when trying to make mazes that look good in the terminal. For example, a common first attempt at a maze will often look something like this.

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

Using a combination of ASCII characters allows us to create a somewhat recognizable maze structure. However, due to rectangular cells the gaps between the vertical bars and the plus signs can seem like paths when in fact they are not. If we look to Unicode we can discover some characters that are tailor made to our task of building paths and walls. 

### Box-drawing Characters

*Note that to enjoy the remainder of this design, you will need to have installed a fully featured font that your terminal can use to display Unicode and box-drawing characters.*

One of my favorite Wikipedia pages is the [Box-drawing characters](https://en.wikipedia.org/wiki/Box-drawing_characters) page. The general purpose of these characters is to address the problems we saw with the tiny gaps in our attempt at mazes with ASCII characters. I often find myself visiting this page when writing documentation or diagrams for projects I am working on. It is very satisfying to have lines appear to seamlessly connect in a terminal or editor.

While traditionally intended to help create clean terminal user interfaces, box-drawing characters can also help us in our maze endeavors. What are mazes if not boxes with little chunks taken out to form paths? Let's examine characters that the Wikipedia page provides to us in the order they are presented. Here is the light weight variant of box-drawing characters we are most interested in.

```txt
─  │  ┌  ┐  └  ┘  ├  ┤  ┬  ┴  ┼
```

Naturally, we can form a grid with these characters.

![box-draw-grid](/assets/images/box-draw-grid.png)

### Bit Encoding
