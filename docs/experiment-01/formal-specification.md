# Experiment 01 — Formal representation and validation

**Date:** 2026-10-29
**Status:** Mathematical specification; no parser, validator or solver executed.

## 1. Scope and coordinates

A puzzle contains **R rows and C columns of cells**, positive integers unless a future explicit compatibility requirement says otherwise. Coordinates are zero-based. r increases downward; c increases rightward.

- Cells: (r,c) with 0 <= r < R, 0 <= c < C.
- Dots / vertices: (r,c) with 0 <= r <= R, 0 <= c <= C.
- Horizontal edges: **H(r,c)** between vertices (r,c) and (r,c+1), 0 <= r <= R, 0 <= c < C.
- Vertical edges: **V(r,c)** between vertices (r,c) and (r+1,c), 0 <= r < R, 0 <= c <= C.
- Total vertices: **(R+1)(C+1)**.
- Total undirected edges: **(R+1)C + R(C+1)**.

A cell (r,c) is bounded by H(r,c), H(r+1,c), V(r,c), V(r,c+1).

Recommended initial canonical format: a rectangular clue matrix and two separate horizontal/vertical edge arrays. The chosen representation is for experiment interoperability, **not** a final Android app data model.

- Clues: integers 0, 1, 2, 3, or a blank sentinel (shown as "." in fixtures).
- Working edges: UNKNOWN, SELECTED, or EXCLUDED.
- Completed-loop mathematics: selected-edge set S; all other legal edges are not selected, even if left UNKNOWN in the player's notes.
- Canonical edge identity uses orientation plus coordinates, never an ordered traversal index.
- Row-major flattening is possible, but should only be used with a documented reversible mapping.
- Saved UI history (undo, viewport, pencil marks) is separate from immutable puzzle clues and from formal solutions.

Reject malformed dimensions, nonrectangular clue matrices, illegal clue values (including 4 in classic mode), duplicate or out-of-range edge IDs, and invalid input formats before solution classification.

## 2. Exact final-solution invariants

Let x(e) be 1 for selected edges and 0 otherwise.

For every numbered cell c with clue n(c):
  sum of its four x(e) = n(c).

For every grid vertex v:
  sum of x(e) incident to v is either 0 or 2.

Additionally:
  |S| > 0;
  every selected edge is a legal grid edge;
  all vertices incident to selected edges belong to one connected component, using only selected edges.

On this finite undirected square-grid graph, nonemptiness + degrees 0/2 + connectivity of the selected subgraph implies exactly one simple cycle. These three conditions are jointly essential.

Only degree counts cannot rule out two disjoint cycles. Checking that every numbered cell is satisfied cannot rule them out either. The center of a degree-4 square-grid intersection cannot be crossed because selected degree 4 is forbidden.

## 3. Suggested candidate validator (conceptual, no code)

1. Validate board dimensions and clue matrix.
2. Normalize proposed selected edges as a set of legal edge identities; explicitly reject malformed or duplicate declarations under a strict input format.
3. Count the selected edges bordering each numbered cell.
4. Count selected incident edges at each vertex; require degree 0 or 2.
5. Require at least one selected edge.
6. Starting at any selected vertex, traverse selected edges; require every selected vertex to be reached. Equivalent: exactly one nonempty selected-edge component.
7. Accept the loop only when all checks pass.

Independent check: tracing along the two incident edges from any used vertex must return to the start without reusing an edge or leaving another selected component. A second traversal implementation can expose connectivity bugs.

Do not require every nonselected edge to carry an X: marking is an optional player activity. A purported solution that selects no edges is invalid even when all clues are 0 or blank.

## 4. Partial states must not be confused with completed solutions

A partial state includes SELECTED, EXCLUDED and UNKNOWN edges.

- **Local contradiction**: a clue already has too many selected edges, or even selecting all unknown surrounding edges cannot reach its clue; analogous infeasibility at a vertex.
- **Globally infeasible partial state**: no valid complete loop extends all selected/excluded decisions. Determining this can require full search.
- **Consistent so far**: no contradiction discovered; **not** proof that a completion exists.
- **Completed valid loop**: exactly the final selected-edge cycle satisfies the rules. The player need not mark every other edge X.

Premature-loop detection must consider whether selected edges or still-required clues outside the closed loop can be satisfied. A completed cycle is not a mistake merely because many unused edges remain undecided.

## 5. Puzzle existence and uniqueness

Puzzle = grid size plus clue matrix, independent of any working state.

- **Zero solutions**: complete search has proved no valid selected-edge set.
- **Exactly one**: complete search has found a valid set and ruled out every distinct second set.
- **Two or more**: a second **different edge set** is found; no need to enumerate the rest.
- **Unknown**: search incomplete due to timeout, memory, interruption or unsupported size. A found witness can be reported separately as “at least one solution known”; this is not proof of uniqueness.

Both clockwise and counterclockwise traversals of one cycle, and any different starting vertex along it, denote the **same** solution. The exclusion constraint for uniqueness must prohibit the entire previously found selected-edge assignment, not merely one starting point or travel direction.

When a generator starts from a known loop, that loop is a witness of existence but is not evidence of uniqueness.

## 6. Candidate algorithm families and pitfalls

- Local propagation can force edge inclusion/exclusion from clue and vertex saturation. Sound propagation is useful but often incomplete.
- Backtracking can exhaustively branch on unknown edges, provided pruning is sound and the global one-loop property is checked.
- SAT/SMT/constraint methods can express clue and vertex constraints, but **must** add a correct connectivity/circuit constraint or reject disconnected models and search again.
- One possible SAT approach is incremental counterexample blocking: find a locally valid model, check the global cycle; if invalid, add a constraint excluding that model or a sound subtour-elimination cut. Simply accepting the first 0/2-degree model is wrong.
- Some models encode a directed circuit with vertex self-loops for unused vertices, but this is only a candidate encoding until all its assumptions are checked.
- An inside/outside representation needs special attention to closed boundaries, holes and touching regions. It cannot be assumed to enforce a single cycle without proof.
- SAT UNSAT, solver UNKNOWN, and application-level validation failure are different statuses.

## 7. Important edge cases

A. 1x1 with no clue has one legal cycle, despite an empty clue set.
B. 1x1 with clue 0 or 3 is unsatisfiable: its only possible nonempty cycle uses all four edges.
C. 1x2 all-blank has three cycles, not one.
D. 2x5 allows two separated 2x2 perimeter loops with degree-2 vertices and locally valid clues (see fixture catalog).
E. Closing a small loop can leave further selected components outside it.
F. Different traversals of one cycle are not separate solutions.
G. Fully correct selected edges with no X markings are still solved.
H. A finite timeout cannot prove zero or exactly one solution.
I. Empty selected edges and degree-4 junctions are invalid final solutions.
J. The border has corners/degree-2 vertices and must be indexed without wrapping around.
K. In-progress edge sets may contain endpoints of degree 1; this is not itself an invalid **partial** state.

## Evidence

The rules agree with [Nikoli](https://www.nikoli.co.jp/en/puzzles/slitherlink/). The disconnect between local edge constraints and single-loop enforcement is explicitly described by [OptiLog](https://hardlog.udl.cat/static/doc/optilog/html/optilog/use-cases/slitherlink/modelling-slitherlink.html); see also [Copris](https://cspsat.gitlab.io/copris-puzzles/slitherlink/index.html). The derived invariants and proposed API boundaries have **not** been tested in executable code.
