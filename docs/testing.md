
---

## `docs/testing.md`
```md
---
layout: default
title: Testing & Validation
---

# Testing & Validation

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

## 1. Testing approach
Testing is based on:
- Correctness: path length + valid route
- Behaviour: nodes expanded / explored pattern
- Robustness: edge cases (no path, start==goal, blocked start/goal)

## 2. Current tested case (evidence)
### Test A — Provided grid with obstacles
**Input grid**
S 0 0 0 0  
0 1 1 1 0  
0 0 0 0 1  
0 0 1 0 1  
0 1 1 0 G  

**Observed output**
- Path found. Path length = 8 moves
- Nodes expanded = 12

S . . . .  
* # # # .  
* * * * #  
x x # * #  
x # # * G  

**Validation notes**
- The path respects the **no diagonal** rule.
- The path does not pass through obstacles.
- The length is consistent with the corridor structure created by obstacles.

## 3. Test matrix (add evidence as you run them)
| Test ID | Scenario | Expected result | Status | Evidence |
|---|---|---|---|---|
| T1 | Start == Goal | length = 0, expanded small | Planned | screenshot/output |
| T2 | No path exists | clean “no path” output | Planned | screenshot/output |
| T3 | Fully open grid | near-direct Manhattan path | Planned | screenshot/output |
| T4 | Start surrounded | “no path” or minimal expansion | Planned | screenshot/output |
| T5 | Goal surrounded | “no path” | Planned | screenshot/output |
| T6 | Narrow corridor maze | correct shortest, higher expansions | Planned | screenshot/output |

## 4. Optional performance notes (if measured)
If performance is measured later:
- record grid size (N x M)
- record nodes expanded
- record runtime (ms)
- compare heuristic variants (optional extension)