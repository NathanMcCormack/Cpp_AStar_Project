---
layout: default
title: Reflection & Next Steps
---

# Reflection & Next Steps

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

## 1. Biggest problems encountered (and how I solved them)
### Problem A — Off-by-one / indexing confusion
When mapping “last cell” coordinates, I initially had to reinforce 0-based indexing:
- last row index is `rows-1`
- last column index is `cols-1`

**Fix:** treat all grid access consistently as `grid[row][col]` with `0 <= row < rows`.

### Problem B — Priority queue ordering
A* depends on consistently selecting the lowest `f=g+h`.
**Fix:** define a clear comparator and (optionally) a tie-break rule.

### Problem C — Path reconstruction
Even when the search reaches the goal, the final path must be reconstructed using parent links.
**Fix:** store `parent[child] = current` whenever a better `g` is found, then walk backwards from goal.

## 2. Limitations (current)
- Single heuristic (Manhattan only)
- Demonstration grids currently limited (need edge-case evidence)
- No file loading for grids yet (manual setup)

## 3. What I’d improve next
- Add edge-case tests + screenshots in Testing page
- Add support to load grid from file (e.g., CSV or simple text format)
- Optional: compare heuristics (Manhattan vs Euclidean vs Dijkstra/zero heuristic)
- Optional: add weighted tiles (turn into weighted A*)

## 4. Learning outcomes
- Improved understanding of heuristic search and why admissibility matters
- Practical experience with STL containers (priority queue, vector, maps/sets)
- Better debugging discipline for grid algorithms (bounds, neighbours, parents)