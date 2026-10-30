# 04 — Puzzle generation, solving, and quality

**Research date:** 2026-10-29  
**Status:** Comparative research only. No generator, solver, benchmark, or puzzle collection has been implemented.

## Why the puzzle engine matters

Generating any closed loop is not the same as generating a playable puzzle. A useful pipeline must guarantee formal validity, ensure that the clues define only one solution, and select puzzles with appropriate human difficulty and variety.

## Candidate pipeline (hypothesis, not chosen)

1. Construct a single valid nonempty loop on a rectangular grid.
2. Calculate the clue for each cell from its four surrounding loop edges.
3. Remove selected clues.
4. After each removal, check whether exactly one valid loop still satisfies the visible clues.
5. Estimate deductions needed by a human-style logical solver.
6. Filter awkward, repetitive, trivial, or misrated puzzles.
7. Store reproducible puzzle data (size, clues, solution, seed, method/version, verification result).

The candidate loop remains one solution after clue removal, but other solutions may appear. Thus a uniqueness check remains essential.

## Solver alternatives

| Technique | Value | Risk / limitation |
| --- | --- | --- |
| Backtracking + propagation | Simple reference approach; local clue/vertex deductions | Branching can grow quickly; global loop check needed |
| Constraint-programming model | Natural finite-domain clue and degree constraints | Connectivity is not implied by local constraints |
| SAT/SMT | Can support solution-finding and uniqueness exclusion constraints | Encoding and dependencies add complexity |
| Inside/outside region formulation | Can exploit dual representation of loop boundaries | Must handle holes, multiple loops, and topology correctly |
| Rule-based human solver | Supports candidate hints and difficulty explanations | Incomplete rule sets may get stuck without disproving logical solvability |

Generalized Slitherlink solution existence is studied as an NP-complete problem. This is not a runtime forecast for our target board sizes.

Primary publication: [Miyauchi and Tanaka, “Solving Slitherlink with FPGA and SMT Solver,” Journal of Information Processing 28 (2020), 959–969](https://doi.org/10.2197/ipsjjip.28.959). The paper explores SMT and region-oriented formulations; its published performance figures are **not our measured performance**.

## External reference implementations

- [Slinker by Tim Hutton](https://github.com/timhutton/slinker): documentation describes generation, rule-based solving, uniqueness checking, and known difficulty/speed limitations.
- [Simon Tatham's Portable Puzzle Collection](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/): includes the Loopy game and reusable research reference material; license must be understood before copying any code.
- [Loopy manual](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/doc/loopy.html): describes gameplay and options.
- [Published SMT paper](https://doi.org/10.2197/ipsjjip.28.959): formal alternative to direct loop encoding.

These are sources and references, not adopted libraries. Any implementation license and dependency compatibility must be verified before reuse.

## Validation requirements

For a proposed final edge set:

- Every selected edge belongs to the board.
- Each clue count matches exactly.
- Every vertex has degree 0 or 2.
- There is at least one selected edge.
- Every selected edge belongs to **the same connected cycle**.

For uniqueness: search for a second distinct valid set of selected edges, not just another traversal or rotation of the same loop. Report **0 / 1 / 2+ solutions / unknown**. Unknown includes resource exhaustion, interrupted enumeration, and timeouts.

Independently derived validation methods or reference fixtures are desirable. A second implementation can reduce shared bugs but does not mathematically prove correctness.

## Puzzle quality and difficulty

Candidate metrics:

- Maximal technique class required by a logical solver.
- Length/depth of dependent deductions.
- Need for long-range or inside/outside reasoning.
- Number of places with a currently available deduction.
- Amount of repetition and clue sparsity.
- Players' actual error rate, completion time, and qualitative enjoyment.

A logical solver unable to proceed may only reveal an incomplete library of rules, **not** that the puzzle requires guessing. Solver wall-clock runtime is not equivalent to player difficulty.

A generated puzzle can be unique and still dull. Variety should be measured through clue-pattern similarity, loop shape, and human feedback, rather than merely counting different seeds.

## Distribution of generated puzzles: candidate strategies

- Pre-generate, verify, inspect, and ship finite puzzle collections: reliable and easy to test, but content must be replenished.
- Generate on-device: unlimited potential puzzles, but must measure latency, battery, memory and failure behavior.
- Hybrid: ship a verified collection and optionally generate additional puzzles. More flexible but more systems to maintain.

**Preliminary preference:** research pre-validated offline puzzles as the safest initial playable path. This is not a final product decision.

## Proposed first experiments (not executed)

1. Write a small formal fixture catalog containing solvable, impossible, ambiguous, and disconnected-cycle cases.
2. Compare at least two independent solving/verification approaches.
3. Attempt deterministic generation at fixed small board sizes, recording seeds and algorithm versions.
4. Measure uniqueness rate, timeouts, throughput, and representative worst cases.
5. Test logical-difficulty predictions against human observations before committing to difficulty labels.

See [roadmap](06-research-roadmap.md) for order and checkpoints.
