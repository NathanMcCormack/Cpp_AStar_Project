---
layout: default
title: Project Management
---

# Project Management

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Planning approach

This project was developed iteratively, with each lab session used as a structured checkpoint. Progress was tracked using the three-question format from the brief:

1. What did I complete?
2. What is my next step?
3. Any blockers?

GitHub was used for version control, with commits made after each working milestone so that progress is traceable and the report site can be hosted via GitHub Pages.

---

## 2. Weekly development log

| Week | What I completed | Next step | Blockers / risks | Notes |
|---|---|---|---|---|
| W1 | Read the brief; chose C++ with `std::priority_queue`; sketched class design on paper | Design `aStarGrid` class with `Pos` struct | Deciding struct vs class for `Pos` | Chose struct — plain data with no invariants, no need for class |
| W2 | Implemented `aStarGrid`: constructor, `validateRectangular`, `inBounds`, `isObstacle`, `neighbors4`, `printGrid` | Implement Manhattan heuristic and wire it to the search | Priority queue ordering (min vs max heap) | Manhattan added as a static method — belongs logically on the grid type |
| W3 | Implemented `aStarSearch`: open set, closed set, gScore table, stale-entry check | Path reconstruction + console overlay | `cameFrom` initialisation causing loops in edge cases | Fixed with `{-1,-1}` sentinel and safety check |
| W4 | Path reconstruction working; `printOverlay` added to main.cpp; default demo running | Edge-case testing | Time | Tests added: start==goal, no path, open grid, corridor |
| W5 | All 6 tests passing; report pages written; code comments added | Final review and submission | — | — |

---

## 3. Milestones

| Milestone | Description | Status |
|---|---|---|
| M1 | Grid class with validation, neighbour generation, and Manhattan heuristic | ✅ Complete |
| M2 | A\* search producing correct paths on default grid | ✅ Complete |
| M3 | Path reconstruction and console visualisation | ✅ Complete |
| M4 | Edge-case test suite (6 tests, all passing) | ✅ Complete |
| M5 | Report written across all pages with code snippets and evidence | ✅ Complete |

---

## 4. Risk log

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Priority queue ordering bug | Medium | High — wrong paths | Carefully verified comparator with worked example |
| Infinite loop in path reconstruction | Low | High — program hangs | Sentinel `{-1,-1}` + safety break added |
| Edge case crashes (empty grid, blocked start) | Medium | Medium — crash / exception | `validateRectangular` + early guard in `setStart`/`setGoal` |
| Heuristic inadmissibility if diagonals added | Low | High — suboptimal paths | Documented decision to restrict to 4-direction movement |