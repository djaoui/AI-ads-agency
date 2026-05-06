# Prompt Library — Staged Pipeline (Face → Styling → Location → Scene)

**Source of truth for the foundational anchor workflow.** The master `SKILL.md` is the *what / why*; this library is the *how / exact prompts*.

## Order matters — execute in sequence

| # | Step | Output | Why |
|---|---|---|---|
| **1** | ALPHA FACE ANCHOR | Single hero face/portrait, neutral context | Locks identity. All subsequent shots reference this. |
| **2** | MASTER ANCHORS WORKFLOW | Combined face + body + product anchor set | Connects face-to-scene; canonical anchor set per project. |
| **4** | STYLING MASTER | Wardrobe per state (NY-state, transit-state, destination-state etc.) | Solves wardrobe drift. Each per-cut SF references the matching styling master. |
| **5** | LOCATION CLEANING | Clean location plate without subjects | Provides "place" component independent of character. |
| **6** | MASTER SCENE BUILDER | Composes per-cut SFs from face anchor + styling master + location plate + product refs | The proper way to build per-cut start frames. |

## Validated prompt patterns
See `WINNING-PROMPT-PATTERNS.md` for validated NLP / JSON / Seedance prompt structures.

## Why this order matters
Skipping wardrobe master (4) = wardrobe drift across cuts. Skipping location plate (5) = setting drift. Verified failure pattern from prior iterations.
