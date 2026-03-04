
---

## `docs/algorithm.md`
```md
---
layout: default
title: Algorithm & Heuristic
---

# Algorithm & Heuristic

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

## 1. A* (A-Star) in one page
A* searches for the lowest-cost route from **Start** to **Goal** by ranking candidate nodes using:

- `g(n)` = cost from start to node `n`
- `h(n)` = heuristic estimate from `n` to goal
- `f(n) = g(n) + h(n)` = estimated total path cost

A* repeatedly:
1. Pops the node with smallest `f` from the **open set**
2. Marks it visited (moves it to **closed set**)
3. Relaxes its neighbours (updates best-known `g`, stores parent/came-from)

### A* pseudocode (high-level)
```text
open_set = {start}
g[start] = 0
parent[start] = none

while open_set not empty:
    current = node in open_set with min f = g + h
    if current == goal:
        return reconstruct_path(parent, goal)

    move current from open_set to closed_set

    for neighbour in valid_neighbours(current):
        if neighbour is obstacle or in closed_set: continue
        tentative_g = g[current] + step_cost

        if neighbour not in open_set OR tentative_g < g[neighbour]:
            parent[neighbour] = current
            g[neighbour] = tentative_g
            f[neighbour] = g[neighbour] + h(neighbour)
            add neighbour to open_set (if needed)

return "no path"