# Experiment 01 — Reference fixture catalog

**Date:** 2026-10-29
**Status:** Original, hand-reasoned cases; **not executed or independently machine-verified**.

These cases are designed to anchor future automated tests. Small examples are mathematically justified below. Do not describe any case as experimentally passed. The purpose is to catch incorrect local-only validators and false uniqueness claims.

## Fixture notation

- Board rows go from top to bottom. Within each row, tokens are cells from left to right.
- The token "." means an unnumbered cell. Other tokens are standard clues 0..3.
- R x C denotes cell dimensions, not dot dimensions.
- **H(r,c)** and **V(r,c)** use the coordinate definitions in [formal specification](formal-specification.md).
- A witness is a set of selected edges or a closed traversal of neighboring vertices.
- Uniqueness classification is a property of a **clue board**, whereas valid/invalid classification can describe a **particular selected-edge drawing**.

## A. Mathematically derived tiny-board cases

For a 1 x N cell board, the grid is a ladder with only two rows of vertices. Every single simple cycle encloses exactly one consecutive nonempty run of cells [a..b]. In particular, there are N(N+1)/2 distinct single-loop edge sets on an all-blank 1 x N ladder. This observation supplies independent, hand-checkable expectations for the following tests.

| ID | Dimensions | Clues, by row | Expected exact solution count | Reason |
| --- | --- | --- | --- | --- |
| F01 | 1x1 | . | 1 | One four-edge cell perimeter |
| F02 | 1x1 | 0 | 0 | Only nonempty cycle has four surrounding edges |
| F03 | 1x1 | 3 | 0 | Same reasoning; classic clue 4 not allowed |
| F04 | 1x2 | . . | 3 | Enclose left cell, right cell, or both |
| F05 | 1x2 | 3 . | 1 | Only the two-cell outside perimeter has three edges around left cell |
| F06 | 1x2 | 1 . | 1 | Only the right-cell perimeter contributes one edge to left cell |
| F07 | 1x2 | 2 . | 0 | The three cycles contribute 4, 1 and 3 left-cell edges |
| F08 | 1x5 | 1 3 2 3 1 | 1 | Unique consecutive run spanning cells 2, 3 and 4 (1-based) |
| F09 | 2x2 | 0 0 / 0 0 | 0 | Every grid edge borders a zero cell; only empty edges remain, prohibited |
| F10 | 3x3 | . . . / . . . / . . . | 2+ | Distinct cell-perimeter loops exist |

**Proof detail for F08:** In a one-row ladder, a cycle enclosing consecutive cells a..b gives: 4 around a lone enclosed cell; otherwise 3 around each endpoint cell, 2 around interior enclosed cells, 1 around immediately neighboring unenclosed cells, and 0 farther away. Given 1 3 2 3 1, the 3 clues force the enclosed run to start at cell 2 and end at cell 4; the remaining clues then agree.

These 1-row cases are **mathematical boundary tests**. A production generator may quite reasonably restrict grids to at least 4x4, but the specification and validator should still handle small legal grids or clearly reject them as unsupported input.

## B. Valid witness; uniqueness still open

**F11 — 3x3 nonrectangular loop witness**

Clues:
    2 3 1
    3 2 0
    1 0 0

Select edges around the three-cell L-shaped region occupying cells (0,0), (0,1), and (1,0). Closed vertex traversal, in (r,c) coordinates:
    (0,0) -> (0,1) -> (0,2) -> (1,2) -> (1,1) ->
    (2,1) -> (2,0) -> (1,0) -> (0,0)

There are 8 distinct edges. This is one simple cycle. Direct edge counting gives the shown 3x3 clue matrix, so **at least one valid solution exists**. Whether a different solution exists remains **unknown** until exhaustive verification.

**F12 — Published 4x4 Copris example**

