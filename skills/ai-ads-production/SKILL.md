---
name: AI Ads Production Skill — Master SOP
description: Portable consolidated playbook for AI-generated paid social and brand ads. Two-pass workflow, format fingerprints, post-production rules, model selection, pricing, and validated workarounds for known failure modes. Survives device loss via iCloud sync. Source of truth.
version: 2026-05-06
---

# AI ADS PRODUCTION — MASTER SKILL

This consolidates 30+ memory files into a single portable SOP. Load on any new Claude Code install:
1. Place this file at `~/.claude/skills/ai-ads-production/SKILL.md` (or symlink from iCloud)
2. Restart Claude Code
3. Skill loads automatically into every session

---

## ⭐ DEFAULT PRODUCTION WORKFLOW — TWO-PASS (validated AGS Charys 2026-05-05)

**Identity comes from a START FRAME, not from passing character refs to Seedance.**

### Pass 1 — Identity stamp (NB Pro / Higgsfield Soul ID)
- Generate the start frame using ALL character refs (face + body)
- Single character: 2 char refs + 4 product refs = 6 (NB Pro max)
- Two characters: 4 char refs + 2 product refs = 6 (with explicit per-character labeling + anti-merge negatives — `character_left_NAME` / `character_right_NAME` blocks)
- Product only: 4 product refs

### Pass 2 — Motion render (Seedance 2.0)
- `reference_images[]` = **start frame + product refs only** — NO character face/body refs
- Prompt explicitly: *"Image 1 IS the first frame of the video. Render frame 1 to match Image 1 EXACTLY for character identity, framing, lighting and composition. Preserve identity from Image 1 across all subsequent frames."*

### Multi-shot extension
- Single Seedance call ≤15s with shot-by-shot prompt embedded — cheapest path
- Long-form / radical scene shifts: chain via `reference_videos[0]` carrying previous shot (identity + motion + grade transfer) + new start frame for new framing

### Anti-patterns
- ❌ Pass character face/body refs to Seedance → guarantees AI-feel motion
- ❌ Skip start frame entirely → loses identity continuity
- ❌ Two-pass while ALSO including char refs alongside the start frame → re-poisons motion pipeline
- ❌ Use Seedance 2.0 Real Human path without considering ID-verification friction

---

## HARD RULES — Never override

### 1. Never render text in video models
AI video models hallucinate text characters. Always burn brand handles, captions, CTAs via FFmpeg post-production. Validated failure: AGS Charys MS3 produced "@ancientgreesandals" instead of "@ancientgreeksandals" — $1.31 burned. **Generate visuals only; burn ALL text in post.**

### 2. `generate_audio: false` for all multi-shot or sound-sensitive ads
AI video audio bleeds across shots (footsteps in aerial shots, dialogue ghosts from ref videos). Set `generate_audio: false` on all cinematic/multi-shot Seedance calls. Build audio entirely in post:
- Per-shot ambient via AudioGen / MusicGen (text-prompted) or freesound.org public-domain
- Music underbed via MusicGen
- Foley/SFX via FFmpeg sine + library samples
- Dialogue via ElevenLabs voice clone or recorded VO
- Bleeps via FFmpeg sine wave at exact whisper-detected timecode

Exception: single-shot UGC where one ambient holds throughout (talking-head testimonial in one room).

### 3. Visible agent for every physical action
AI video models render the *result* of physical interactions without showing the *agent*. Always specify in prompt:
- ❌ "Door closes" → ✅ "Driver's gloved hand pushes door shut, hand stays on handle for a beat"
- ❌ "Case is placed" → ✅ "Both hands lift case and lower it onto the leather seat"
- ❌ "Curtain opens" → ✅ "Hand grasps the rope pull, tugs downward, curtain parts in response"

Validated failure: Globe-Trotter v3 — "Driver closes the rear door" rendered as door closing on its own (supernatural break in immersion).

### 4. Material anchoring (3+ places + negate alternatives)
Models hallucinate the more statistically-common material if target material is mentioned only once. For non-default materials (suede wedge sole, cork sandals, etc.):
- State target material in 3+ places: scene + dedicated `material_rule` block + props
- Per-component breakdown ("the wedge sole is suede; the footbed is suede; the strap is suede")
- Negate every common alternative explicitly: "NO leather, NO polished, NO wood, NO cork, NO plastic, NO vinyl, NO varnished, NO lacquered"
- End with "match reference images exactly for material AND construction"

Validated failure: AGS Charys SF3 v1 produced leather wedge sole instead of suede — propagated to MS3 video.

