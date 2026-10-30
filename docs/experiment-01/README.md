# Experiment 01 — Puzzle logic, validation, and uniqueness

**Research date:** 2026-10-29
**Status:** Research and protocol design complete; solver experiment **not executed**.
**Scope:** Classic square-grid Slitherlink only. No Android app, no production implementation, no puzzle generator and no technology selection.

This folder is the authoritative Experiment 01 research record. It extends, and does not replace, the earlier research documents.

## Documents

1. [Formal representation and validation](formal-specification.md) — canonical edge model, invariants, candidate checking, and error classes.
2. [Reference fixture catalog](reference-fixtures.md) — original small puzzles, hand-derived expectations, invalid candidate edge sets, and cases still needing independent validation.
3. [Solvers, generation references, and licenses](solver-review.md) — Slinker, Copris, Loopy, SAT/SMT and research limitations.
4. [Next controlled solver experiment](experiment-protocol.md) — reproducibility, comparison plan, completion criteria, and unanswered questions.

## Findings

- A puzzle's clues constrain cell edge counts, and each vertex of a finished loop has degree zero or two. Neither condition alone enforces a single connected cycle.
- A nonempty set of selected edges whose every incident vertex has degree two and whose selected graph is connected is one closed loop.
- Puzzle **solution existence**, **solution uniqueness**, **candidate drawing validity**, and **partial-player-state consistency** are different questions. Never interchange them.
- A valid puzzle may have multiple valid loops. Two is enough to establish non-uniqueness; a bounded solver that stops before completing the search must report **unknown** when it has not found a second loop.
- A rule-based solver is valuable for hints and human-difficulty research but is not automatically an exhaustive uniqueness oracle.
- Early test fixtures can be mathematically derived without executing software; those are **reasoned reference cases**, not experimentally verified benchmarks.

## Evidence classification

- **Verified external source:** a directly linked publication, official game documentation, or an upstream license file. Features described by authors remain their claims until tested.
- **Mathematically derived:** an explicit finite-graph argument or hand-checkable witness. Not yet independently cross-checked by software.
- **Candidate / hypothesis:** a proposed algorithm, performance expectation, or test without measured results.
- **Unverified:** a case with a known witness but unknown uniqueness, or a library behavior not executed locally.
- **Executed result:** none in this experiment.

## Research outcome and next step

The formal specification and initial fixture catalog are ready for review. The next separately authorized activity is a **small independent exact-solver comparison**, with no Android UI. See [experiment protocol](experiment-protocol.md).

## Reference sources reviewed 2026-10-29

- [Nikoli rules](https://www.nikoli.co.jp/en/puzzles/slitherlink/)
- [Copris Slitherlink documentation, including source and uniqueness option](https://cspsat.gitlab.io/copris-puzzles/slitherlink/index.html)
- [Slinker](https://github.com/timhutton/slinker)
- [Slinker GPL-3.0 license](https://github.com/timhutton/slinker/blob/main/LICENSE.txt)
- [Loopy manual](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/doc/loopy.html)
- [Simon Tatham MIT license](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/doc/intro.html)
- [Miyauchi and Tanaka, 2020](https://doi.org/10.2197/ipsjjip.28.959)
- [Yato, 2000, original complexity paper abstract](https://ipsj.ixsq.nii.ac.jp/records/32081)
- [OptiLog SAT modeling caveat](https://hardlog.udl.cat/static/doc/optilog/html/optilog/use-cases/slitherlink/modelling-slitherlink.html)
