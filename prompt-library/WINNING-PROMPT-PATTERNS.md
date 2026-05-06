# Winning Prompt Patterns
*Validated through AGS Stiliani production — April 2026*

---

## 1. NANO BANANA STANDARD — NLP Numbered Reference Format

**Confirmed:** Produces best start frame results. "Results is amazing."

**Structure:**
```
Generate a photorealistic [style] image by seamlessly synthesizing the provided reference images.

Follow these strict reference constraints:
1. Environment & Lighting (Image 1 — Location Master): [exact spatial geometry, lighting direction, shadow quality]
2. Wardrobe & Body (Image 2 — Outfit Anchor): [exact anatomical proportions, outfit items, what must not be obscured]
3. Identity (Image 3 — Face Anchor): [exact facial geometry, hair, skin, eyes, expression]
4. Footwear/Product (Images 4, 5, 6, 7 — [Product] Packshots): Override any existing footwear. Reproduce [every specific detail] with zero deviation.

Creative Direction & Pose:
[Framing. Character position. Key action. Specific hand/body placement. What is NOT present.]

Technical Specifications:
[Photorealism level. Light quality. Focus. Atmosphere. Shot-on-iPhone if UGC.]

Negative: [comma-separated list]
```

**Key rules:**
- "wearing/holding" relationship must be explicit when body part interacts with product (e.g. "feet wearing shoes" not just "sandal on cobblestone")
- Always attach full body reference image when any body part is visible — ensures skin fidelity
- "No phone visible — the phone is the camera" when doing selfie start frames
- Add "two hands on phone" to negative to prevent mirror photo glitch

---

## 2. NANO BANANA PRO — JSON Format

**Confirmed:** JSON wins over NLP in Pro version.

**Structure:**
```json
{
  "scene": "[environment, lighting, mood, people in background if UGC]",
  "subject": {
    "source_of_identity": ["Face.png", "Full Body.png"],
    "source_of_style": ["STYLING.png"],
    "description": "[age, skin, hair, expression, pose, what they hold/wear, interaction with product]"
  },
  "camera": {
    "source_of_location": ["L1.png"],
    "description": "[angle, lens type, framing, movement quality, device type]"
  },
  "props": {
    "source_of_style": ["product-side.webp", "product-top.webp", "product-3/4.webp", "product-detail.jpg"],
    "description": "[physical appearance of prop only — no action, no relationship to subject]"
  },
  "negative": "[comma-separated list]"
}
```

**Key rules:**
- `props.description` = what it IS only (materials, colors, hardware details)
- Subject/character interaction with props belongs in `subject.description`
- Never mix action and appearance in the same block
- **ONE character per generation.** Never upload face/body refs for more than one character in the same generation. NB Pro merges or confuses mixed identity sets. Anchor to the PRIMARY character only (1 face ref + 1 body ref). All secondary characters described in text only (age, clothing, posture). Applies to NB Standard and NB Pro equally.

---

## 3. SEEDANCE 2 — Numbered Image References

**Confirmed:** "Truly amazing. Beating Kling 3 on all aspects."

**Structure:**
```
Starting from the exact composition of Image [N] — a [character from Image 1, outfit from Image 2+3] in [location description] with [lighting]. She films herself in selfie mode — camera faces directly toward viewer, not a mirror. She holds one [product from Images 5,6,7,8] in one hand, raised toward the camera. The other hand rests naturally. Slight natural camera sway, casual framing, feels real and unpolished.

She looks directly into the camera, relaxed and natural, like talking to a friend. While speaking, she [product interaction — rotate, show detail, tilt to reveal sole].

Lock in all [product] details from Images [5,6,7,8] — [exact physical specs] — must match reference images 1:1. Do not invent any [product] detail.

Dialogue ([tone], [accent], ~[duration]):
"[dialogue trimmed to duration × 2.5 words/sec]"

[Camera/lighting/style global notes]. Organic creator content, not a staged ad.

Negative: visible phone, mirror, mirror reflection, extra hands, extra arms, extra limbs, [style violations], wrong [product] design, invented [product] details
```