### 5. UGC must be multi-shot
Single-shot Seedance with 1-2 cuts is NOT enough for UGC ads. Real social-media UGC has constant cuts: medium → CU → cutaway detail → reaction → wide → CTA.
- Target 6–10 cuts in a 12–15s ad (~25 cuts/30s for lead-gen UGC)
- Mix shot types: wide / two-shot / MCU / CU reaction / B-roll cutaway (every 4-5s) / push-in / CTA close
- Every cut has explicit tilt direction (2–5° L or R, never level)
- Cutaways carry voiceover from previous shot

### 6. AI-uncanny gap = motion rhythm, not lip-sync
The AI feel comes from movement/expression rhythm + speed, NOT lip-sync issues. Don't pursue lip-sync solutions for the general motion problem.

Prompt explicitly for natural rhythm:
- "Speech with varied tempo: slows on key words, accelerates through filler, brief breath pause mid-sentence"
- "Facial micro-expressions cycle every ~0.5s — never holds one expression >1.5s"
- "Body micro-fidgets: weight shift, hair touch, collar adjust, gaze dart off-camera between sentences"
- "Reaction onset delayed by ~0.3s after stimulus, like real human processing time"

Don't over-negate "exaggerated reactions" — that flattens to AI poker-face. Specify the exact restrained-but-real reaction.

### 7. NB Pro on Atlas / Higgsfield: NLP, not JSON
Both formats accepted but lean NLP outperforms detailed JSON when references are strong. Detailed geometric language ("trapezoidal", "V-neck necklace shape") competes with what refs already show. Default to NLP first; escalate to JSON only if refs insufficient.

### 8. Always provide image/video links for visual QA claims
Every visual claim must include local path + open command + URL when available + reference comparison link. User verifies CV claims directly. Don't ask them to take visual claims on faith.

### 9. Validation gates are mandatory
- Start frame → user approval BEFORE any video generation
- Each completed shot → user review BEFORE next shot (first run)
- @Video1 chain: download completed → user approves → upload as ref → submit next
- "Submit all shots overnight" only valid for proven workflows, never first run

### 10. SOP development: iterate cheap API first
Never develop a new SOP on a slow queue platform. Use Atlas Cloud / Higgsfield API to validate workflow correctness, then move to quality platform if needed.

### 11. TopView credits — NEVER authorized
Unlimited mode (0 credits) only. Never credit mode. Verify Generate shows "0" before every click.

### 12. Project folder structure
- One project = one folder containing ALL files (images, videos, .md, refs, etc.)
- `_playwright-temp/` for browser-automation artifacts
- `_claude/` for working files / logs
- `_assets/`, `_generated/`, `_post/` for production stages
- Never mix project assets with tool/automation artifacts

### 13. Agent QA before presenting
Every agent output reviewed across 4 dimensions before showing user:
- Narrative logic (does A lead to B?)
- Social realism (would a real person actually do/say this?)
- Cultural authenticity (does this sound like a real [demographic], not a script?)
- Format fidelity (does this match reference pacing/style?)
Agent prompt must include: *"Before returning: read through as a director watching real footage. Check (a) narrative logic (b) social realism (c) cultural authenticity (d) format fidelity. Fix before returning."*

### 14. Reference count rules
| Platform / model | Image refs ceiling |
|---|---|
| NB Pro start frame | 6 (forces trade-off: 1ch+4prod / 2ch+2prod / 0ch+5prod) |
| Seedance 2 multishot | 9 (always 4 product + chars + 1 start frame) |

### 15. Chrome browsing → Playwright only
Never use computer-use for Chrome. Use Playwright MCP tools or local Playwright via Bash.

---

## STREET INTERVIEW UGC FORMAT FINGERPRINT
Reference videos: @theventureroom (1.1M), @cyclicalstore (300K)

- Avg shot duration: 1.5–2.5s (max 3s for hero reaction)
- 12–16 cuts per 30s
- No stabilisation — handheld float, 2–5° tilt off horizontal at all times
- Subjects offset (not centered), faces 60–75% of frame
- Cross-cut every ~2s between interviewer/interviewee
- Cutaways: macro on object during dialogue; brief reaction; push-ins
- Opening line is never a question — it's an action or implied stop
- Interviewer lines: 3–6 words max per shot
- Total spoken: 60–80 words / 45s
- Captions: word-by-word karaoke, 1–2 highlighted in cyan/yellow, max 3 words on screen, lower-third bold sans

### TopView Seedance bracketed prompt template (single-shot UGC, no character refs)
```
[Duration]: 15s
[Shooting Device]: iPhone 13/14 Pro
[Video Style & Type]: UGC tech ad, 9:16 vertical, handheld, natural lighting
[Hook (First 3s)]: [character description] speaks to camera, [key visual hook]
[Video Content]: [demographic + outfit + behavior]. [Inline @ImageN at the action moment]
[Video Pacing & Atmosphere]: Energetic, authentic, fast-paced talking. [Background spec]
[Dialogue]: [Full dialogue]
```
Use this for single-shot mass-production UGC where each ad is self-contained. Drop char refs, describe in text.

