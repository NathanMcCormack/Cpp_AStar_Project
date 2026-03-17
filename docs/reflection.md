---
layout: default
title: Reflection & Next Steps
---

# Reflection & Next Steps

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Biggest problems encountered

### 1.1 Understanding the priority queue behaviour

The first major issue was that `std::priority_queue` is a **max-heap by default**, but A\* needs quick access to the node with the **lowest** `f` score. Early on, this was easy to get wrong because the comparator logic is inverted compared with natural reading.

I addressed this by:

- isolating the comparator in its own small struct
- checking the ordering logic against expected A\* behaviour
- documenting the tie-break rule clearly in both the code and the report

That made the implementation easier to trust and easier to explain.

### 1.2 Making the code robust rather than only functional

The second issue was that a search algorithm can appear to work on the default demo grid while still being fragile on invalid input. A good example was the custom-grid constructor: if the bottom-right cell was blocked, the earlier design still accepted it and only failed later in a less clear way.

I fixed that by adding constructor-level validation for the default start and goal positions. This was a useful reminder that **robustness is part of correctness**, not something extra.

### 1.3 Keeping the report aligned with the actual code

A report becomes weak very quickly if it claims tests, outputs, or design features that the final code does not actually contain. One of the more important clean-up tasks in this project was making sure that:

- file names in the report matched the codebase
- testing claims matched a real test binary
- output examples matched the current executable

That alignment improved the professional quality of the submission.

---

## 2. What worked well

Several design decisions worked particularly well:

### Clear module boundaries

Splitting the project into grid logic, search logic, demo output, and tests made the system easier to reason about. When I needed to improve validation, for example, that work stayed mostly inside `AStarGrid`.

### Manhattan distance choice

Restricting movement to four directions made the heuristic decision straightforward and defensible. The theoretical explanation and the observed node-expansion counts both support this choice.

### Console overlay

The overlay turned out to be very valuable. Even though it is simple, it makes the search behaviour visible and makes the project much easier to demonstrate to a lecturer.

### Separate unit tests

Adding a dedicated test binary strengthened the project significantly. It moved validation from “I ran the demo and it looked right” to repeatable checks that can be rerun after every code change.

---

## 3. What I learned

This project improved my understanding of several C++ and algorithmic topics:

- how A\* balances exact cost and heuristic guidance
- why admissibility and consistency matter in practice, not just theory
- how to structure a small C++ program into reusable modules
- how STL containers such as `std::priority_queue` influence design decisions
- why defensive validation and edge-case testing improve code quality

It also reinforced a broader lesson: a strong submission is not only about getting the core algorithm to run. It is also about making the code **explainable, testable, and maintainable**.

---

## 4. What I would do differently next time

If I restarted the project from the beginning, I would do three things earlier:

1. **Add the test harness sooner** so regressions are caught immediately.
2. **Decide naming/style rules at the start** rather than cleaning them up later.
3. **Keep the report live while coding** so documentation and implementation cannot drift apart.

None of these changed the algorithm itself, but they would have made the process smoother.

---

## 5. Current limitations

The final version is strong for the project brief, but it still has practical limitations:

- grids are still created in code rather than loaded from a file
- all movement costs are uniform
- there is no comparison against BFS or Dijkstra in code
- the project is console-based only
- the tests are not yet wired into CI

These are reasonable boundaries for the scope of the assignment, but they are clear improvement opportunities.

---

## 6. Next steps / future improvements

If this project were developed further, the most useful next steps would be:

### Weighted terrain

Allow certain cells to cost more than others and adapt the algorithm to handle weighted movement. This would make the project closer to real navigation problems.

### Alternative heuristics

Support diagonal movement and compare Manhattan, Euclidean, and Chebyshev heuristics depending on the movement model.

### File-based map loading

Read grids from text files so many scenarios can be tested without recompiling.

### Performance comparison

Implement BFS or Dijkstra alongside A\* and compare node-expansion counts and runtime across the same maps.

### CI-based testing

Add a GitHub Actions workflow so tests run automatically on every push.

---

## 7. Final reflection

The most important outcome of the project is not just that the algorithm works. It is that I can now explain:

- why the algorithm is correct
- why this heuristic was chosen
- how the data structures interact
- what the main edge cases are
- how the implementation was improved over time

That depth of understanding is what turns a working program into a strong academic submission.
