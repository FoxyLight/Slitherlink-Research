# 01 — Rules and mathematical specification

**Research date:** 2026-10-29  
**Status:** Rules sourced; mathematical model established; future edge-case tests unexecuted.

## Established rules

Nikoli's official rules say to connect adjacent dots by horizontal/vertical lines into **one loop**; numbered cells specify exactly how many of their surrounding edges belong to it; the path cannot cross or branch. Blank cells impose no clue count. The classic square-grid symbols are 0, 1, 2, and 3.

Source: [Nikoli: Slitherlink rules](https://www.nikoli.co.jp/en/puzzles/slitherlink/) (reviewed 2026-10-29).

The proposed game is based on the **standard square grid** for initial research. Hexagon, pentagon, and mixed tilings are optional variants, not part of the reference specification.

## Formal square-grid representation

For a board with R rows and C columns of **cells**:

- Vertices/dots: V = (R + 1)(C + 1).
- Possible edges: E = R(C + 1) + C(R + 1).
- Example: a 10 × 10 cell board has 121 vertices and 220 potential edges.
- Each final edge variable x_e is 1 if selected and 0 otherwise.
- Each clue n_c requires sum(x_e for the four edges around c) = n_c.
- Each vertex v must have either 0 or 2 selected incident edges.
- At least one selected edge must exist.
- **All selected edges must be connected**, yielding exactly one cycle.

Clues plus degree restrictions alone **do not** forbid two or more separate loops. A connected-component check or equivalent global constraint is essential.

The final solution differs from player working notes. An **excluded-edge X** helps a person solve the puzzle but is not required for a mathematically completed loop. Untouched edges not in the selected cycle are excluded implicitly in the final solution.

## Validity versus uniqueness

- **A valid solution** satisfies clue counts, vertex degrees, and single-cycle connectivity.
- **A puzzle with a unique solution** has exactly one distinct set of selected edges meeting all requirements.
- A generated loop does not prove uniqueness of the clues derived from it.
- A partial board may admit multiple completions even if the published puzzle has one.

For a uniqueness solver, discovering a second distinct valid solution is enough to reject the clue set; exhaustive enumeration of all solutions is unnecessary. If search terminates early or times out, the correct result is **unknown**.

## Typical deduction classes

1. Direct clue deductions (e.g., a zero rules out its adjacent edges).
2. Clue completion and saturation.
3. Vertex degree zero-or-two propagation.
4. Adjacent-clue patterns.
5. Avoiding premature subloops.
6. Longer-range loop continuity.
7. Inside/outside parity or region reasoning.
8. More complicated deduction chains.

These are categories for later solver/hint research, **not** a finalized hierarchy.

## Difficulty is not a theorem about runtime

Difficulty labels used in apps are not standardized. Actual human difficulty can be affected by the number and depth of deductions, availability of alternative moves, spatial dependencies, familiarity with patterns, and presentation. Solver processing time alone is not an acceptable quality metric.

## Test cases still required

- Valid one-loop case; unsatisfiable case; multi-solution case.
- Two loops that satisfy all local vertex degrees.
- Closed cycle while additional required edges remain.
- Blank clue cells and edge/boundary corner cases.
- A solved puzzle with excluded edges left unmarked.
- Duplicate candidate loops and empty-selected-edge case.
- Timeouts or partial enumeration explicitly classified as unknown.

**No tests have been executed.**

## References

- [Nikoli: Slitherlink](https://www.nikoli.co.jp/en/puzzles/slitherlink/)
- [Simon Tatham's Loopy documentation](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/doc/loopy.html)
- [Miyauchi & Tanaka (2020), Solving Slitherlink with FPGA and SMT Solver](https://doi.org/10.2197/ipsjjip.28.959)