---

## LINKEDIN CINEMATIC SELF-CLONE SOP

For personal-brand cinematic LinkedIn videos in prompted environments. Three passes:

1. **Identity** — Higgsfield Soul ID (one-time train on ~20 photos). Generates photoreal start frames in any prompted scene.
2. **Motion** — Atlas Cloud Seedance 2.0 i2v with start frame + scene refs in `reference_images[]`. NO char refs.
3. **Voice + lip-sync** — ElevenLabs voice clone + Higgsfield InfiniteTalk for lip-sync.

**Pre-flight assets to record:**
- 20–25 photos: 6 front-facing close-ups (varied lighting), 5 three-quarter angle (30–45° L+R), 3 full profile, 5 full-body, 3 expressions (neutral / slight smile / focused), 2 outfits. ≥1024px each. No filters, no sunglasses, no group shots.
- 2–3 min clean voice sample, lavalier or condenser mic, varied tempo, `.wav` or high-bitrate `.mp3`.
- Optional 15s side-angle clip as second motion ref.

Cost per 10s clip: ~$3.80 (Soul ID + Seedance + ElevenLabs + InfiniteTalk).

For talking-head explainer (chest-up, single setting): use **HeyGen Avatar V** instead — $5/min, faster, less fiddling.

---

## ATLAS CLOUD PRICING (verified)
- Seedance 2.0 regular reference-to-video: ~$0.33/s (verified $1.31 for 4s)
- Seedance 2.0 Fast: ~30% cheaper
- NB Pro Edit: $0.14/req
- Full ~24s ad: ~$8 video + $0.40 NB frames = ~$8.50

Atlas's "Base $X/req, Discount Y%" labels on the model card are misleading. Verify per-second math against industry before quoting.

---

## SEEDANCE CHARACTER REF "AI-LOOK" — KNOWN TRADE-OFF + WORKAROUND

WaveSpeed quote: *"Seedance 2.0 is juggling two jobs at once: keeping a recognizable face and delivering motion that feels alive. When it has to pick, it often picks motion."*

**Solution: two-pass workflow** (validated AGS Charys 2026-05-05) — see top of this document.

**Alternatives if Seedance can't be made to work:**
- Higgsfield Soul ID + Soul Jump (architecturally separates identity from motion)
- Kling 3.0 i2v (most natural biomechanics per reviewer comparisons)
- Veo 3.1 — WORSE for this issue (stiff bodies, frozen smiles)

**ByteDance content-policy gate:** Seedance can reject photorealistic faces in start frames with "input image may contain real person." Mitigations: soften/blur face area, use back-3/4 view, partially obscure (sunglasses, hat shadow), or accept as a single-character not-too-photoreal scene.

---

## POST-PRODUCTION PIPELINE

### FFmpeg
- Concat with re-encode for codec safety: `ffmpeg -f concat -safe 0 -i list.txt -c:v libx264 -preset fast -crf 20 -c:a aac out.mp4`
- Strip audio: `-an`
- Trim: `-t Xs`

### Subtitles (word-by-word karaoke)
- Whisper-cli with `-ml 1 -oj` for word-level JSON
- Aggregate tokens → words → bursts of 1-3 words at punctuation/gap boundaries
- Render via PIL with proper vertical centering (use font metrics ascent/descent, not bbox alone)
- Cyan highlight on key words (skip stopwords)
- Compose via `ffmpeg overlay enable='between(t,Ts,Te)'`
- Skip zero-duration bursts (whisper artifacts)
- Clamp output with `-t` to prevent runaway encoding

### Audio rebuild from scratch
- AudioGen / MusicGen on Python 3.12 venv (`/tmp/musicgen_venv`)
- Per-shot ambient stems, ~50 tokens/sec for `max_new_tokens`
- amix multiple stems with `adelay` per stem timecode + `afade` for crossfades
- Music underbed at 18-25% with optional fade-out near end
- Bleeps: `sine=frequency=1000:duration=$BLEEP_DUR` + `adelay` + `volume` mute on original

### Brand handle / end-card
- PIL with serif font (NewYork.ttf or HelveticaNeue Light)
- Subtle drop shadow + crisp white
- Composite via FFmpeg overlay with fade-in
- Position in upper or middle third (not bottom edge)

---

## TOOLING SETUP

