# 06 — Open questions, experiment plan, and roadmap

**Date:** 2026-10-29  
**Status:** Proposed research checkpoints only. No experiments executed. No Android development authorized.

## Priority unknowns

**High**
1. Can an independent checker reliably reject multi-loop, zero-solution, and ambiguous puzzles?
2. Can a candidate generator consistently yield **unique** and interesting puzzles?
3. Can difficulty be described by reproducible logical techniques and validated by players?
4. Which real-phone interaction produces the fewest mistakes and least frustration?

**Medium**
5. Can hints explain a forced deduction rather than merely match an unseen solution?
6. What zoom/selection scheme works for large boards?
7. Does Compose Canvas outperform a custom View or Godot for our exact interaction?
8. What assistance and progression options do players actually want?
9. What copyright/trademark and Play policy checks apply to the final product?

## Experiment 01 — Rules and uniqueness feasibility (recommended next step)

**Objective:** Develop a precise, testable square-grid specification and evaluate independent verification approaches.

**Inputs:** small known fixtures; proposed valid, unsatisfiable, multiple-solution, multi-loop, boundary and blank-cell cases. Start with 3×3 to 10×10 boards as a research range, not a hard product limit.

**Work, only when authorized:** select candidate solver approaches, run reproducible comparisons, record seeds and versions, measure runtime and unknown/timeouts.

**Pass criteria (to be finalized before execution):**
- Each known fixture classified correctly as 0, 1, or 2+ valid solutions when a complete result is reported.
- A disjoint-multiple-loop candidate rejected even if local clue and degree constraints pass.
- Incomplete searches are recorded as **unknown**, never a definitive classification.
- Results include fixtures, code/version identities, hardware, settings and reproducible logs.
- Disagreements are investigated rather than averaged away.

**Not authorized by this documentation:** implementation of production code, Android app creation, or generation of a large commercial puzzle library.

## Experiment 02 — Generation and quality

Compare proposed constructive-loop and clue-removal schemes, with validation after removals. For fixed test ranges measure uniqueness success, timeouts, speed, repetition, clue density and basic logical features. Do not equate large output quantity with enjoyable difficulty.

## Experiment 03 — Human difficulty and hint logic

Define and record candidate rule classes. Compare solver explanations with a small sample of player reasoning. Quantify where the logical solver stalls and where ratings disagree with people. Do not claim that a stalled solver proves a puzzle needs guessing.

## Experiment 04 — Touch input and zoom

Compare tap cycle, dedicated line/X tools, drag and alternative selection on identical puzzles. Record device/OS, grid size, orientation, zoom, incorrect edits, reversals, timings, interruption behavior and player feedback. Treat qualitative comments separately from measured performance.

## Experiment 05 — Android technical proof of concept

Only after explicitly approving prototype work, evaluate Compose Canvas, a custom View, and optionally Godot against actual interaction requirements. Inspect rendering clarity, gesture routing, persistence and accessibility. Do **not** infer superiority from brand familiarity.

## Preparation stages and simple completion gates

| Stage | Required outcome |
| --- | --- |
| Research baseline | Sources, competitor claims, uncertainties and boundaries recorded |
| Logic specification | Formal rules and fixture set reviewed |
| Solver validation | Correctness and uniqueness method demonstrated with reproducible tests |
| Generator validation | Measured puzzle success, performance and quality risks |
| Difficulty/hint study | Candidate ratings and explanations compared with human reasoning |
| Controls study | Device-tested accuracy and interaction data |
| Technology selection | Written selection justified by observed tradeoffs |
| Initial playable scope | Small feature list with explicit tests, approved separately |
| Development | Begins **only** after new authorization |

Do not use SPBT. Use plain-language milestones, evidence, and acceptance criteria.

## Proposed immediate action

Write the test-case catalog and experimental design for **Experiment 01**. The primary question is correctness and uniqueness, not graphics, monetization, or the eventual app's name.

## Exit state of research to date

Rules and broad options are established. Solver correctness, generation throughput, quality ratings, player ergonomics, and technology fit remain **unverified**. This project is in preparation, not implementation.
