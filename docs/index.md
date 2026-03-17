---
layout: default
title: A* Algorithm Project (C++)
---

# A* Algorithm Project (C++)

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Project overview

This project is a **modern C++ console application** that implements the **A\*** pathfinding algorithm on a 2D grid with obstacles. The program finds the shortest route from a start cell to a goal cell using a **heuristic-guided search**, then visualises both the final path and the cells expanded during the search.

The project was designed to satisfy the brief requirements for:

- modern C++ (C++17 used)
- object-oriented, modular design
- use of the C++ Standard Library
- clear testing and validation
- evidence of iterative development
- documented reflection and AI usage

---

## 2. Problem being solved

A pathfinding algorithm must answer a simple question efficiently:

> **Given a start cell, a goal cell, and a set of blocked cells, what is the shortest valid path?**

This is a useful problem in game AI, robotics, warehouse routing, and map navigation. A brute-force search such as BFS will eventually find the shortest path on an unweighted grid, but it wastes work by expanding outward in every direction. A\* improves this by combining:

- the **known distance already travelled** from the start (`g`), and
- a **heuristic estimate** of the remaining distance to the goal (`h`).

That balance makes it both correct and efficient for this type of grid.

---

## 3. Core design choices

### 3.1 4-direction movement only

Movement is restricted to:

- up
- down
- left
- right

No diagonals are allowed. This keeps the movement model simple and matches the chosen heuristic.

### 3.2 Manhattan distance heuristic

Because movement is orthogonal only, the heuristic used is **Manhattan distance**:

```text
h(n) = |row_n - row_goal| + |col_n - col_goal|
```

This is appropriate because it never over-estimates the true cost on a 4-direction grid with unit step cost. That gives A\* its optimality guarantee.

### 3.3 Modular class-based structure

The solution is split into independent modules:

| File | Responsibility |
|---|---|
| `a_star_grid.h/.cpp` | Grid storage, validation, neighbour generation, heuristic |
| `a_star.h/.cpp` | Search algorithm, open set, closed set, path reconstruction |
| `main.cpp` | Demo program and console output |
| `unit_tests.h/.cpp` | Unit test harness and test cases |
| `Makefile` | Simple build and test commands |

This separation makes the project easier to explain, test, and extend.

---

## 4. Sample output

Default demonstration run:

```text
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

Legend:

- `S` = start
- `G` = goal
- `#` = obstacle
- `*` = cells on the final shortest path
- `x` = expanded but not on the final path
- `.` = walkable cell never expanded

The output shows that the algorithm does not blindly explore the whole 5×5 grid. It expands only the cells needed to reach the goal efficiently.

---

## 5. Style and code quality conventions used

To keep the project consistent and professional, I applied one style across the whole codebase:

- **K&R / attached brace style**
- **PascalCase** for types and major functions (`AStarGrid`, `AStarResult`, `RunAStarSearch`)
- **snake_case** for local variables and struct data members (`row_count`, `nodes_expanded`)
- **trailing underscore** for private class data members (`grid_`, `start_`, `goal_`)
- self-contained headers with include guards
- symbolic constants instead of unexplained magic numbers where appropriate

A `.clang-format` file is included so formatting can be reproduced consistently in VS Code or another editor.

---

## 6. Modern C++ features used

This project uses C++17 and several modern C++ practices:

- `std::vector` for dynamic grid and table storage
- `std::priority_queue` for the A\* open set
- range-based `for` loops
- `constexpr` constants
- `[[nodiscard]]` on functions where ignoring a return value would be a mistake
- exceptions (`std::invalid_argument`) for invalid grid/state input
- move semantics when storing the reconstructed path

The implementation deliberately avoids raw pointers and manual memory management.

---

## 7. Why this project meets the brief well

This submission is more than a working demo. It also demonstrates:

- understanding of why A\* works
- justification of the heuristic choice
- reusable and extensible code structure
- edge-case handling and defensive validation
- separate unit tests for both normal and failure scenarios
- traceable project management, reflection, and ethical AI documentation

That alignment between **code**, **testing**, and **reporting** is important for the final mark.
