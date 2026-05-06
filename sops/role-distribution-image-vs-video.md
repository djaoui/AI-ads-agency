# SOP: Role Distribution — Image Model vs Video Model

**Critical learning** (validated 2026-05-06 on Globe-Trotter v5): 70% of visual identity is encoded in the start frame. Adding more text to the video prompt can't override what the image shows. **Stop trying to fix scene-level visual issues with Seedance prompts. Fix them at the start frame.**

## Role split

| Concern | Owned by | Method |
|---|---|---|
| **Atmosphere** (light, time of day, weather, smoke, mist, wet/dry, palette) | Image model (NB Pro / Soul ID) | Generate or regenerate the start frame |
| **Environment cleanliness** (immaculate vs cluttered, brand-grade) | Image model | Start frame anchors this |
| **Character look** (face, age, wardrobe, posture, attitude) | Image model | Reference images + start frame |
| **Product fidelity** (materials, hardware, colour) | Image model + Seedance refs | Product packshots in `reference_images[]` |
| **Composition** (framing, depth, blocking) | Image model | Each shot's start frame |
| **Action / motion** | Video model (Seedance) | Action verbs in per-shot prompt |
| **Camera move** (push-in, dolly, tilt, pan) | Video model | Per-shot direction |
| **Dialogue / facial emotion timing** | Video model | Per-shot dialogue + reaction beats |
| **Cuts / pacing** | Video model | Multi-shot prompt structure |

## Implication: per-cut start frames

For ads where shots have meaningfully different settings, lighting, or composition, **generate ONE start frame per cut** rather than one per multishot. Cost: ~$0.14 per NB Pro frame. Trivial relative to the visual control gain.

Example: 7-cut ad → 7 NB Pro start frames → pass all 7 as `reference_images[]` to Seedance with explicit per-shot anchoring ("Image 1 = first frame of shot 1; Image 4 = first frame of shot 4"). Seedance multishot reference-to-video accepts up to 9 image references — fits naturally.

## New prompt density rule

With per-cut start frames carrying visuals, the Seedance prompt shrinks to:
- Per-shot: 1 sentence action + camera move + dialogue (if any). ~30-50 words.
- Negatives: 5-8 max, only failures specific to action/motion (not visuals — those are in the SFs).
- Total prompt budget: ~250 words for a 15s multi-cut ad.

## Anti-pattern (what we did v3-v5)
- 1500-word prompts trying to re-describe visuals already encoded in the start frame
- Stacking 30+ negatives from accumulated past failures
- Restating brand voice in every shot

## Pattern (going forward)
- Each problem visible in the output that's "scene-level" (atmosphere, environment, character look) → fix the start frame
- Each problem that's "motion-level" (ghost physics, missing action, wrong pacing) → fix the prompt
- Don't mix the two channels

## Workflow
1. Storyboard the cuts (Creative Director writes shot list)
2. Generate per-cut start frames via NB Pro using a single approved character reference + product refs
3. QA each start frame against Brand Bible BEFORE generating video
4. Submit Seedance with all SFs as references + lean per-shot prompt
5. Run mechanical QA + Creative Director filter
6. If a shot has a SCENE-LEVEL defect → regenerate THAT start frame, not the video

## Where brand guardrails live

Confirmed by user: **brand-bible enforcement happens primarily at the start-frame stage, not in the video prompt.** Atmosphere, environment cleanliness, character composure, palette, mood — all encoded in the SF. The video prompt only enforces motion-level guardrails (visible agents, no ghost physics, dialogue tempo). This means:
- The Bible-Builder agent's most important downstream consumer is the **NB Pro start-frame prompt writer**, not the Seedance prompt writer
- If a video output violates the Bible visually, the fix is upstream at the SF, not at the video prompt
- Per-cut SFs let you inspect and approve each shot's brand compliance BEFORE spending video credits
