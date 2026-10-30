# 03 — Player experience and creative options

**Date:** 2026-10-29  
**Status:** Interaction alternatives and hypotheses; no design or accessibility implementation is approved.

## Design goal

Let the player focus on the puzzle. Minimize accidental input, repeated corrections, visual overload, and uncertainty about board state.

## Control candidates

| Control | Possible benefit | Possible cost | Test requirement |
| --- | --- | --- | --- |
| Tap to cycle unknown → line → X | Discoverable, compact | Repeated tapping; cycles may be error-prone | Measure corrections per 100 edge edits |
| Separate line and exclusion tools | Predictable mass marking | Mode-switch friction | Measure speed and mode errors |
| Drag to connect segments | Efficient flow and satisfying gestures | Overshooting; ambiguity with pan/zoom | Compare mis-selections and corrections |
| Magnified/offset edge selector | Precision on dense boards | Slower and more UI complexity | Compare dense-grid error rates |
| Cursor/directional input | Motor/accessibility alternative | Extra steps | Test keyboard/switch usability |

**Working hypothesis (not decision):** support precise tap entry first, then consider drag and alternative controls if actual usability tests justify them.

## Board and visual design issues

- Use readable clue text and unmistakable line, excluded, and unknown states.
- Do not rely only on color for edge meaning; support contrast and color vision differences.
- Investigate adaptive cell size, pinch zoom, pan limits, hit detection, and orientation changes.
- On a dense board, individual logical edges may be much closer than standard Android's recommended 48dp touch targets. Explore alternative interaction and enlarged selection approaches, rather than claiming all edges meet that target.
- Ensure animation and vibration are optional; no visual theme has been chosen.
- A custom-drawn board needs intentional accessibility semantics, not just visually large buttons.

Sources: [Android touch target guidance](https://support.google.com/accessibility/android/answer/7101858), [Compose gesture behavior](https://developer.android.com/develop/ui/compose/touch-input/pointer-input/understand-gestures).

## Assistance levels worth investigating

1. **Minimal:** the player's marks and essential game feedback.
2. **Guided:** highlight currently satisfied clues and directly observable contradictions.
3. **Explanatory:** identify a logically forced deduction and explain why, without arbitrary answer disclosure.

Keep distinct:
- An invalid player state (e.g., clue oversatisfied).
- A state logically impossible to complete.
- A mark that differs from a hidden reference solution.

These are different notions of “mistake.” Do not silently conflate them.

## Feature candidates

**Foundation candidates:** local resume, clear edge state, undo/redo, responsive controls, optional exclusion marks.

**Research priorities:** instructional hints, tentative markings, automatic trivial deductions, human-calibrated difficulty progression, and consistent saving.

**Later possibilities:** technique-based learning collections, daily offline puzzles, custom themes, non-square grids, achievements, and multi-device syncing.

Do not choose a feature simply because competitors have it, and do not mistake larger feature counts for higher player satisfaction.

## Experiment protocol: touch input

- Compare the control methods on the same puzzles and grid sizes on real devices.
- Capture device size, density, Android version, orientation, and zoom.
- Record edge edits, accidental marks, corrections, interaction time, and subjective preference separately.
- Test 1-handed and 2-handed use, pan/zoom conflicts, interrupted gestures, and accessibility settings.
- Avoid treating a single player's preference as general evidence.

**No touch-control prototypes or player tests have been conducted.**