**Key rules:**
- Mark start frame explicitly in the interface (game changer for product fidelity)
- Do NOT upload location as a separate image (causes 2s empty scene)
- Start frame encodes location — describe location verbally only
- Numbered image references (Image 1, Image 2...) not @element syntax
- "must match reference images 1:1" + "Lock in all details" = mandatory for product fidelity
- "visible phone, mirror" in negative prevents mirror photo glitch
- "extra hands, extra arms, extra limbs" in negative prevents phantom limb glitch
- Max duration: 15s (~37 words). 8s recommended for testing (~20 words).

**Image upload order (confirmed):**
1. Face anchor
2. Full body anchor
3. Outfit/styling anchor
4. Location (verbal only — do not upload)
5–8. Product packshots (all angles)
9. Start frame (mark as start frame in interface)

---

## 4. KLING 3 — Single Shot

**Confirmed:** Works for individual shots with short, precisely timed dialogue.

**Structure:**
```
[Character @mention] [action/behavior description]. [Camera/framing description].

She speaks with a [accent] accent: "[dialogue in natural quotes]." [Natural behavior — blink, glance, etc.] [One non-model quality signal.]

@[location] [light description]. [Device/aesthetic signal]. [Format signal].

[Ambient/audio description]. No music.

Negative: [list]
```

**Key rules:**
- Dialogue in natural quotes with accent inline — NOT `[Narrator: female, accent...]` format
- Max ~10 words per 4s, ~12 words per 5s, ~15 words per 6s
- "Not a pose" / "organic creator content" = critical UGC signals
- Never use "cinematic" or "fashion campaign quality" for UGC
- @mention must be used in prompt text to activate reference
- Start frame required when @mentions are present (even T2V)

---

## 5. KLING 3 — Multi-Shot

**Confirmed:** Works but with limitations on audio across shots.

**Structure:**
```
GLOBAL: [UGC format signals]. @[location] background static throughout. @[product] physically locked to [body part].

---
Shot 1 — [duration]s
[Scene description. Camera. Character action.] [If no dialogue: "She does not speak — lips closed."]
Audio: [dialogue in quotes with accent] OR Ambient only: [sounds]. No music.

---
Shot 2 — [duration]s
[...]

Negative (global): [list]
```

**Key rules:**
- Keep dialogue SHORT per shot — fits duration × 2.5 words/sec
- For walking shots: NO dialogue (character lip-syncs instead of walking)
- Global style block at end, not repeated per shot
- Shot N labels trigger auto-splitting
- Max 15s total (6 shots). Prefer 3 shots for reliability.
- Audio misalignment increases with more shots — prefer single-shot generations for dialogue-heavy content

---

## 6. AUDIO DIRECTION SYNTAX

Use labeled prefixes to separate audio layers. Cleaner than freeform sentences — easier to strip or swap per model.

```
SFX: [discrete sound events — footsteps on cobblestone, bag zipper, coin drop]
Ambient: [background atmosphere — Mediterranean street hum, light breeze, faint crowd]
Dialogue: "[spoken words in quotes, accent inline]"
```

**Rules:**
- SFX = discrete, intentional sounds tied to action
- Ambient = background texture/atmosphere
- No music unless explicitly needed — "No music." at end
- Seedance 2 / Kling 3: use all three layers when relevant; strip to Dialogue-only for tight confessionals

---

## 7. LENS VOCABULARY — FORMAT FINGERPRINT SHORTHAND

Use lens type as a single token that carries format intent:

| Lens | Aesthetic | Use for |
|------|-----------|---------|
| iPhone lens | Everyday naturalistic, no distortion | UGC, confessional, selfie |
| GoPro | Wide, immersive, barrel distortion | Action, POV, outdoor |
| 85mm | Shallow DoF, subject isolated, editorial | Brand, product close-up |
| 24mm | Slight wide distortion, environmental | Travel, lifestyle, documentary |

**In Format Fingerprint:** naming the lens auto-enforces the aesthetic — "iPhone lens" in the Fingerprint blocks "cinematic" drift.

---

## 8. DURATION PRECISION GUIDE

| Duration | Structure | Use |
|----------|-----------|-----|
| 4s | One action | Single gesture, product reveal, reaction |
| 6s | Setup + action | Enter scene + do thing |
| 8s | Mini arc (setup + action + reaction) | Short confessional beat, walking moment |

