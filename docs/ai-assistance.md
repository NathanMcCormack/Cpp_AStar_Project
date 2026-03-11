---
layout: default
title: AI & Academic Integrity
---

# AI & Academic Integrity

[Home](index.html) | [Algorithm](algorithm.html) | [Implementation](implementation.html) | [Testing](testing.html) | [Project Management](project-management.html) | [Reflection](reflection.html) | [AI & Integrity](ai-assistance.html) | [References](references.html)

---

## 1. How AI tools were used

AI tools (specifically Claude by Anthropic) were used as a **learning and review aid** — not as a code generator. All design decisions, class structures, and algorithmic choices are my own. The tool was used in the following limited ways:

- Clarifying theoretical questions (e.g. "what does admissibility formally mean?")
- Getting plain-language explanations of STL behaviour (e.g. how `std::priority_queue` comparators work)
- Reviewing code for comments and clarity after the implementation was already written
- Helping flesh out the wording of report pages after the technical content was drafted

Any AI-assisted content was:
- Limited in scope (specific, targeted questions — not "write my A* implementation")
- Adapted to my design (the structure, naming, and logic are mine)
- Fully understood by me (I can explain every line in person during a lab demonstration)

---

## 2. Traceability table

| Date | What I asked | What I used | Where used |
|---|---|---|---|
| W2 | "What does admissible mean formally for A*?" | Added formal definition + admissibility explanation | `docs/algorithm.md` — Section 3 |
| W3 | "Why does std::priority_queue give the max by default?" | Added min-heap explanation + `NodeGreater` comment | `aStar.cpp` — comparator comment block |
| W4 | "Review my stale-entry comment for clarity" | Improved the comment explaining why `cur.g != gScore` detects stale entries | `aStar.cpp` — stale-entry check |
| W5 | "Help expand my report page wording" | Report page prose polished, but technical content and structure are my own | `docs/` — multiple pages |

---

## 3. What I did NOT use AI for

- The class design (`aStarGrid`, `Pos`, `AStarResult`) — designed on paper before any coding
- The decision to use 4-direction movement and Manhattan distance
-

---

## 4. Independence statement

