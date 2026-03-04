---
layout: default
title: Home
---

# A* Algorithm Project (C++)

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

## Overview
This project is a **modern C++ console application** that implements the **A\*** pathfinding algorithm on a **2D grid with obstacles**. The movement model is **4-directional only (no diagonals)**, so the chosen heuristic is **Manhattan distance**.

### Current features implemented (so far)
- Grid representation with obstacles
- A* search (open set / closed set)
- Manhattan heuristic
- Path reconstruction and console visualisation
- Simple runtime statistics (path length, nodes expanded)

## How to Run (example)
> Update the commands below to match your build system.

### Option A: g++ (single file example)
```bash
g++ -std=c++17 -O2 -Wall -Wextra -pedantic main.cpp -o astar
./astar