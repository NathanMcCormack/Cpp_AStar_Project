---
layout: default
title: Design & Implementation
---

# Design & Implementation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html)

[Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

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
| `a_star.h/.cpp` | Search result type and A* implementation |
| `main.cpp` | Builds a demo grid, runs the search, prints a visual overlay |
| `unit_tests.h/.cpp` | Test harness and unit tests |

---

## 3. Class diagram and object-oriented design

The project was designed using a modular object-oriented structure rather than placing all logic in a single file. This makes the code easier to read, test, maintain, and explain.

The main modules are:

- **`AStarGrid`** — stores the map, start node, goal node, validation rules, neighbour generation, and helper functions such as Manhattan distance
- **`RunAStarSearch` / `AStarResult`** — perform the search and package the result cleanly
- **`main.cpp`** — demonstrates the final program behaviour and console visualisation
- **`unit_tests.cpp`** — verifies expected behaviour using repeatable test cases

When the class diagram is exported, it should be placed here in:

`docs/assets/images/class-diagram.png`

<!-- Uncomment when the image is added:
![Class Diagram](assets/images/class-diagram.png)
-->

This design follows **separation of concerns**:
- the grid class manages data and movement rules
- the search logic manages the pathfinding process
- the main program handles demonstration output
- the test file handles verification

That separation improved clarity and made later extensions easier.

---

## 4. `AStarGrid`: representing the environment

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

## 5. Validation strategy

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

This is used by both `SetStart()` and `SetGoal()`, and it is also called by the constructor so that a custom grid cannot create an invalid default start or goal. That was an important robustness improvement over the earlier version.

---

## 6. Neighbour generation

Neighbour generation is isolated in `Neighbours4()`.

A simplified version of the movement directions is shown below:

```cpp
constexpr std::array<AStarGrid::Position, 4> kDirections = {
    AStarGrid::Position{-1, 0},
    AStarGrid::Position{1, 0},
    AStarGrid::Position{0, -1},
    AStarGrid::Position{0, 1}
};
```

The function returns only valid orthogonal neighbours that are:

- in bounds
- not blocked by an obstacle

This keeps all movement rules in one place. If the project were later extended to support diagonals or weighted terrain, this is one of the first places that would need to change.

---

## 7. `AStarResult`: packaging search output cleanly

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

## 8. Core A* data structures

### 8.1 Open set

```cpp
std::priority_queue<OpenNode,
                    std::vector<OpenNode>,
                    OpenNodeGreater> open_set;
```

The open set stores candidate frontier nodes. It is implemented with `std::priority_queue` for efficient extraction of the lowest-ranked node.

### 8.2 `g_score`

```cpp
std::vector<std::vector<int>> g_score(
    row_count, std::vector<int>(col_count, kInfinity));
```

This stores the best known exact path cost from the start to every cell.

### 8.3 `came_from`

```cpp
std::vector<std::vector<Position>> came_from(
    row_count, std::vector<Position>(col_count, kNoParent));
```

This stores the parent of each cell on the best known route so the final path can be reconstructed by walking backward from the goal.

### 8.4 Closed set

A 2D table records whether a node has already been fully expanded. Because Manhattan distance is consistent for this problem, closed nodes do not need to be reopened.

---

## 9. Stale-entry handling

A common practical issue with A* in C++ is that `std::priority_queue` has no **decrease-key** operation. If a better path to a node is found, the code cannot edit the old queue entry in place.

The solution used here is **lazy deletion**:

- push the improved entry into the queue
- leave the old entry where it is
- when the old entry is eventually popped, ignore it because its stored `g_score` no longer matches the best known table value

This keeps the implementation simple, correct, and efficient enough for the project scope.

---

## 10. Path reconstruction

Once the goal is reached, the path is rebuilt by following `came_from` links backward:

1. start at the goal
2. repeatedly move to its parent
3. stop when the start is reached
4. reverse the collected sequence

A sentinel position `{-1, -1}` is used to detect an invalid parent chain. This is mainly defensive programming, but it prevents a silent infinite loop if reconstruction data were ever corrupted.

---

## 11. Development process

### Step 1: test the base grid and helper output

The first milestone was representing the environment correctly and making sure the start, goal, and blocked cells printed in the right positions.

This early debug stage also printed the Manhattan distance and the walkable neighbours of the start node. That was useful because the project only allows **4-directional movement**, so both the heuristic and neighbour logic needed to match that rule exactly.

![Grid, Manhattan Distance, and Neighbour Debugging](/workspaces/C-_A-_Project/docs/assets/images/ManhattanStep1.png)

I also kept a screenshot of the supporting `main.cpp` debug code used during this stage.

![Debug Code in main.cpp](/workspaces/C-_A-_Project/docs/assets/images/MainCppManhattanStep1.png)

### Step 2: verify blocked-grid behaviour

A correct pathfinding program must also handle failure cases properly. I created a blocked map where the goal could not be reached and verified that the program reported **No path found** instead of inventing an invalid route.

![No Path Found Output](/workspaces/C-_A-_Project/docs/assets/images/ManhattanDistanceNoPathStep2.png)

This stage improved robustness and helped validate the defensive logic in the implementation.

### Step 3: run A* on a solvable grid

Once the grid, heuristic, and neighbour logic were all working, the complete A* search was run on a solvable map.

The program successfully:

- found a path from `S` to `G`
- reported the path length in moves
- reported the number of expanded nodes
- displayed the explored route visually

![Successful A* Output](/workspaces/C-_A-_Project/docs/assets/images/ManhattanStep2.png)

### Step 4: produce the final visual overlay

The final stage improved presentation by overlaying the search result onto the grid using distinct symbols:

- `*` for the final path
- `x` for expanded cells that are not part of the final path
- `#` for obstacles
- `S` and `G` for the endpoints

This made the final result easier to explain during demonstration and helped show how the algorithm explored the search space.

![Final Overlay Visualisation](assets/images/final-overlay.png)

---

## 12. Console visualisation

The final output layer is intentionally kept in `main.cpp`, not inside the search logic. That separation matters:

- the algorithm module focuses on correctness
- the demo/output layer focuses on presentation

The overlay uses:

- `*` for the final path
- `x` for expanded cells not on the path
- `#` for obstacles
- `S` and `G` for the endpoints

---

## 13. Unit testing support

To improve correctness and maintainability, I added a separate unit-test source/header pair rather than relying only on manual checking in `main.cpp`.

The unit tests verify core behaviours such as:

- Manhattan distance calculation
- neighbour generation indirectly through valid path structure
- path found on a solvable map
- correct handling of a blocked map
- validation of invalid grids
- validation of invalid start/goal assumptions in the constructor

---

## 14. Style decisions applied in the code

The project follows a consistent style influenced by the Google C++ Style Guide, the C++ Core Guidelines, and K&R-style brace formatting:

- **self-contained headers** with include guards
- **PascalCase** for types and major functions
- **snake_case** for variables and data members
- **private member trailing underscores**
- **symbolic constants** such as `kStepCost`, `kInfinity`, and `kNoParent`
- **no raw `new` / `delete`**
- comments used for non-obvious logic rather than obvious line-by-line narration