Source: [Copris Slitherlink documentation](https://cspsat.gitlab.io/copris-puzzles/slitherlink/index.html), example input and solution. Clues (".": blank):
    1 . . 0
    . . . .
    1 . 2 1
    . 2 3 .

The upstream documentation displays one solution and explicitly reports **NumOfSolutions >= 1** (non-multiple search mode). Therefore the directly supported claim is **at least one solution**, not unique. This fixture is a **third-party reported witness**, pending our own independent edge and clue verification. It must not be used as a gold-standard uniqueness answer.

## C. Candidate-drawing rejection cases

**F13 — Two separated cycles that satisfy local constraints**

Board 2x5, all clue values 2:
    2 2 2 2 2
    2 2 2 2 2

Candidate edge set: outside perimeter of the left 2x2 block spanning columns 0..1, **plus** outside perimeter of the right 2x2 block spanning columns 3..4, with the one-column gap between them. Each loop has eight selected edges, for 16 total, and the selected components have disjoint vertices.

Every vertex touching selected edges has degree 2. Every cell in the two 2x2 blocks has two selected edges. Each gap cell also has two selected edges, one supplied by the left loop border and one by the right loop border. Thus **every local clue and vertex-degree requirement is satisfied**, but the selected drawing is **INVALID** because it has two connected components.

Important: this does **not** establish whether the 2x5 all-2 puzzle has zero, one, or multiple valid single-loop solutions. Puzzle uniqueness status: **unverified**.

**F14 — Empty selected-edge set:** no selected edges on all-blank 1x1. Candidate drawing INVALID (requires nonempty loop), despite vacuous local constraints.

**F15 — Branch:** 2x2 blank board; select three distinct edges meeting at center vertex (1,1). Candidate drawing INVALID (degree 3); still an allowable *unfinished* partial state only if intended as a temporary player marking, and can never be extended without removing an edge.

**F16 — Open path:** 2x2 blank board; select one edge only. Candidate drawing INVALID as a final solution; not necessarily contradictory as partial progress.

**F17 — Out-of-range edge:** 2x2 board; include H(3,0) as an input edge. Must be REJECTED AS MALFORMED, not silently ignored.

**F18 — Equivalent traversals:** rotate or reverse the vertex traversal of F11's single witness. Must be treated as the SAME selected-edge set, never counted as another solution.

**F19 — Correct loop, missing crosses:** F01 selected four-edge perimeter, all other edges unknown (there are no others for 1x1). Also test a valid F11 cycle with nonselected edges left UNKNOWN instead of EXCLUDED. Candidate remains VALID. A player's X marks are not required for completion.

**F20 — Artificial resource cutoff:** request uniqueness of F04 while forcing a search interruption before full proof. Classification must be UNKNOWN unless two distinct valid loops have already been found, in which case 2+ is established.

## D. Future property-based and mutation tests (not yet run)

- Symmetry: reflecting or rotating a board and its solution should preserve existence and solution count; map H/V correctly on a quarter-turn.
- Conservation: canonical edge set should be independent of insertion order.
- Remove one selected edge from a valid cycle: final solution must fail degree/connectivity requirements.
- Duplicate an edge in serialized input: strict parser should reject rather than count twice.
- Change one clue beyond the selected-edge count: known witness should fail, although another valid solution may exist.
- Add a second disjoint cycle to any known loop: candidate must fail single-component test.
- Check a partial state where local clue bounds are not violated but global completion is impossible, to test global reasoning rather than trivial pruning.
- A fixed seed must reproduce the same generator result **once a generator actually exists**.

## Fixture acceptance and limitations

Statuses above are **hand-derived expected outcomes**, except F12 which is a third-party reported example and F11/F13 where we specify a verifiable drawing. No automated runs, independently implemented solvers, or benchmark checks have happened. An independently implemented validator and at least one exhaustive solver should check every fixture when the separately authorized experiment begins.

Do not copy published commercial puzzle packs into this repository. F01–F11 and F13–F20 are constructed for this research; F12 is attributed to the published Copris example and included solely as a small research reference.
