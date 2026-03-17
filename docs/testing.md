---
layout: default
title: Testing & Validation
---

# Testing & Validation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Testing strategy

The brief requires testing of both general and edge scenarios, so I separated testing into two levels:

1. **Demo run** via `main.cpp` to show the algorithm visually on the default grid
2. **Unit tests** via `unit_tests.cpp` to verify correctness and error handling in a repeatable way

This is stronger than relying only on screenshots or a manual demo because the same checks can be re-run after every code change.

---

## 2. How the test binary is run

```bash
make test
```

or directly:

```bash
g++ -std=c++17 -O2 -Wall -Wextra -pedantic unit_tests.cpp a_star_grid.cpp a_star.cpp -o astar_tests
./astar_tests
```

The test harness prints a pass/fail line for each case and a summary at the end.

---

## 3. Unit-test output

Current test run:

```text
[PASS] DefaultGridFindsOptimalPath
[PASS] StartEqualsGoalReturnsSingleCellPath
[PASS] NoPathWhenStartIsBoxedIn
[PASS] OpenGridUsesManhattanOptimalCost
[PASS] BlockedGoalRejectedByConstructor
[PASS] JaggedGridRejected

Passed 6 of 6 tests.
```

---

## 4. Individual test cases

### T1 — Default 5×5 grid with obstacles

Purpose: verify the main demonstration case still produces the expected optimal path.

Checks:

- path exists
- path length is 8 moves
- nodes expanded is 10
- path starts at `S`, ends at `G`, moves one orthogonal step at a time, and never enters an obstacle

Result: **Pass**

---

### T2 — Start equals goal

Purpose: verify the early-exit branch.

Checks:

- path exists immediately
- path length is 0 moves
- exactly 1 node is expanded

Result: **Pass**

This is important because it proves the algorithm handles a trivial boundary case correctly rather than doing unnecessary work.

---

### T3 — No path when the start is boxed in

Grid used:

```text
S 1 .
1 1 1
. 1 G
```

Purpose: verify that the search correctly reports failure when the start cannot move anywhere.

Checks:

- no path is found
- exactly 1 node is expanded

Result: **Pass**

---

### T4 — Fully open 5×5 grid

Purpose: verify that the algorithm matches the known Manhattan-optimal cost on an obstacle-free grid.

Checks:

- path exists
- path length is 8 moves from `(0,0)` to `(4,4)`
- nodes expanded is 9 with the current heuristic/tie-break rule
- path remains valid and orthogonal

Result: **Pass**

---

### T5 — Blocked default goal rejected by constructor

Grid used:

```text
0 0
0 1
```

Purpose: verify that invalid grid setups are rejected immediately.

Checks:

- constructing `AStarGrid` throws `std::invalid_argument`

Result: **Pass**

This is a meaningful robustness test because it validates the improved constructor logic rather than only the search itself.

---

### T6 — Jagged grid rejected

Example input:

```text
{0, 0, 0}
{0, 1}
{0, 0, 0}
```

Purpose: ensure malformed non-rectangular input is not accepted.

Checks:

- constructor throws `std::invalid_argument`

Result: **Pass**

---

## 5. Why these tests are useful

The six tests are deliberately mixed:

- **normal success cases**: T1, T4
- **boundary case**: T2
- **search failure case**: T3
- **input validation / robustness cases**: T5, T6

That gives better coverage than testing only “happy path” scenarios.

---

## 6. Manual validation from the demo output

The default console demo still matters because it shows algorithm behaviour visually, not just numerically.

```text
Path found. Path length = 8 moves
Nodes expanded = 10
S * . . .
# * # # .
. * * * #
. x # * #
. # # * G
```

From this output I can manually verify:

- the path is continuous
- no diagonal moves appear
- the path does not cross obstacles
- A\* expands only a subset of the available cells

That visual validation complements the unit tests well.

---

## 7. Remaining testing limitations

The project now has a proper unit-test binary, but there is still room to go further:

- add larger randomised test grids
- compare node counts against BFS for performance evidence
- add automated CI so tests run on every push
- collect line coverage with a dedicated coverage tool

For the current brief, the existing test suite is sufficient and clearly demonstrates both correctness and validation effort.
