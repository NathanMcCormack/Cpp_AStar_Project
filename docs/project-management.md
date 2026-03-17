---
layout: default
title: Project Management
---

# Project Management

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Planning approach

I treated each lab as a **development checkpoint** rather than trying to build the entire project at once. That matched the brief requirement to demonstrate ongoing progress in person.

For each session I tracked three questions:

1. What did I complete?
2. What is the next step?
3. What blockers or risks do I have?

This made the project easier to manage and also created a clear narrative for demonstrations.

---

## 2. Development stages

### Stage 1 — Understand the brief and choose a scope

Initial decisions:

- implement a **grid-based** A\* rather than a generic arbitrary graph
- use **4-direction movement only**
- use **Manhattan distance** as the heuristic
- aim for a clean console demonstration rather than overcomplicating the UI

This gave a realistic scope while still leaving room for good technical depth.

### Stage 2 — Build the grid abstraction

Tasks completed:

- `AStarGrid` class created
- rectangular-grid validation added
- obstacle checks added
- start and goal validation added
- neighbour generation implemented

Main risk at this stage: invalid input causing bugs later in the search.

### Stage 3 — Implement A* search

Tasks completed:

- open set using `std::priority_queue`
- `g_score` and `came_from` tables
- closed set
- stale-entry handling for improved paths
- path reconstruction once goal is reached

Main challenge at this stage: understanding that `std::priority_queue` is a max-heap by default, so the comparator had to be inverted.

### Stage 4 — Add presentation and evidence

Tasks completed:

- console overlay added
- node-expansion counts reported
- sample output documented in GitHub Pages

This made the project much easier to explain during a demonstration because the algorithm behaviour became visible instead of hidden.

### Stage 5 — Strengthen quality

Tasks completed:

- file/module naming cleaned up
- code style made consistent
- `.clang-format` added
- `Makefile` added
- constructor robustness improved
- separate unit-test binary created
- report pages rewritten so they accurately match the code

This stage was important for turning a working prototype into a stronger submission.

---

## 3. Weekly log summary

| Week | Progress made | Next step | Main blocker / decision |
|---|---|---|---|
| W1 | Read brief, chose grid-based A\* scope | Design classes and data flow | Keep project achievable but still strong technically |
| W2 | Built `AStarGrid`, validation, neighbour generation | Add heuristic and search logic | Deciding how much grid logic should live in one class |
| W3 | Implemented A\* search and path reconstruction | Produce visual output | Getting the priority queue ordering correct |
| W4 | Demo working, path and expansion stats visible | Test edge cases | Avoiding hidden bugs in invalid grids and reconstruction |
| W5 | Added unit tests, formatting rules, Makefile, stronger report pages | Final review | Making sure report, code, and output all match exactly |

---

## 4. Risk log

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Wrong priority-queue ordering | Medium | High | Wrote custom comparator and verified using known outputs |
| Invalid grid silently accepted | Medium | Medium | Added constructor validation for blocked default start/goal |
| Reconstruction loop on bad parent chain | Low | High | Used sentinel parent value and defensive check |
| Report and code drifting out of sync | Medium | Medium | Re-ran builds/tests and updated pages only after verifying outputs |
| Style inconsistency across files | Medium | Low | Applied one naming/formatting convention and added `.clang-format` |

---

## 5. Milestones

| Milestone | Description | Status |
|---|---|---|
| M1 | Grid abstraction with validation and neighbour generation | ✅ Complete |
| M2 | A\* search returns a correct shortest path | ✅ Complete |
| M3 | Console visualisation of path and explored cells | ✅ Complete |
| M4 | Separate unit-test binary with general and edge cases | ✅ Complete |
| M5 | GitHub Pages report aligned to the final codebase | ✅ Complete |

---

## 6. What this section demonstrates

This section shows that the project was built iteratively rather than appearing as a single final dump. That matters for the brief because the code quality, testing, and report quality all came from a sequence of improvements:

- first make it work
- then make it correct
- then make it robust
- then make it presentable

That development process is a real part of the submission quality, not just background detail.
