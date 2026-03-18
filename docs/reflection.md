---
layout: default
title: Reflection & Next Steps
---

# Reflection & Next Steps

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html)

[Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Biggest problems encountered

### 1.1 Getting the movement logic correct

One of the first implementation challenges was making sure that movement matched the rules of the project. Because this version of A* only allows **4-directional movement** (up, down, left, right), the neighbour generation function had to return only valid orthogonal cells and ignore blocked or out-of-bounds positions.

I addressed this by:

- isolating the neighbour logic in a dedicated `Neighbours4()` function
- testing it separately before running the full search
- checking that only valid walkable neighbours were returned

### 1.2 Handling no-path cases correctly

Another important issue was making sure the program behaved correctly when no valid route existed. A pathfinding program can appear to work well on a simple demo grid, but it also needs to fail cleanly when the goal is unreachable.

I addressed this by:

- creating a blocked test grid where no path should exist
- checking that the program returned **No path found**
- ensuring that no invalid route was printed in this case

This improved the robustness of the solution and showed that the implementation handles edge cases as well as normal cases.

### 1.3 Keeping the report aligned with the actual code

One of the most important clean-up tasks in this project was making sure that the documentation matched the finished implementation.

I addressed this by checking that:

- file names in the report matched the real codebase
- testing claims matched the actual unit test files
- screenshots and output examples matched the current executable

This improved the professional quality of the final submission.

---

## 2. What worked well

Several design decisions worked particularly well:

### Clear module boundaries

Splitting the project into grid logic, search logic, demo output, and tests made the system easier to reason about. When I needed to improve validation, for example, that work stayed mostly inside `AStarGrid`.

### Separate unit tests

Adding a dedicated test binary strengthened the project significantly.

---

## 3. What I learned

This project improved my understanding of several C++ and algorithmic topics:

- how A\* balances exact cost and heuristic guidance
- why admissibility and consistency matter in practice, not just theory
- how to structure a small C++ program into reusable modules
- why defensive validation and edge-case testing improve code quality

---

## 4. What I would do differently next time

If I restarted the project from the beginning, I would do three things earlier:

1. **Add the test harness sooner** so regressions are caught immediately.
2. **Decide naming/style rules at the start** rather than cleaning them up later.
3. **Keep the report live while coding** so documentation and implementation cannot drift apart.

None of these changed the algorithm itself, but they would have made the process smoother.

---

## 5. Next steps / future improvements

If this project were developed further, the most useful next steps would be:

### Weighted terrain

Allow certain cells to cost more than others and adapt the algorithm to handle weighted movement. This would make the project closer to real navigation problems.

### Alternative heuristics

Support diagonal movement and compare Manhattan, Euclidean, and Chebyshev heuristics depending on the movement model.

### CI-based testing

Add a GitHub Actions workflow so tests run automatically on every push.

