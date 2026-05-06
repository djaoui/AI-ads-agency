# Prompt Library — Staged Pipeline (Face → Styling → Location → Scene)

**Source of truth for the foundational anchor workflow.** These 8 numbered steps are the rigorous pipeline for any ad needing character + product + scene fidelity. The master `SKILL.md` is the *what / why*; this library is the *how / exact-prompts*.

## Order matters — execute in sequence

| # | Step | Output | Why |
|---|---|---|---|
| **1** | ALPHA FACE ANCHOR | Single hero face/portrait of the character, neutral context | Locks identity. All subsequent shots reference this. |
| **2** | MASTER ANCHORS WORKFLOW | Combined face + body + product anchors, validated together | One canonical anchor set per project. |
| **3** | CASTING DIRECTOR | Codifies WHO the character is: archetype, demographic, attitude, posture | Brand-bible-aligned casting decision. |
| **4** | STYLING MASTER | Wardrobe per state (NY-state, transit-state, destination-state etc.) — locked once per state | Solves wardrobe drift. Each per-cut SF references the matching styling master. |
| **5** | LOCATION CLEANING | Clean / generate location plate without subjects | Provides the "place" component independent of character. |
| **6** | MASTER SCENE BUILDER | Composes the SF: face anchor + styling master + location plate + product refs | The proper way to build a per-cut start frame. |
| **9** | CHARACTER CONSISTENCY | Check + enforce identity continuity across shots | Run before submitting any video gen. |
| **11** | NEXT SHOT MODE | Extends to subsequent shots while preserving consistency | The proper way to generate shot N+1, N+2, etc. |

## Validated prompt patterns
See `WINNING-PROMPT-PATTERNS.md` for the validated NLP / JSON / Seedance patterns that win for each model.

## Why we drifted (and why we're returning)
Earlier iterations skipped steps 3-4-9 and jumped straight to scene-building. Result: wardrobe drift, character inconsistency across cuts, repeated regeneration costs. Staged pipeline costs slightly more upfront ($0.50–$1 in NB Pro frames for masters) but eliminates ~80% of downstream rework.

## How SKILL.md uses this library
Every workflow that involves a character + scene MUST follow steps 1→11 before submitting Seedance. Reference these step files for the exact prompts; use `SKILL.md` for the high-level workflow rules (two-pass, no char refs in Seedance, no audio gen, etc.).
