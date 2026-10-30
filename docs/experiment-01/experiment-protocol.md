# Experiment 01 — Proposed controlled solver verification

**Protocol drafted:** 2026-10-29
**Status:** Ready for review; **not executed**.
**Authorization boundary:** Research and protocol design only today. A new request must authorize running a small solver experiment.

## Research question

Can two substantially independent exact techniques agree on clue-board solution counts and reject deceptive locally valid multi-loop drawings, with complete, reproducible evidence?

## Candidate methods (to be authorized later)

- **Path A:** original reference implementation using undirected H/V edge states, clue/vertex propagation, exhaustive backtracking when necessary, and a separate graph-component validator.
- **Path B:** published Copris Slitherlink v2.0 run with the multiple-solution option (-o multi), verifying that output actually distinguishes zero, exactly one and two-or-more solutions for our fixtures.
- **Additional cross-check:** Loopy or SMT circuit encoding only if A/B disagree or coverage is inadequate. No Slinker GPL source reuse.

Two implementations can share a mistaken interpretation of the rules. Therefore also evaluate manually provable cases and an independent final edge-set witness validator.

## Test scope and inputs

1. Start with F01–F10 from [fixture catalog](reference-fixtures.md): tiny cases with mathematically derived counts (0, 1, 2+).
2. Check F11–F12: known or reported witnesses, but **do not assume uniqueness**. Compare exact solver counts only after completeness is established.
3. Check F13–F19: candidate-drawing validator behavior, graph connectivity, malformed data, equivalent cycles, and unmarked non-loop edges.
4. Simulate F20: incomplete search must yield UNKNOWN rather than a confident guess.
5. Only after the above, choose a small fixed sample of 3x3, 4x4, 5x5, 6x6, 8x8 and 10x10 clue boards, each with an explicit origin and permission to use. If generators are not yet authorized, use original hand-specified boards and publicly licensed test inputs; do not copy commercial packs.

## Measurement contract

Record per test:
- Fixture ID and immutable clue grid; separately any selected-edge witness.
- Exact repository commit, solver version/source revision, run command, parameters and configuration.
- Hardware CPU, memory, OS, Java/Scala/solver versions as applicable.
- Search completion status: COMPLETE, INTERRUPTED, RESOURCE_LIMIT, ERROR.
- Solution classification: ZERO, UNIQUE, MULTIPLE, UNKNOWN; number of distinct witnesses found (capped at two).
- Witness selected-edge IDs / canonical SHA when a witness exists.
- Independent witness-validator outcome and any mismatch.
- Wall-clock and CPU runtime, peak memory if measurable, number of branches/nodes if supported.
- Repeated-run variation and timeout configuration; record any random seed.
- Raw stdout/stderr and diagnostic logs sufficient to reproduce conclusions.

Do not present results from another author's machine as our measurements.

## Proposed pass/fail conditions

**Correctness conditions (all required):**
1. Complete solver runs match all hand-proved count classifications F01–F10.
2. Both methods classify the two-loop candidate F13 as **INVALID FINAL DRAWING**, despite valid local counts.
3. Every reported solution witness independently passes all clue, vertex, nonempty and connectivity conditions.
4. Equivalent traversal orderings do not produce false second solutions.
5. Incomplete search is reported as UNKNOWN unless two distinct valid witnesses already prove MULTIPLE.
6. Parsing and invalid-input failures are not mislabeled as unsatisfiable puzzles.
7. Any A/B disagreement is retained as a failure/unknown pending investigation, not averaged away.

**Performance conditions:** diagnostic only on the first pass. Report distributions (median, p95, maximum, timeouts) for identical inputs and environment. No unsupported speed threshold or Android prediction will be adopted before measurement.

**Exit decision:**
- **Proceed** only if correctness requirements hold on the predeclared fixture set and a reproducible report exists.
- **Revise** the model or reference fixtures when disagreement reveals an error; log the correction.
- **Inconclusive** if tools cannot run, search is incomplete, or test coverage is insufficient.

## Later generation experiment: explicit separation

Generation is not part of the initial solver verification. If separately authorized, experiment with constructive loops and clue removal only after a reliable exact verifier exists. Track seed, candidate loop, clue removals, final solution count, timeouts, clue density, and duplicate/near-duplicate rate.

A 10x10 puzzle may be trivial for a person and a 5x5 puzzle may be difficult. Do not use solver wall-clock time as a substitute for human difficulty.

## Open questions for next iteration

- Does the minimal reference parser accept 1-row/1-column mathematical boundary puzzles, or must an adapter feed those to Copris?
- Does Copris -o multi show exact counts or lower bounds depending on search completion?
- How should the smallest disconnected-loop rejection be encoded as a final drawing and as a partial-state contradiction?
- Are all claimed upstream licenses consistent across bundled files and transitive dependencies?
- Which additional independently proven 3x3 to 5x5 **unique** puzzles can be added without copying third-party material?
- What runtime/memory envelope is realistic on representative Android devices once technology selection is authorized?

## Immediate next action

Request authorization for a deliberately small **solver verification experiment** only: implement or run the two exact methods against the approved test catalog, capture all outputs, compare failures and document them. Do **not** build the Android app, generate a commercial puzzle library, or treat difficulty scoring as solved.
