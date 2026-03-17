---
layout: default
title: AI & Academic Integrity
---

# AI & Academic Integrity

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Principle followed

AI tools were used as a **support tool**, not as a substitute for understanding. Any AI-assisted material that influenced the project was:

- reviewed critically
- adapted to fit my own design choices
- compiled and tested in my own project
- kept only if I could explain it clearly in person

This follows the brief requirement that AI use must be limited, adapted, and fully understood.

---

## 2. What AI was used for

AI support was used in four bounded ways:

1. **Concept clarification**
   - understanding admissibility, consistency, and tie-breaking in A\*
   - understanding why `std::priority_queue` behaves as a max-heap by default

2. **Code review / cleanup support**
   - reviewing naming consistency and formatting
   - identifying robustness gaps such as invalid default start/goal positions
   - suggesting sensible unit-test scenarios

3. **Report drafting support**
   - improving wording and structure of GitHub Pages sections
   - helping keep explanations concise and technically accurate

4. **Reflection support**
   - helping organise what problems were encountered and how they were solved

---

## 3. What AI was not trusted to do blindly

The following still required my own judgement:

- deciding the project scope
- choosing 4-direction movement
- choosing Manhattan distance
- checking whether suggested code actually compiled
- checking whether outputs matched the report
- checking whether test expectations were correct
- deciding what was honest and appropriate to include in the final submission

In other words, AI suggestions were treated as **proposals to verify**, not as automatically correct answers.

---

## 4. Traceability examples

| Area | Example of AI-assisted support | What I actually kept |
|---|---|---|
| Theory | Clarified the difference between admissible and consistent heuristics | Rewrote the explanation in my own report wording |
| STL usage | Reviewed how to invert `std::priority_queue` ordering | Implemented and verified my own comparator |
| Robustness | Highlighted that a blocked default goal should be rejected early | Added constructor validation and tested it |
| Testing | Suggested edge cases worth checking | Created a separate `unit_tests.cpp` binary and validated expected results |
| Report | Helped improve structure and phrasing | Kept only the parts that matched the final code exactly |

---

## 5. Why this still represents my own understanding

The strongest evidence that the work is genuinely understood is that I can explain:

- what each file is responsible for
- why Manhattan distance is valid here
- why the stale-entry check works
- why the constructor validation was added
- what every unit test is checking
- what limitations remain in the final design

That level of explanation is more important than whether AI was used at all. The key issue is whether the final submission reflects my own understanding and judgement.

---

## 6. Academic integrity statement

I have not treated AI output as a finished submission. I used it selectively for explanation, review, and refinement, and I manually checked the final code, outputs, and report content before including them.

Where AI-assisted ideas were useful, they were incorporated only after being:

- understood
- edited to suit the project
- tested against the actual codebase

That is the standard I used throughout this project.
