# 05 — Android technology, distribution, and intellectual property

**Reviewed:** 2026-10-29  
**Status:** Preliminary comparison; no stack, public name, minimum Android version, licensing model, or monetization has been selected.

## Implementation alternatives

| Approach | Benefits | Limitations | Preliminary position |
| --- | --- | --- | --- |
| Kotlin + Jetpack Compose + custom Canvas | Native app UI, custom drawing, gesture support, Android tooling | Must implement hit testing, zoom/pan, accessibility semantics | Leading candidate for a technical experiment |
| Kotlin + custom View/Canvas | Direct low-level touch/render control | More manual UI/lifecycle work | Strong alternative |
| Godot | Convenient 2D game prototyping and visuals | Extra engine layer for an otherwise static logic grid | Worth comparing if tactile/animated presentation becomes central |
| Flutter | Cross-platform UI and painter tools | Extra framework/language without a confirmed cross-platform requirement | Lower research priority |

A likely separation of concerns is Android UI / board rendering / platform-independent puzzle logic / local persistence. This is an architectural **candidate**, not an approved design.

Sources: [Compose drawing/Canvas](https://developer.android.com/develop/ui/compose/graphics/draw/overview); [Compose gestures](https://developer.android.com/develop/ui/compose/touch-input/pointer-input/understand-gestures); [Godot Android export](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_android.html).

## Accessibility and mobile behavior

- Standard Android touch-target guidance suggests at least 48dp for touchable controls; a dense grid may be geometrically unable to allocate separate targets of that size without zoom or an alternative input technique.
- Test edge targeting, pinch/pan conflicts, accessibility semantics, high-contrast and color-independent states, and interruption/resume.
- Define a minimum supported Android version only after considering devices and testing needs.
- Keep the puzzle engine independent of UI frameworks so logic can be tested without a phone.

Source: [Android accessibility touch target guidance](https://support.google.com/accessibility/android/answer/7101858).

## Current Google Play requirements

The official Google Play target API policy states that **new Android phone/tablet apps and app updates submitted beginning August 31, 2026 must target API level 36 (Android 16) or higher**. This is different from choosing the minimum supported Android version.

Source: [Google Play target API rules](https://support.google.com/googleplay/android-developer/answer/11926878) (reviewed 2026-10-29).

New apps published to Google Play must use Android App Bundles; check the current packaging requirements near release.

For certain personal developer accounts created after November 13, 2023, Google requires a closed test with at least 12 continuously opted-in testers for 14 days before application for production access. This is **account-dependent**, not automatically applicable to every developer.

Source: [Google Play testing requirements](https://support.google.com/googleplay/android-developer/answer/14151465).

Google Play's Data safety form and privacy policy are required for published applications even when no user data is collected (with noted internal-testing exemptions).

Source: [Google Play Data safety requirements](https://support.google.com/googleplay/android-developer/answer/10787469).

All platform requirements should be checked again before release.

## Branding and rights

Nikoli publishes the official Slitherlink rules and identifies the game as its original puzzle. A secondary Japanese trademark listing describes Nikoli's SLITHERLINK/スリザーリンク registration (e.g., entry 5056858). This is a **legal review signal**, not a complete global trademark clearance.

Sources: [Nikoli official description](https://www.nikoli.co.jp/en/puzzles/slitherlink/); [Japanese trademark listing (secondary)](https://patent-i.com/tm/applicant/0020124/).

Do not use “Slitherlink” as a final product name without checking actual registrations in relevant markets and seeking appropriate legal advice where needed. The repository name is a research label.

Respect original artwork, puzzle collections, source code, branding, and license requirements. Familiar mechanics do not grant permission to copy expressive content. No legal clearance has been performed.

## Privacy and business models (unselected)

Offline-first local saves could reduce server cost, account requirements, and data exposure, but privacy obligations still apply. Candidate release models: fully free, one-time paid, free starter plus puzzle packs, or optional one-time unlock. Advertising and subscriptions add interruption, privacy, support, and business complexity.

**No monetization model selected.** Prioritize player experience and evidence over assumptions about revenue.
