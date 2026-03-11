---
layout: default
title: Home
---

# A* Algorithm Project (C++)
**Nathan McCormack — 2025/2026**

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## Overview

This project is a **modern C++ console application** that implements the **A\*** (A-star) pathfinding algorithm on a **2D grid with obstacles**. The movement model is **4-directional only (no diagonals)**, and the chosen heuristic is **Manhattan distance** — the natural and admissible choice for this movement model.

The project is structured across three source files:

| File | Role |
|---|---|
| `helloWorld.h / .cpp` | `aStarGrid` class — grid representation, neighbours, heuristic |
| `aStar.h / .cpp` | `aStarSearch` function — the A* algorithm itself |
| `main.cpp` | Entry point — runs a demo search and visualises results |

This separation follows the **single-responsibility principle**: the grid does not know about search, and the search does not know about rendering.

---

## Features implemented

- 2D grid representation with configurable obstacles (0 = free, 1 = blocked)
- Configurable start and goal positions with input validation
- A\* search using a min-heap open set (`std::priority_queue`) and boolean closed set
- Manhattan distance heuristic (admissible and consistent)
- Lazy-deletion / stale-entry handling for the open set (avoids need for decrease-key)
- Path reconstruction via `cameFrom` parent table
- Console visualisation overlaying explored cells (`x`) and optimal path (`*`)
- Runtime statistics: path length (moves) and nodes expanded

---

## Sample output

Running the default 5×5 grid:

```
Base grid:
S 0 0 0 0
1 0 1 1 0
0 0 0 0 1
0 0 1 0 1
0 1 1 0 G

A* result:
Path found. Path length = 8 moves
Nodes expanded = 10
S * . . .
# * # # .
. * * * #
. x # * #
. # # * G
```

Legend: `S` = start, `G` = goal, `#` = obstacle, `*` = path, `x` = explored (not on path), `.` = unvisited free cell.

The algorithm explored only **10 nodes** on a 25-cell grid — the heuristic is directing search efficiently toward the goal.

---

## How to build and run

```bash
# Compile (all three .cpp files together)
g++ -std=c++17 -O2 -Wall -Wextra -pedantic main.cpp helloWorld.cpp aStar.cpp -o astar

# Run
./astar
```

Requires a C++17-compatible compiler (g++ 7+ or clang++ 5+).