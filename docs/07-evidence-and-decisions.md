# 07 — Evidence, decisions, and research log

**Created:** 2026-10-29

This log records what is **actually established** versus what is only proposed. Update it whenever an experiment, research correction, or project decision changes the record.

## Binding project instructions

| Date | Record | Status |
| --- | --- | --- |
| 2026-10-29 | Repository FoxyLight/Slitherlink-Research is the authoritative location for project research documentation | User direction |
| 2026-10-29 | Do not use SPBT for this project | Explicit user direction |
| 2026-10-29 | Research and preparation first; no app, production generator, or repository replacement | Current scope |

## Established facts with primary evidence

| Topic | Evidence | Scope |
| --- | --- | --- |
| Classic Slitherlink rules | [Nikoli official rules](https://www.nikoli.co.jp/en/puzzles/slitherlink/) | Defines one nonbranching/noncrossing loop and clue counts |
| Competitors already provide many advanced options | [Ejelta](https://play.google.com/store/apps/details?id=com.ejelta.slitherlink.ads), [Conceptis](https://play.google.com/store/apps/details?id=com.conceptispuzzles.slitherlink), [Tatham Android](https://play.google.com/store/apps/details?id=name.boyle.chris.sgtpuzzles) | Developer claims, not direct testing |
| Research methods for Slitherlink solving exist | [2020 journal article](https://doi.org/10.2197/ipsjjip.28.959), [Slinker](https://github.com/timhutton/slinker) | Literature/implementation descriptions only |
| Compose has custom drawing/gesture mechanisms | [Graphics](https://developer.android.com/develop/ui/compose/graphics/draw/overview), [Gestures](https://developer.android.com/develop/ui/compose/touch-input/pointer-input/understand-gestures) | Capability documentation, not a performance benchmark |
| Play API target requirement in Oct 2026 | [Google Play policy](https://support.google.com/googleplay/android-developer/answer/11926878) | New app submissions must target Android 16/API 36+ |
| Certain personal Play accounts require a closed test | [Google Play policy](https://support.google.com/googleplay/android-developer/answer/14151465) | Account-dependent |
| Play Data safety documentation required | [Google Play policy](https://support.google.com/googleplay/android-developer/answer/10787469) | Current published-app obligation |

## Mathematical facts versus experiments

**Derived formal requirements (not a measured experiment):**
- Selected edges must satisfy clue counts and vertex degrees (0/2).
- At least one edge must be selected.
- Selected edges must form one connected component.
- Uniqueness is a distinct property from existence of one valid solution.

**Not completed:**
- No code executed or benchmark results.
- No independently checked fixture catalog.
- No generated-puzzle yield or uniqueness measurements.
- No human difficulty calibration.
- No Android app installs, touch testing or accessibility audits.
- No final implementation, visual, naming, or monetization decision.

## Recommendations, not decisions

- Investigate a pre-generated validated puzzle catalog for the first playable build.
- Investigate Kotlin + Compose Canvas as a leading Android candidate.
- Test explainable hints, tentative lines and accurate touch input.
- Start with Experiment 01: formal rules, fixtures, and uniqueness feasibility.

## Required format for future records

Every future experiment should include:

- Date; objective; hypotheses and explicit success criteria set beforehand.
- Sources and exact versions; test fixtures and reproducible inputs.
- Hardware/OS/tool versions, commands, parameters and seeds where relevant.
- Observed raw results (successes, failures, timeouts and anomalies).
- Separate interpretation and confidence statement.
- Decisions actually approved; unresolved follow-up questions.
- Links to commits or files containing evidence.

**Change control:** Never rewrite an unexecuted plan as a successful test. If a source claim is corrected, preserve what changed and why.

## Next entry

**Pending:** Design the Experiment 01 test fixture catalog and choose independent reference-verification methods.