### Atlas Cloud MCP
```
claude mcp add -s user atlascloud --env ATLASCLOUD_API_KEY=YOUR_KEY -- npx -y atlascloud-mcp
```
After install, restart Claude Code to load tools.

Direct API endpoints (when MCP unavailable):
- Upload: POST `https://api.atlascloud.ai/api/v1/model/uploadMedia` (multipart)
- Generate video: POST `https://api.atlascloud.ai/api/v1/model/generateVideo` (JSON: `{model, prompt, reference_images, ...}`)
- Generate image: POST `https://api.atlascloud.ai/api/v1/model/generateImage`
- Poll: GET `https://api.atlascloud.ai/api/v1/model/prediction/{id}`

Auth: `Authorization: Bearer apikey-...`

### Higgsfield MCP
Already installed as `higgsfield`. Tools: `media_upload` + `media_confirm` + `generate_image` + `generate_video` + `models_explore` + `job_status` + `transactions` + `balance` + `soul_train`.

### Whisper-cpp
`brew install whisper-cpp`
Model at `~/.whisper-models/ggml-base.en.bin`

### MusicGen / AudioGen
```
python3.12 -m venv /tmp/musicgen_venv
/tmp/musicgen_venv/bin/pip install transformers torch torchaudio scipy
```
Use `transformers` route (avoids `audiocraft` `av` build issue on Python 3.14+).

### Demucs (audio source separation, e.g. extracting clean ambient from a video with dialogue)
```
pipx install demucs
pipx inject demucs torchcodec
demucs --two-stems vocals -o demucs_out input.wav
```
Use `no_vocals.wav` stem as clean ambient bed.

---

## DELIVERY OPTIONS (after files generated)

| Method | Where | Persistence | Notes |
|---|---|---|---|
| iCloud Drive root | `~/Library/Mobile Documents/com~apple~CloudDocs/` | Permanent on iCloud | Slow sync to phone (sometimes hours) |
| Atlas Cloud upload | `https://atlas-img.oss-accelerate-overseas.aliyuncs.com/...` | 24h | Reliable, plays in Safari |
| Gofile.io | `https://gofile.io/d/XXX` | Longer-lived (~1 week) | Page view, fallback |
| Tmpfiles.org | `http://tmpfiles.org/dl/N/file.mp4` | ~60min | HTTP only, can expire fast |
| ShareOnboardingGuide | Anthropic-hosted Claude Code share | Permanent | For shareable Claude Code guides only, not arbitrary files |

For large videos to mobile: Atlas Cloud URL is most reliable.

---

## VERIFIED FAILURE MODES (file as we hit them, learn forever)

| Failure | Cause | Fix |
|---|---|---|
| Sofia + Claire merged into one brunette | Two char ref sets without explicit per-character labeling | `character_left_NAME` / `character_right_NAME` blocks + anti-merge negatives |
| Wedge sole rendered as leather instead of suede | Material mentioned once, no anti-leather negatives | Hammer material 3+ places, negate alternatives |
| Pair of shoes when prompted single | Seedance bias toward pair in product shots | Strong negative + "single hero shoe" repeated |
| Criss-cross X strap instead of single V-strap | Top-view ambiguous reference + "V-shaped" prompt language | Drop ambiguous ref, describe geometry concretely without "V-shape" word |
| Brand handle misspelled (`@ancientgreesandals`) | AI text rendering unreliable | Never render text in video model — burn in FFmpeg post |
| Footsteps audio in aerial shot | Seedance auto-audio bleeds across shots | `generate_audio: false`, build audio in post |
| Door closes by itself | "Door closes" without visible agent | Always specify visible body part causing physical action |
| Character stays outside car when it drives away | Ambiguous "back of car" (trunk vs. seat) | Explicit "ducks head and lowers into rear leather seat" + "drives away with him visible through rear window" |
| 403MB FFmpeg output | Zero-duration `-t 0` burst loops | Skip bursts where end-start < 0.1s, clamp output `-t` |
| Atlas image upload error | PNG > 10MB | Convert to JPG90 with `sips -s format jpeg -s formatOptions 90` |
| Audio doesn't QA itself | Claude reads frames not audio | Always run whisper or ask user to listen |
| ByteDance "input may contain real person" rejection | Photorealistic face in start frame | Soften/blur face, use back-3/4, or generate without prominent face |

---

## EMERGENCY DELIVERY PROTOCOL

If a complex multi-shot fails late:
1. Identify the smallest unit you can salvage (one good multishot + a missing one)
2. Cheap pivot: substitute the failing piece with an existing approved asset
3. Deliver imperfect-but-working version; flag remaining defects honestly to user
4. Schedule the perfect retry for v(N+1) with a corrected approach

Never let pursuit of perfection block delivery on a tight pitch deadline.
