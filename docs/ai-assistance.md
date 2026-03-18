---
layout: default
title: AI & Academic Integrity
---

# AI & Academic Integrity

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) 

[Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. Role of AI in this project

In this project, AI was used to help generate and refine:

- the C++ class structure
- the A\* search implementation
- grid handling and validation logic
- test scaffolding and unit-test ideas
- improvements to naming, modularity, and documentation wording

---

## 2. What I was responsible for

Although AI helped produce code, I was still responsible for:

- deciding the project scope and movement rules
- choosing the overall structure of the program
- checking whether the generated code actually matched the requirements
- questioning design decisions made by the AI
- testing the code on valid and invalid grids
- identifying where the report and implementation did not match
- writing and organising the GitHub Pages report myself
- understanding the final implementation well enough to explain it during assessment

---

## 3. How AI was used responsibly

AI outputs were not accepted blindly. A repeated review process was used:

1. **Prompt the AI clearly** with the required constraints  
   - C++
   - grid-based A\*
   - 4-directional movement only
   - Manhattan distance heuristic
   - modular design with header/source separation

2. **Read the generated code carefully**  
   I reviewed class names, method responsibilities, control flow, and data structures rather than just copying the output directly.

3. **Question design decisions**  
   I asked why particular choices had been made, for example:
   - why `std::priority_queue` was suitable
   - why Manhattan distance matched 4-direction movement
   - why `g_score`, `came_from`, and `closed` were needed

4. **Test the generated code**  
   The code was run on normal and edge-case examples to check that it behaved correctly.

5. **Refine or replace weak output**  
   If the AI produced code, wording, or structure that was unclear, inconsistent, or too generic, it was revised until it matched the actual project.

---

## 4. Importance of strong prompting

A major part of using AI effectively was writing strong prompts. Weak prompts produced vague or overly generic answers, while stronger prompts led to code and explanations that were much closer to the actual project requirements.

The most effective prompts included:

- the programming language
- the exact algorithm required
- the movement constraints
- the need for modular header/source files
- the desired coding style
- the requirement to explain *why* each design choice was made
- the need to keep documentation consistent with the implementation

For example, prompts were most useful when they specified details such as:

- “no diagonal movement”
- “use Manhattan distance”
- “return the files separately”
- “explain the logic step by step”

---

## 5. Questioning the AI output

One of the most important parts of this project was not just using AI to generate code, but actively questioning its output.

Examples of questions that were important during development included:

- Why is Manhattan distance the correct heuristic here?
- Why does `rows() - 1` or `cols() - 1` define the bottom-right goal?
- Why does the path length use `path.size() - 1` rather than `path.size()`?

---

## 6. Limitations and risks of AI use

AI also had limitations, and these needed to be managed carefully.

Potential risks included:

- explanations that sound correct but are slightly imprecise
- inconsistent naming or file references
- documentation that claims more than the implementation actually provides
- overcomplicated solutions that are harder to explain in a demo

---

## 7. Reflection on AI use

Using AI in this project was helpful, but the most valuable part was not the generation itself. The most valuable part was the process of reviewing, questioning, refining, and validating what was generated.

This project showed that effective AI use in software development is not passive. It requires:

- precise prompting
- technical judgement
- willingness to challenge the output
- careful testing
- and enough understanding to explain the final design clearly

For that reason, AI was best viewed as a development assistant rather than an automatic solution. The quality of the final project depended not only on what the AI produced, but on how critically and effectively its output was used.
