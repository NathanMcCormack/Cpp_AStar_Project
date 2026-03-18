---
layout: default
title: Algorithm & Heuristic
---

# Algorithm & Heuristic

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) 

[Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. What A* does

A\* is a **best-first graph search algorithm** that finds a shortest path between a start node and a goal node. It is widely used because it combines two desirable properties:

- it is **complete** for this problem setting: if a path exists, it will find one
- it is **optimal** when the heuristic is admissible and edge costs are non-negative

In this project, every move costs 1, so the total path cost is just the number of orthogonal steps taken.

The ranking function used by A\* is:

```text
f(n) = g(n) + h(n)
```

| Term | Meaning |
|---|---|
| `g(n)` | exact cost from the start to node `n` |
| `h(n)` | estimated cost from node `n` to the goal |
| `f(n)` | estimated total cost of a path through `n` |

The algorithm repeatedly expands the open-set node with the **smallest `f` value**.

---

## 2. Why A* instead of BFS or Dijkstra

### BFS

Breadth-First Search is correct for an unweighted grid, but it expands outward level by level in all directions. It has no awareness of the goal location, so it often explores many unnecessary cells.

### Dijkstra

Dijkstra's algorithm generalises BFS to weighted graphs. It is also correct here, but without a heuristic it still expands nodes more broadly than necessary.

### A*

A\* keeps Dijkstra's exact path-cost tracking (`g`) but adds a heuristic (`h`) to bias the search toward the goal. That gives the best balance for this project:

- still optimal
- still complete
- usually fewer node expansions

If `h(n) = 0` for all nodes, A\* becomes Dijkstra's algorithm. This makes A\* a strict improvement when a good admissible heuristic is available.

---

## 3. Heuristic choice: Manhattan distance

The heuristic used is **Manhattan distance**:

```text
h(n) = |row_n - row_goal| + |col_n - col_goal|
```

This is the number of horizontal plus vertical moves needed **if there were no obstacles**.

### Why it is admissible

On a 4-direction grid with unit step cost, no valid path can ever be shorter than the Manhattan distance, because every move changes either the row or the column by exactly 1. Obstacles may force the actual path to be **longer**, but never shorter.

That means Manhattan distance is a **lower bound** on the true remaining cost, so it never over-estimates.

### Why it is consistent

For any node `n` and neighbour `n'`, moving one step changes the Manhattan distance by at most 1. Therefore:

```text
h(n) ≤ cost(n, n') + h(n')
```

Since `cost(n, n') = 1` in this project, the consistency condition holds. That matters because once a node is closed, its best `g` value is already known and it does not need to be re-opened.

---

## 4. Why Manhattan is a better fit than Euclidean here

Euclidean distance is also admissible on a 4-direction grid, but it is a looser estimate than Manhattan distance because it measures straight-line distance through continuous space. In a grid where we are not allowing diagonla movement, that estimate is less informative.

Manhattan distance is a better match because:

- it reflects the legal movement model exactly
- it is cheaper to compute than Euclidean distance
- it usually causes fewer unnecessary expansions

So Manhattan distance is both theoretically sound and practically better for this implementation.

---

## 5. A* pseudocode for this project

```text
open_set ← priority queue ordered by lowest f
closed   ← false for every cell
g[start] ← 0
parent[start] ← none
push start into open_set

while open_set not empty:
    current ← pop lowest-f node

    if current is stale: skip
    if current already closed: skip

    close current

    if current == goal:
        reconstruct path and return

    for each 4-direction neighbour:
        tentative_g ← g[current] + 1

        if tentative_g < g[neighbour]:
            parent[neighbour] ← current
            g[neighbour] ← tentative_g
            f ← tentative_g + h(neighbour)
            push neighbour into open_set

return no path
```

The key implementation detail is the **stale-entry check**. Since `std::priority_queue` does not support decrease-key, improved nodes are pushed again instead. Old entries are ignored later when their stored `g` no longer matches the best known table entry.

---

## 6. Tie-breaking rule

If two open-set nodes have the same `f` value, this implementation prefers the node with the **smaller `h` value**.

```cpp
if (left.f_score != right.f_score) {
  return left.f_score > right.f_score;
}
return left.h_score > right.h_score;
```

This does not change optimality, but it makes the search appear more direct because it favours nodes physically closer to the goal.

---

## 7. Complexity discussion

Let `V` be the number of cells and `E` be the number of valid grid edges.

- In the worst case, A\* still behaves similarly to Dijkstra and can explore much of the grid.
- With a good heuristic such as Manhattan distance, the practical number of expansions is much lower.
- Each push/pop on the open set costs `O(log V)` because it is implemented with a priority queue.

For this project, the main performance observation is not asymptotic complexity alone, but **how many nodes were actually expanded**. That is why node-expansion counts are reported in both the demo and the tests.

---

## 8. Limitations of the current algorithm design

The current version assumes:

- a 2D rectangular grid
- uniform movement cost (every step costs 1)
- no diagonal movement
- static obstacles only

These are reasonable simplifications for the brief, but extending to weighted terrain or diagonal motion would require a different heuristic and slightly different neighbour logic.
