---
layout: default
title: Design & Implementation
---

# Design & Implementation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Design goals

Before writing the implementation, I set four design goals:

1. **Correctness first** — always return an optimal path or correctly report that none exists.
2. **Readable structure** — split the code into small modules rather than one large file.
3. **Reusable grid abstraction** — keep grid logic separate from search logic.
4. **Defensive validation** — reject invalid grids and invalid start/goal placements early.

These goals shaped both the file structure and the coding style.

---

## 2. File structure and responsibilities

| File | Responsibility |
|---|---|
| `a_star_grid.h/.cpp` | Grid class, validation, neighbour generation, Manhattan heuristic |
| `a_star.h/.cpp` | Search result type and A\* implementation |
| `main.cpp` | Builds a demo grid, runs the search, prints a visual overlay |
| `unit_tests.h/.cpp` | Test harness and six unit tests |
| `Makefile` | Build automation for the demo binary and test binary |
| `.clang-format` | Shared formatting rules for editor/tool consistency |

This directly supports the brief requirement for **object-oriented design and modular code**.

---

## 3. `AStarGrid`: representing the environment

The environment is stored as a 2D `std::vector<std::vector<int>>`:

- `0` = free cell
- `1` = obstacle

The class encapsulates:

- grid dimensions
- start and goal positions
- validation rules
- neighbour generation
- grid printing
- the Manhattan distance helper

### Why a `Position` struct was used

Instead of `std::pair<int, int>`, the code uses a named struct:

```cpp
struct Position {
  int row;
  int col;
};
```

This improves readability because `position.row` and `position.col` make the coordinate meaning explicit. It also allows equality operators to be defined directly on the type.

---

## 4. Validation strategy

The implementation uses **fail-fast validation**.

### Grid validation

`ValidateRectangular()` checks that:

- the grid is not empty
- the grid has at least one column
- every row has the same width
- every cell is either `0` or `1`

### Position validation

`ValidateWalkablePosition()` checks that:

- the position is inside the grid
- the position is not placed on an obstacle

This is used by both `SetStart()` and `SetGoal()`, and it is also called by the constructor so that a custom grid cannot silently create an invalid default start or goal. That was an important robustness improvement over the earlier version.

---

## 5. Neighbour generation

Neighbour generation is isolated in `Neighbours4()`:

```cpp
constexpr std::array<AStarGrid::Position, 4> kDirections{{
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1},
}};
```

The function returns only valid orthogonal neighbours that are:

- in bounds
- not blocked by an obstacle

This keeps all movement rules in one place. If the project were later extended to support diagonals or weighted terrain, this is one of the first places that would need to change.

---

## 6. `AStarResult`: packaging search output cleanly

The search does not just return a boolean. It returns a dedicated result struct:

```cpp
struct AStarResult {
  bool found = false;
  std::vector<AStarGrid::Position> path;
  std::vector<AStarGrid::Position> visited_order;
  int nodes_expanded = 0;
};
```

This design is better than returning multiple values through separate reference parameters because it keeps the search output cohesive and easier to test.

The helper `PathLengthInMoves()` converts the stored path size into move count, which avoids repeating `path.size() - 1` logic elsewhere.

---

## 7. Core A* data structures

### 7.1 Open set

```cpp
std::priority_queue<OpenNode,
                    std::vector<OpenNode>,
                    OpenNodeGreater> open_set;
```

The open set stores candidate frontier nodes. It is implemented with `std::priority_queue` for efficient extraction of the lowest-ranked node.

### 7.2 `g_score`

```cpp
std::vector<std::vector<int>> g_score(
    row_count, std::vector<int>(col_count, kInfinity));
```

This stores the best known exact path cost from the start to every cell.

### 7.3 `came_from`

```cpp
std::vector<std::vector<Position>> came_from(
    row_count, std::vector<Position>(col_count, kNoParent));
```

This stores the parent of each cell on the best known route so the final path can be reconstructed by walking backward from the goal.

### 7.4 Closed set

A 2D table records whether a node has already been fully expanded. Because Manhattan distance is consistent for this problem, closed nodes do not need to be reopened.

---

## 8. Stale-entry handling

A common practical issue with A\* in C++ is that `std::priority_queue` has no **decrease-key** operation. If a better path to a node is found, the code cannot edit the old queue entry in place.

The solution used here is **lazy deletion**:

- push the improved entry into the queue
- leave the old entry where it is
- when the old entry is eventually popped, ignore it because its stored `g_score` no longer matches the best known table value

This keeps the implementation simple, correct, and efficient enough for the project scope.

---

## 9. Path reconstruction

Once the goal is reached, the path is rebuilt by following `came_from` links backward:

1. start at the goal
2. repeatedly move to its parent
3. stop when the start is reached
4. reverse the collected sequence

A sentinel position `{-1, -1}` is used to detect an invalid parent chain. This is mainly defensive programming, but it prevents a silent infinite loop if reconstruction data were ever corrupted.

---

## 10. Console visualisation

The final output layer is intentionally kept in `main.cpp`, not inside the search class. That separation matters:

- the algorithm module focuses on correctness
- the UI/demo module focuses on presentation

The overlay uses:

- `*` for the final path
- `x` for expanded cells not on the path
- `#` for obstacles
- `S` and `G` for the endpoints

This makes the search behaviour easy to explain during a lab demonstration.

---

## 11. Style decisions applied in the code

The project follows a consistent style influenced by the Google C++ Style Guide and the C++ Core Guidelines:

- **self-contained headers** with include guards
- **PascalCase** for types and major functions
- **snake_case** for variables and data members
- **private member trailing underscores**
- **symbolic constants** such as `kStepCost`, `kInfinity`, and `kNoParent`
- **no raw `new` / `delete`**
- comments used for non-obvious logic rather than obvious line-by-line narration

The formatter configuration in `.clang-format` helps enforce this consistently.

---

## 12. Reuse and extension potential

The code is intentionally designed so it can be extended without rewriting everything. Reasonable future extensions include:

- loading grids from a file instead of hardcoding them
- supporting weighted terrain
- adding diagonal movement with a different heuristic
- benchmarking A\* against BFS or Dijkstra
- visualising larger grids or animated search steps

That makes the current implementation a good foundation rather than just a one-off demo.
