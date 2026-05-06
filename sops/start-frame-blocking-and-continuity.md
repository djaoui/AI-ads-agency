# SOP: Start Frame Blocking + Sprite Continuity Across Cuts

When generating per-cut start frames, each SF must be planned as part of a SEQUENCE — not as a standalone image. The audience reads continuity from cut to cut. Inconsistent sprite positions, character orientations, or camera POVs across cuts break the story.

## Mandatory thinking checklist before writing each SF prompt

For each shot, explicitly answer:

1. **Sprite positions** — where is each subject (character, vehicle, props) located in 2D frame space and 3D world space?
2. **Sprite orientations** — which way is each subject FACING? Direction of travel?
3. **Camera POV** — where is the camera? What's its angle? How does the camera see each sprite?
4. **What the viewer perceives** — given camera + sprites, what does the audience SEE? Side view? Front view? Rear view? Over-shoulder?
5. **Continuity from previous shot** — if character was at point A facing right in shot N-1, where do they need to be at start of shot N? Did the camera move, did the character move, or both?
6. **Continuity to next shot** — does this SF set up the next cut cleanly?

## Common sprite-blocking failures (caught from Globe-Trotter v6 attempt 1)

- **Shot 1**: car parked nose-on facing camera, character walking toward car's front — but next shot has no car at all. Discontinuity.
- **Shot 2**: character walking toward camera with NO setting reference to shot 1. Could be a different street.
- **Shot 3**: car suddenly visible from REAR as if character has reversed direction. Geometric impossibility.

## How to write blocking-coherent SF prompts

Use staging language a film director would use:

```
Wide angle from across the street looking east toward the brownstone stoop on the left side of frame and the parked Town Car on the right side of frame.
The character has just descended the bottom step, facing screen-right, mid-stride toward the parked Town Car which sits at the kerb in the right third of frame.
The chauffeur stands beside the open rear door, facing screen-left toward the approaching character.
Direction of travel: character moves screen-left to screen-right.
```

vs the failure mode:
```
Character walks down brownstone street with case. Town Car visible. Composed.
```

The first version forces NB Pro to render a coherent geometric scene; the second leaves it to the model's defaults.

## Pass previous SF as reference for next SF (chained generation)

To enforce continuity, when generating shot N's SF, pass shot N-1's SF as one of the input images to NB Pro Edit alongside the standard product/character refs. Prompt: *"Continue from the moment shown in image 1 (previous shot). The character is now [position change], camera has moved to [new POV]."*

This costs nothing extra (NB Pro accepts up to 10 images) and dramatically improves cut-to-cut coherence.

## Decision rule: new SF vs extend video

| Condition | Choice |
|---|---|
| Framing changes significantly (wide → CU, side → front, etc.) | New SF + new Seedance call |
| Scene/setting changes (NY → SBH) | New SF + new Seedance call (chained via reference_videos for style) |
| Lighting changes meaningfully | New SF |
| Camera move stays smooth (dolly-in continues, pan continues) | Extend within ONE Seedance call (single shot longer) |
| Same action continues in same setting at same framing | Extend within one Seedance call |
| Internal cut within a single environment with same character | Multi-shot prompt within one Seedance call (still uses ONE start frame) |

## When to ALSO pass previous video as reference for style continuity

When chained Seedance gens are used (shot N references shot N-1 video output), pass the previous shot's MP4 as `reference_videos[0]` in addition to the new SF. This locks:
- Color grade
- Lighting palette
- Character identity (carried forward via the video reference)
- Motion rhythm

The new SF anchors composition; the previous-video reference anchors continuity. Both together = strongest control.
