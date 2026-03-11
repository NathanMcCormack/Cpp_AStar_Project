---
layout: default
title: Algorithm & Heuristic
---

# Algorithm & Heuristic

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. What is A*?

A\* is a **best-first search algorithm** that finds the shortest path between two nodes in a graph. It is widely used in game AI, robotics, and navigation because it is both **complete** (always finds a path if one exists) and **optimal** (finds the shortest one), provided the heuristic satisfies the admissibility condition.

A\* ranks every candidate node using the evaluation function:

> **f(n) = g(n) + h(n)**

| Symbol | Meaning |
|---|---|
| `g(n)` | The known, exact cost from the start node to node `n` |
| `h(n)` | A heuristic estimate of the cost from `n` to the goal |
| `f(n)` | The estimated total cost of the cheapest path through `n` |

The algorithm always expands the open-set node with the **lowest f value** first.

---

## 2. Why A* beats plain BFS or Dijkstra

- **BFS** (Breadth-First Search) expands nodes level by level — it finds the shortest path on unweighted grids but explores in all directions equally, wasting effort on cells moving away from the goal.
- **Dijkstra's algorithm** is like BFS with edge weights — still explores in all directions, just with cost-awareness.
- **A\*** adds the heuristic `h(n)` to guide the search *toward* the goal, dramatically reducing the number of nodes expanded.

In the limit where `h(n) = 0` for all nodes, A\* degenerates to Dijkstra's algorithm. In the limit where `g(n) = 0`, it becomes a pure greedy best-first search. A\* balances both.

---

## 3. The heuristic: Manhattan distance

For this project, movement is **4-directional** (up, down, left, right — no diagonals). The chosen heuristic is **Manhattan distance**:

> **h(n) = |n.row − goal.row| + |n.col − goal.col|**

### Why Manhattan distance?

**Admissibility:** A heuristic is admissible if it *never over-estimates* the true cost to the goal. With 4-direction movement and unit step cost, the minimum number of moves from any cell to the goal is exactly its Manhattan distance — you cannot do it in fewer. Therefore Manhattan distance never over-estimates, making it admissible.

**Consistency (monotonicity):** For any node `n` and neighbour `n'`, `h(n) ≤ cost(n, n') + h(n')`. Manhattan distance satisfies this because moving one step changes the Manhattan distance by exactly 1. Consistency implies admissibility and additionally guarantees that once a node is expanded its `g` value is already optimal — so we never need to re-open closed nodes.

### Why not Euclidean distance?

Euclidean distance (`√(Δr² + Δc²)`) is also admissible for 4-direction grids (it under-estimates even more aggressively than Manhattan), but it requires a floating-point square root and may under-estimate so much that A\* explores more cells than necessary. Manhattan is the tightest admissible heuristic for 4-direction grids, making it the best choice here.

---

## 4. A* pseudocode

```text
open_set  ← {start}               // min-heap ordered by f = g + h
g[start]  ← 0
parent[start] ← none

while open_set is not empty:
    current ← node in open_set with min f

    if current == goal:
        return reconstruct_path(parent, goal)

    move current to closed_set

    for each neighbour of current:
        if neighbour is obstacle or in closed_set: skip

        tentative_g ← g[current] + step_cost(current, neighbour)

        if tentative_g < g[neighbour]:
            parent[neighbour]  ← current
            g[neighbour]       ← tentative_g
            f[neighbour]       ← tentative_g + h(neighbour)
            add neighbour to open_set

return "no path found"
```

---

## 5. Tie-breaking

When multiple nodes share the same `f` value, the order in which they are expanded can affect the shape (though not the length) of the final path. This implementation breaks ties by preferring the node with the **smaller h value** — i.e. the one physically closer to the goal. This produces more direct-looking paths without sacrificing optimality.

```cpp
// From aStar.cpp — the priority queue comparator
if (a.f != b.f) return a.f > b.f;   // lower f = higher priority
return a.h > b.h;                    // tie-break: lower h wins
```

---

## 6. Optimality guarantee

Because Manhattan distance is **admissible and consistent**, A\* with this heuristic is guaranteed to:
1. Find a path if one exists (completeness)
2. Return a path of minimum length (optimality)
3. Never re-expand a node once it has been closed (efficiency)

These guarantees are proved in the original Hart, Nilsson & Raphael (1968) paper.