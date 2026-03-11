---
layout: default
title: Testing & Validation
---

# Testing & Validation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Testing approach

Testing covers three dimensions:

- **Correctness** — does the path have the right length? Does it stay on walkable cells?
- **Behaviour** — are the explored cells and node-expansion counts sensible?
- **Robustness** — does the program handle edge cases without crashing or producing wrong output?

All tests were run from the compiled binary (`g++ -std=c++17 -O2`) and output was captured manually.

---

## 2. Test results

### T1 — Default 5×5 grid with obstacles

**Grid** (S = start, G = goal, 1 = obstacle):

```
S 0 0 0 0
1 0 1 1 0
0 0 0 0 1
0 0 1 0 1
0 1 1 0 G
```

**Output:**
```
Path found. Path length = 8 moves
Nodes expanded = 10

S * . . .
# * # # .
. * * * #
. x # * #
. # # * G
```

**Validation:**
- Path of 8 moves is optimal — the obstacles force a specific corridor.
- No diagonal moves used (4-direction constraint respected).
- Path does not pass through any `#` cell.
- Only 10 of 25 cells expanded — the heuristic is directing search efficiently.

---

### T2 — Start equals Goal

```
Grid: default 5×5, setStart({0,0}), setGoal({0,0})
```

**Output:**
```
Path found. Path length = 0 moves
Nodes expanded = 1
```

**Validation:** The early-exit branch fires immediately and returns a single-cell path with no search. Correct.

---

### T3 — No path exists

**Grid** (start surrounded by a wall of obstacles):

```
S 1 .
1 1 1
. 1 G
```

**Output:**
```
No path found. Nodes expanded = 1
```

**Validation:** Start is surrounded on both reachable sides. The algorithm exhausts the open set with only 1 expansion (start itself) and correctly reports no path. No crash.

---

### T4 — Fully open 5×5 grid

**Grid:** All zeros, start = (0,0), goal = (4,4).

**Output:**
```
Path found. Path length = 8 moves
Nodes expanded = 9
```

**Validation:** The Manhattan-optimal path from (0,0) to (4,4) is 8 moves (4 right + 4 down in any order). Only 9 nodes expanded on a 25-cell grid — the heuristic guides search almost directly to the goal, expanding barely any off-path cells.

---

### T5 — Start surrounded (2-side block)

**Grid:**
```
S 1 .
1 . .
. . G
```

**Output:**
```
No path found. Nodes expanded = 1
```

**Validation:** Start at (0,0) has obstacles at (0,1) and (1,0) — both accessible neighbours blocked. Algorithm correctly reports no path immediately.

---

### T6 — Narrow corridor maze

**Grid** (goal at top-right, path must snake down and around):

```
S 1 . . G
. 1 . 1 .
. 1 . 1 .
. . . 1 .
. 1 1 1 .
```

**Output:**
```
Path found. Path length = 10 moves
Nodes expanded = 11
```

**Validation:**
- Path correctly navigates the corridor (down left side, across bottom, up right side).
- 10-move path is optimal for this layout.
- 11 nodes expanded on a 25-cell grid — still very focused despite the complex layout.

---

## 3. Full test matrix

| ID | Scenario | Expected | Actual path length | Actual nodes expanded | Pass? |
|---|---|---|---|---|---|
| T1 | Default 5×5 with obstacles | Path found, length 8 | 8 | 10 | ✅ |
| T2 | Start == Goal | Path length 0, expand 1 | 0 | 1 | ✅ |
| T3 | No path (completely surrounded) | No path found | — | 1 | ✅ |
| T4 | Fully open 5×5 | Path found, length 8 | 8 | 9 | ✅ |
| T5 | Start 2-side blocked | No path found | — | 1 | ✅ |
| T6 | Narrow corridor | Path found, length 10 | 10 | 11 | ✅ |

All 6 tests pass.

---

## 4. What the node-expansion counts reveal

| Test | Grid cells | Nodes expanded | % explored |
|---|---|---|---|
| T1 (obstacles) | 25 | 10 | 40% |
| T4 (fully open) | 25 | 9 | 36% |
| T6 (corridor maze) | 25 | 11 | 44% |

Even on the most complex layout, A\* explored fewer than half the cells. This contrasts with BFS, which would expand cells radiating outward in all directions and would likely visit the majority of the grid before finding the goal.