**Multi-shot color consistency:** When generating separate single shots that will be assembled, add to each prompt:
`Match color grade to Shot [N].`
Anchors all shots to the same grade before post-production.

---

## UNIVERSAL ANTI-SLOP RULES

| Signal to INJECT | Signal to EXCLUDE |
|-----------------|-------------------|
| Shot on iPhone / handheld | Cinematic |
| Natural available light | Studio lighting / ring light |
| Slight imperfect framing | Perfect framing / centred |
| Tourists / people in background | Empty sanitised street |
| Casual stride / natural behavior | Model walk / runway pose |
| Creator speaks to phone like a friend | Fashion campaign eye contact |
| Organic creator content, not a staged ad | Fashion campaign quality |

---

## 9. SEEDANCE 2.0 — VIDEO EXTENSION & STYLE REFERENCE

*Validated from research: May 2026. Sources: OpusClip, WaveSpeedAI, Seedance docs, TopView docs.*

### Two distinct modes — do not conflate

| Mode | What to upload | What it does | Use case |
|---|---|---|---|
| **Extension** | The clip to continue (≤15s) | Generates forward continuation — full temporal coherence | Shot A → seamless Shot A' |
| **@Video style ref** | Any reference clip | Copies camera language, motion, lighting, color grade into new content | Shot A style → apply to Shot B |

### Extension syntax
```
Extend @Video1 by [duration]s. [Describe what happens next — camera move, subject action.]
```
Duration parameter = the extension length only (e.g. 5s), not total.

### Style reference syntax
```
Reference @Video1 color grade and handheld camera movement. [New scene description.]
```
```
[Scene description.] Match color grade, lighting, and camera movement style of @Video1.
```

**Critical rule:** Always name explicitly what to borrow. "@Video1" alone is underspecified and model may ignore it.
- ✅ "Reference @Video1 color grade and lighting"
- ✅ "Reference @Video1 handheld camera movement style and rhythm"
- ❌ "Reference @Video1" (too vague — will likely be ignored)

### Upload order & naming
- @VideoN = nth video in upload order (first uploaded = @Video1)
- @ImageN = nth image in upload order
- @AudioN = nth audio in upload order
- Numbers are positional, not semantic

### TopView implementation
- Works in Omni Reference mode (upload video ref + write prompt)
- No dedicated "Extend" button — handled entirely via upload + syntax
- Max **3 video clips** as references, **total ≤ 15s** combined across all uploaded videos
- Max **12 total files** (images + videos + audio combined)
- @mention syntax: `@Video1`, `@Video2`, `@Video3`

### Multi-shot workflow (street interview / shot-reverse-shot)
1. Generate Shot 1 fresh (no reference video)
2. Download Shot 1 → upload as reference for Shot 2
3. In Shot 2 prompt: `Reference @Video1 color grade, lighting quality, and handheld camera movement style.`
4. Repeat — each shot references the previous for chain consistency
5. **Do NOT chain more than 4-5 shots** — color drift accumulates after 3-6 extensions

### Constraints
- Aspect ratio must match source for extension (mismatch breaks continuity)
- Chain drift: color/lighting may shift after extension 3-4; use "Match color grade to Shot 1" as a backup anchor
- Start frame still recommended for all shots even when using video reference
- Do NOT use extension mode for different framings (wide→tight) — use style reference mode instead

---

## WHAT CONSISTENTLY FAILS

- `[Narrator: female, French-Italian accent, low register...]` → Kling ignores structure, uses it as VO not character speech
- Dialogue too long for shot duration → audio misalignment, rushed speech, lip sync failure
- "Cinematic fashion campaign quality" → brand video aesthetic, not UGC
- Multi-shot with all dialogue → audio bleeds between shots
- JSON format in Nano Banana standard → use NLP
- Product-only description without "wearing/holding" relationship → wrong render
- @element syntax in Seedance 2 → use numbered Image references
- Separate location image in Seedance 2 → empty scene at start
- Showing phone in start frame → mirror photo glitch
- Hair-touching or complex arm gestures → phantom limb generation
