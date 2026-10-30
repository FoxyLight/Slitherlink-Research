# Slitherlink Android App — Research Repository

**Research baseline:** October 29, 2026  
**Status:** Research and preparation only. No Android app, production solver, puzzle generator, or playtest has been started.

This repository is the **authoritative project record** for the proposed Android loop-puzzle application. Decisions, source-backed findings, experiment protocols, measurements, and unresolved questions belong here. Chat summaries or speculative ideas are not automatically confirmed project facts.

## Research index

1. [Rules and mathematical specification](docs/01-rules-and-mathematics.md)
2. [Existing Android applications](docs/02-existing-android-apps.md)
3. [Player experience and creative options](docs/03-player-experience.md)
4. [Puzzle generation, solving, and quality](docs/04-puzzle-logic-research.md)
5. [Android technology, distribution, and IP](docs/05-android-and-distribution.md)
6. [Open questions and preparation roadmap](docs/06-research-roadmap.md)
7. [Evidence and decision log](docs/07-evidence-and-decisions.md)
8. [Experiment 01 — formal rules, verified sources, reference cases and experiment protocol](docs/experiment-01/README.md)

## How to interpret the documentation

- **Verified:** grounded in a named primary source, an established mathematical fact, or a reproducible completed test; cite the evidence.
- **Developer claim:** published by another app's developer; not independently tested.
- **User report:** feedback attributed to a reviewer; not automatically representative.
- **Hypothesis / candidate:** proposed direction, not validated or approved.
- **Open:** an unanswered question or missing evidence.
- **Decision:** an explicit project choice, with the date, rationale, and possible conditions for revisiting it.

Important distinction: a valid closed loop is **not** proof that a clue set has only one valid solution. All claims about puzzle uniqueness must be supported by explicit verification.

## Authorized scope

Research rules, algorithms, competitor apps, user interaction, Android technologies, and publishing constraints. Design narrow, measurable experiments with straightforward checkpoints and completion criteria.

**Not authorized:** building an Android project, implementing a production solver/generator, fixing the visual identity, copying existing puzzle sets, creating a new repository, selecting a final monetization model, or releasing an app.

Do **not** use SPBT in this project.

## Source and test discipline

1. Link claims to sources and record an access/review date for changeable facts.
2. Keep source claims separate from our observations and hypotheses.
3. Record devices, inputs, seeds, versions, methods, failures, and raw outputs for actual experiments.
4. Never label an unexecuted experiment as passed.
5. Record timeouts as **unknown**, not as zero or one solution.
6. Revise earlier conclusions transparently, retaining why the change occurred.
7. Prefer original project writing and original test fixtures. Respect external content and software licenses.

## Immediate next research activity

**Experiment 01: Puzzle logic and uniqueness feasibility.** Specify square-grid edges, clues, cycle constraints, and test fixtures. Compare independent solver approaches on the same known cases. Begin with a research protocol, not application code. See [roadmap](docs/06-research-roadmap.md).

---
This repository is a documentation record, not a fork of Nikoli, Conceptis, Ejelta, Simon Tatham's Puzzles, or any other product. “Slitherlink” is a descriptive working name; the eventual public branding remains open pending trademark review.

## Experiment 01 research update (2026-10-29)

The [Experiment 01 research package](docs/experiment-01/README.md) now documents the formal edge model, hand-derived tiny-grid test cases, a deceptive disconnected-loop candidate, external solver/license review, and a protocol for the next *not-yet-run* exact-solver comparison. **No solver, generator, Android app, or benchmark was implemented/executed in this update.**
