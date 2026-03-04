
---

## `docs/implementation.md`
```md
---
layout: default
title: Design & Implementation
---

# Design & Implementation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

## 1. Grid representation
The program represents the environment as a 2D grid:

- `0` = walkable cell
- `1` = obstacle cell

Start and goal are stored as coordinates `(row, col)`.  
Because indexing is **0-based**, the bottom-right cell is `(rows-1, cols-1)`.

### Listing 1 — 4-direction neighbour offsets
```cpp
static constexpr int kDirs[4][2] = {
  {-1, 0}, // up
  { 1, 0}, // down
  { 0,-1}, // left
  { 0, 1}  // right
};