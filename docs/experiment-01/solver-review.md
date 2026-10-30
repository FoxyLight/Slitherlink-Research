# Experiment 01 — Solver techniques, reference implementations, and licenses

**Research reviewed:** 2026-10-29
**Status:** Source/documentation analysis only; no source compiled, program run, or runtime measured.

## 1. Four distinct approaches

| Approach | Main value | Uniqueness limitation / issue |
| --- | --- | --- |
| Propagation plus exhaustive backtracking | Small understandable reference; strict graph validation possible | Explosive branching on some boards; pruning must preserve completeness |
| Constraint programming (CP) | Expresses arithmetic and degree restrictions naturally | Must constrain/reject disjoint circuits |
| SAT/SMT | Mature general-purpose search and incremental exclusion | A model with only local constraints may represent multiple loops |
| Human-rule deductive solver | Meaningful hint steps and candidate difficulty profile | Inability to finish is not a uniqueness proof |

A **graph component check** is a validator primitive; by itself it is not a full puzzle-solving algorithm. It can be embedded in any of the exact search approaches.

### Exact uniqueness method (conceptual)

Find a first fully valid solution S1. Then search for a different fully valid selected-edge set S2, either by exhaustive branching or by a sound blocking condition. If S2 exists -> 2+. If the exhaustive search proves no S2 -> exactly one. If no solution exists and search is complete -> zero. An interrupted search -> unknown unless two distinct witnesses have already proved nonuniqueness.

For SAT-style methods, adding a full-assignment blocking clause to a **local-only** solver and finding another local model is not enough: disconnected-cycle models must be filtered or connectivity modeled. It may take multiple blocking/cut rounds.

## 2. Slinker by Tim Hutton

- Project: https://github.com/timhutton/slinker
- Author's described capabilities: generation, logical-rule solving, uniqueness checks, hints, and Loopy import/export.
- Author's documented open issues: slow generation/large-board solving; uncertainty about how to rate puzzle difficulty and target harder rules.
- **License confirmed from LICENSE.txt and COPYING.txt: GNU GPL version 3.** https://github.com/timhutton/slinker/blob/main/LICENSE.txt
- Research suitability: excellent for understanding rule sets and puzzle-generation tradeoffs. **No reuse or incorporation of GPL code is approved.** Commercial distribution is not automatically forbidden by GPL, but GPL-covered reuse can impose source-distribution and other obligations. Treat licensing decisions separately from algorithm research.
- No executable benchmark or independent correctness audit performed.

## 3. Copris Slitherlink by Naoyuki Tamura

- Reference and inline source: https://cspsat.gitlab.io/copris-puzzles/slitherlink/index.html
- Copris uses a Scala constraint-programming DSL; version 2.0 documentation describes Scala 2.12, bundled Copris/Sugar/Sat4j, optional external SAT solvers and **-o multi** for multiple-solution checking.
- The shown solver uses directed candidate arcs, local clue restrictions, and circuit handling. It is a strong **independent reference** compared with our future undirected-edge validator.
- Published evaluation: 840 example instances all solved within the documented 3600-second limit; the numbers are **upstream results on their machine and workload**, not ours.
- **License:** project page specifies BSD 3-Clause.
- Research suitability: highest-priority independent reference for the proposed first experiment. Confirm its exact enumeration semantics and output format using small fixtures before treating it as an oracle. Do not call a one-solution output proof of uniqueness.
- Potential setup burden: Scala/Java version dependencies and an optional SAT executable.

## 4. Simon Tatham's Loopy

- Collection: https://www.chiark.greenend.org.uk/~sgtatham/puzzles/
- Loopy user manual: https://www.chiark.greenend.org.uk/~sgtatham/puzzles/doc/loopy.html
- Source distribution: https://www.chiark.greenend.org.uk/~sgtatham/puzzles/
- The manual documents square/non-square grids, selectable difficulty and automatic following of unique paths; the distributed collection is licensed under **MIT**, as expressly stated in its introduction and license.
- Research suitability: mature reference for user-facing behavior, solving, puzzle generation and varied difficulty.
- Caveat: generator difficulty settings or completing a published puzzle are not by themselves an independent complete uniqueness proof. Source version, solver configuration, recursion behavior, and puzzle identity must be recorded before using Loopy for formal comparison.
- No binary or generator run during this phase.

## 5. Research literature

- Takayuki Yato, *On the NP-completeness of the Slither Link Puzzle* (2000), abstract/source: https://ipsj.ixsq.nii.ac.jp/records/32081. Existence of a solution is NP-complete for generalized instances. This does not predict practical runtime for our boards.
- Takayuki Yato and Takahiro Seta, *Complexity and Completeness of Finding Another Solution and Its Application to Puzzles* (2003): https://globals.ieice.org/en_transactions/fundamentals/10.1587/e86-a_5_1052/_p. Addresses the separate problem of finding another solution.
- Tetsuo Miyauchi and Kiyofumi Tanaka, *Solving Slitherlink with FPGA and SMT Solver* (2020): https://doi.org/10.2197/ipsjjip.28.959. Compares approaches with a region inside/outside formulation. Its timing results are on its own workload/hardware.
- OptiLog Slitherlink model: https://hardlog.udl.cat/static/doc/optilog/html/optilog/use-cases/slitherlink/modelling-slitherlink.html. Clearly warns its example CNF constraints **do not** impose one cycle, making it a useful negative research example.

## 6. What we should and should not infer

**Supported:** software and papers exist for exact solving, multiple-solution detection and logical deduction. License terms differ materially between Slinker (GPL-3.0), Copris (BSD-3-Clause) and Loopy (MIT).

**Not supported yet:** any claim that one solver is fastest on our target Android hardware, that a generator is complete and unbiased, that a rule difficulty rating matches human experience, or that all upstream implementations are free of bugs.

**Candidate strategy for the next experiment:** independently implement or configure two exact verification paths only after authorization: (A) a compact edge-search solver with independent final graph validator, and (B) Copris multiple-solution mode. Compare outputs on the fixture catalog. Loopy is an additional cross-check but is not required for the first pass. Do not import Slinker GPL code.

## 7. Complexity and correctness cautions

- Never mistake a local cycle for the one required global loop.
- Never count alternate directions/start positions as distinct solutions.
- Treat UNKNOWN as distinct from UNSAT.
- Validate every solver-provided witness with an independent graph checker.
- When a solver reports a unique solution, verify both existence and exhaustive exclusion of all alternatives.
- Keep algorithm execution time separate from human puzzle difficulty.

All implementation comparisons are **proposed**, not run.
