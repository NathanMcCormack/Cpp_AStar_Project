---
layout: default
title: Design & Implementation
---

# Design & Implementation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Project structure

The codebase is deliberately split into three logical units:

| File | Responsibility |
|---|---|
| `helloWorld.h / .cpp` | Grid representation, validation, heuristic |
| `aStar.h / .cpp` | A* algorithm — search and path reconstruction |
| `main.cpp` | Program entry point and console visualisation |

This separation follows the **single-responsibility principle**: each file has one job. Changing the heuristic does not touch main.cpp; changing the visualisation does not touch aStar.cpp.

---

## 2. Grid representation — `aStarGrid`

The environment is a 2D `std::vector<std::vector<int>>` stored inside the `aStarGrid` class:

- `0` = walkable (free) cell
- `1` = obstacle (blocked) cell

Coordinates are `(row, col)`, zero-based from the top-left corner.

### The `Pos` struct

Rather than using `std::pair<int,int>`, a named struct is used for clarity:

```cpp
struct Pos {
    int r;   // row
    int c;   // col

    bool operator==(const Pos& other) const noexcept {
        return r == other.r && c == other.c;
    }
};
```

Using `p.r` and `p.c` is significantly more readable than `p.first` and `p.second`, especially in grid-heavy code.

### Input validation

The constructor calls `validateRectangular()` which checks:
- The grid is not empty
- Every row has the same width (rectangular)
- Every cell value is exactly 0 or 1

`setStart()` and `setGoal()` additionally check that the chosen cell is in-bounds and not an obstacle. This fails fast with a descriptive `std::invalid_argument` rather than causing a silent bug later.

---

## 3. Neighbour generation — 4-direction movement

```cpp
const Pos dirs[4] = {
    {-1, 0},   // up
    { 1, 0},   // down
    { 0,-1},   // left
    { 0, 1}    // right
};
```

Only orthogonal movement is allowed. This is a deliberate design choice: Manhattan distance is only admissible when diagonal movement is excluded. Allowing diagonals would require switching to Chebyshev or Euclidean distance.

---

## 4. The A* algorithm — key data structures

### Open set (priority queue)

```cpp
std::priority_queue<Node, std::vector<Node>, NodeGreater> open;
```

`std::priority_queue` is used as a **min-heap** by providing a custom comparator `NodeGreater` that inverts the default ordering. The node with the smallest `f = g + h` is always at the top.

### Closed set (boolean array)

```cpp
std::vector<std::vector<bool>> closed(R, std::vector<bool>(C, false));
```

A simple 2D boolean array. Once a cell is popped from the open set and expanded, its entry is set to `true`. Any future occurrence of that cell in the queue is discarded immediately.

### gScore table

```cpp
std::vector<std::vector<int>> gScore(R, std::vector<int>(C, INF));
```

Stores the cheapest known cost from start to every cell. Initialised to `INF` (a large sentinel, not `INT_MAX` to avoid overflow). Updated whenever a cheaper route to a cell is discovered.

### cameFrom table (path reconstruction)

```cpp
std::vector<std::vector<Pos>> cameFrom(R, std::vector<Pos>(C, Pos{-1,-1}));
```

Records the predecessor of each cell on the best-known path. After the goal is reached, this table is walked backwards from goal → start, and then reversed to produce the final path.

---

## 5. Stale-entry handling (lazy deletion)

`std::priority_queue` does not support the "decrease-key" operation needed when a shorter path to an already-queued node is found. The solution is the **lazy deletion** pattern:

1. When a better path to node `n` is found, push a **new entry** for `n` with the updated `g` and `f`.
2. When a node is popped, **check if its `g` matches** `gScore[r][c]`. If not, the entry is stale — discard it and continue.

```cpp
// Stale-entry check (from aStar.cpp)
if (cur.g != gScore[p.r][p.c]) continue;
```

This is simpler than implementing decrease-key and performs well in practice because the extra entries are discarded cheaply.

---

## 6. Console visualisation

`printOverlay()` in `main.cpp` renders a composite view by building a character grid in layers:

| Layer | Symbol | Meaning |
|---|---|---|
| Base | `.` | Free, unvisited cell |
| Base | `#` | Obstacle |
| Visited | `x` | Explored by A\* but not on the final path |
| Path | `*` | On the optimal path |
| Override | `S` / `G` | Start / goal (always on top) |

The `x` vs `*` distinction is important for understanding the algorithm: it shows exactly which cells A\* considered before committing to the final path.

---

## 7. Design choices and extensibility

| Choice | Reasoning |
|---|---|
| `aStarGrid` class wraps the grid | Encapsulation — bounds checking and obstacle logic live in one place |
| `AStarResult` struct return type | Clean API — all outputs in one value, no out-parameters |
| Static `manhattan()` method | Depends only on two positions, not on grid state — logically belongs as a utility on the grid type |
| `const aStarGrid&` parameter to `aStarSearch` | The algorithm is read-only with respect to the grid; passing by const ref makes this explicit |
| Separate heuristic function | Straightforward to replace Manhattan with another heuristic (Euclidean, Chebyshev, custom weight) without touching the core loop |