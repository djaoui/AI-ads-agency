# Globe-Trotter v5 — Production Package

**Format:** 9:16 / 15s / two-pass / Seedance 2.0 chained multishots
**Brief:** NY → SBH → Caribbean luxury hotel arc, Bible-compliant, v4 defects fixed.

---

## 1. Bible compliance plan

| Bible section | v5 honour |
|---|---|
| Voice — composure over enthusiasm | Zero dialogue. Single en-dash typographic end-card; no urgency, no price, no exclamation. |
| Visual mood — Register A (lifestyle) → Register B (studio-icon) | NY/SBH/Hotel = Register A diffuse daylight, low-contrast mid-tones, no golden hour. End beat = case alone, museum-framed (Register B). |
| Character archetype | Adult woman 30–40, tonal wool coat, pointed leather boots, composed, never smiling, three-quarter rear or profile, mid-stride. |
| Universe | NY limestone façade (Mayfair-equivalent stand-in for Hausmann), private SBH tarmac, Caribbean colonial-hotel suite — all stone/wood/linen, no resort-cliché beach hero. |
| Cinematography | 6 shots / 15s = 12 cuts/30s pace (luxury heritage benchmark). Locked-off + slow gimbal only. No handheld, no shaky-cam, no motion blur. |
| No-go list | Negated explicitly in both prompts: no smoke, no breath vapour, no haze, no neon, no smiling, no crowds, no phones, no logos, no kiss, no children. |

---

## 2. Decision on start frames — REUSE existing approved SFs

**Reasoning:** Bible compliance defects in v4 were 80% **motion-prompt** failures (self-closing door, smoke hallucinated, breath vapour, dust elevation, mirrored window) — NOT start-frame failures. The three approved SFs (SF_NY, SF_SBH, SF3_hotel) already passed ByteDance review; regenerating risks 1–3 rejection cycles on a pitch deadline (Emergency Delivery Protocol §). The composure/no-smile correction can be enforced in the Seedance motion prompt (character is shown rear/profile in MS1 anyway; SBH approach is rear; hotel reveal is over-shoulder). Brightness in NY SF is acceptable: the Bible specifies overcast diffused daylight, and a darker NY street still reads as dawn-overcast not dystopian once smoke + vapour are negated and a soft warm interior light spills from a doorway in motion. **No regeneration.**

If MS1 returns with composure-violating expression, regenerate SF_NY only as a v5.1 patch — not pre-emptively.

---

## 3. MS1 — NY arc (7s, generate_audio:false)

**Reference order:** [SF_NY, product_url_1, product_url_2, product_url_3, product_url_4]
**Model:** seedance-2-0 i2v · `generate_audio: false` · `duration: 7`

```
Image 1 IS the first frame of the video. Render frame 1 to match Image 1 EXACTLY for character identity, framing, lighting, composition. Preserve identity from Image 1 across all subsequent frames. Images 2–5 are the Globe-Trotter Centenary case — black Vulcanised Fibreboard body, brass-gold corner caps, brass-gold lock plate, tan leather straps with brass studs, leather top handle. Match these references exactly for material, hardware, and proportion in every shot the case appears.

[Duration]: 7s, 9:16 vertical, 24fps cinematic
[Style]: Refined editorial luxury, Register A — overcast diffused daylight, low-contrast mid-tone richness, warm Parisian-stone neutrals, oxblood case, charcoal black, soft cobble grey, cool dawn-grey sky. Locked-off and slow gimbal only. NO handheld, NO shaky-cam, NO motion blur on camera.

[Shot 1 — 0.0–2.5s — Side profile, locked-off]: Woman in long charcoal wool coat and pointed leather boots walks left-to-right across frame on a wet limestone Manhattan side street. She carries the black Centenary case in her right hand. Her expression is composed, contemplative, three-quarter profile, gaze level forward — never to camera, never smiling. Diffused dawn light from frame-right; soft warm interior glow from a brass-framed doorway behind her. Negative space above her head 40% of frame.

[Shot 2 — 2.5–4.0s — Front-view jump cut, locked-off, lower angle]: Hard cut to her walking directly toward camera, mid-stride, case in right hand swinging gently. Eyes lowered, lips relaxed, no smile. Background limestone building façade compressed by ~85mm framing. Same diffused light continuity.

[Shot 3 — 4.0–7.0s — Chauffeur door action, slow gimbal in]: Black sedan curbside. A chauffeur in dark suit and grey leather driving gloves stands beside the open rear door, his right gloved hand on the top edge of the door. The woman lowers herself into the rear leather seat; the chauffeur’s gloved hand visibly pushes the door closed in a smooth single arc, his hand stays on the handle for a beat after closure. Camera dollies in 30cm. The case is set on the rear seat beside her by HER OWN gloved hand — visible, deliberate, not floating.

[Material rule]: The case is BLACK Vulcanised Fibreboard with BRASS-GOLD corner caps, BRASS-GOLD lock plate, TAN leather straps, BRASS studs, leather top handle. Match product reference images exactly.

[Negative prompt]: NO street smoke, NO steam vents, NO atmospheric haze, NO breath vapour from mouth or nose, NO fog clouds near face, NO rain, NO snow, NO dust elevation, NO neon, NO fluorescent colour, NO orange-and-teal grade, NO cold-blue tech grade, NO golden hour, NO direct sun flare, NO smiling, NO laughing, NO eye contact with camera, NO crowd, NO pedestrians within 3m, NO phones, NO scrolling, NO visible third-party logos, NO graphic tees, NO sportswear, NO door closing on its own, NO supernatural physics, NO floating objects, NO motion blur from camera, NO shaky-cam, NO handheld jitter, NO text on screen, NO captions, NO end-card.

[Audio]: silent — generate_audio false.
```

---

## 4. MS2 — SBH approach + tarmac arrival + hotel window-reveal (8s, generate_audio:false, @Video1 chain)

**Reference order:** [SF_SBH, MS1_output_video (as `reference_videos[0]` for @Video1 style/grade chain), SF3_hotel, product_url_1, product_url_2, product_url_3, product_url_4]
**Model:** seedance-2-0 i2v · `generate_audio: false` · `duration: 8`

```
Image 1 IS the first frame of shot 1. Image 3 IS the first frame of shot 3. Render frame 1 to match Image 1 EXACTLY and render the cut to Image 3 to match Image 3 EXACTLY for identity, framing, lighting, composition. Preserve identity across all frames. Reference @Video1 (the previous NY segment) for color grade, lighting register, character identity, and refined locked-off camera discipline — match its tonal grade. Images 4–7 are the Globe-Trotter Centenary case — black Vulcanised Fibreboard, brass-gold corners and lock, tan leather straps, brass studs. Match exactly.

[Duration]: 8s, 9:16 vertical, 24fps cinematic
[Style]: Register A continuation — diffused Caribbean overcast daylight, low-saturation, NOT tropical-postcard. Stone/linen/wood palette. Locked-off and slow gimbal only.

[Shot 4 — 0.0–2.5s — SBH approach, three-quarter rear, slow gimbal alongside]: Same woman, now in lighter ecru wool coat (continuity of silhouette), walks across pristine private tarmac toward a small private jet, three-quarter rear framing, case in right hand. Two ground crew in pressed uniform stand at attention beside the air-stair, one of them extending a white-gloved hand toward the rail. NO dust on tarmac, NO heat haze, NO engine smoke, NO atmospheric particulate. Tarmac is immaculate, wet-clean reflective. Composed posture, no smile, no glance back.

[Shot 5 — 2.5–4.5s — Tarmac arrival hand-off, locked-off MCU on case]: Cut to MCU on the woman's gloved hand and the ground crewman's white-gloved hand together placing the Centenary case onto a polished brass luggage trolley. Both hands visible, both grip the case top handle and leather strap. The hands lower the case in one smooth motion. Brass-gold hardware catches a single soft highlight. NO floating case, NO self-placing.

[Shot 6 — 4.5–8.0s — Hotel window reveal, slow dolly in]: Cut to Image 3 — Caribbean colonial hotel suite interior, linen drapes, dark wood floor, single tall French window with TWO panes. The woman stands in three-quarter rear, both hands raised, her LEFT hand grips the left pane handle and her RIGHT hand grips the right pane handle simultaneously; both panes swing open OUTWARD in unison as she pushes. View beyond reveals diffused overcast Caribbean horizon — soft grey sky, calm sea, single distant sailboat silhouette. The Centenary case rests on a dark wood luggage stand at frame-right, lock plate facing camera. Camera dollies forward 40cm toward her shoulder. She does not turn. No smile. Composed.

[Material rule]: Case = BLACK Vulcanised Fibreboard, BRASS-GOLD corners + lock plate, TAN leather straps with BRASS studs, leather top handle. Identical to MS1 case. Match references exactly.

[Negative prompt]: NO street smoke, NO atmospheric haze, NO breath vapour, NO heat haze, NO tarmac dust elevation, NO engine exhaust visible, NO golden hour, NO orange-teal grade, NO neon, NO tropical-postcard saturation, NO palm-tree-cliché framing, NO beach hero, NO hotel pool, NO crowd, NO smiling, NO eye contact with camera, NO laughing, NO single window pane mirroring itself, NO ghost hand, NO self-opening windows, NO self-closing doors, NO floating objects, NO supernatural physics, NO phones, NO logos, NO text on screen, NO captions, NO motion blur from camera, NO handheld jitter.

[Audio]: silent — generate_audio false.
```

---

## 5. Post-prod plan

**Audio architecture (built from scratch, MusicGen / AudioGen, 15s timeline):**

| Stem | Source | Window | Level |
|---|---|---|---|
| Music underbed — refined orchestral solo piano, sparse, single-note motifs, no swells, RP-restraint | MusicGen prompt: *"Sparse solo grand piano, single notes, slow legato, restrained refined cinematic, no strings swell, no percussion, library wood-panel acoustic, 70 BPM, A minor"* | 0.0–15.0s | -18 LUFS, fade-in 0.5s, fade-out 1.0s |
| NY ambient — distant city wash, soft wet-tyre rumble, no horns, no sirens | AudioGen *"distant overcast city street, soft wet tyres, no traffic horns, no sirens, no voices"* | 0.0–7.0s | -28 LUFS |
| Tarmac ambient — distant low jet idle, clean wind, no engines spooling | AudioGen *"distant private jet idle, soft steady wind on open tarmac, no spooling engines"* | 7.0–11.5s | -30 LUFS, crossfade 0.4s with NY |
| Hotel suite ambient — interior linen-room tone, faint distant sea | AudioGen *"empty colonial hotel suite interior tone, very distant calm sea through open window, no birds chatter"* | 11.5–15.0s | -28 LUFS |
| Foley — door-thunk MS1 shot 3 (sine 80Hz + library thump), case-set MS2 shot 5 (soft brass clink) | FFmpeg + library | t=6.7s, t=4.0s | -20 LUFS spot |

**Music brand alignment (Bible Cinematography prediction):** *"orchestral / piano / single-cello score"* → Single piano chosen. NO folksy lo-fi, NO acoustic guitar, NO beats. If MusicGen output trends folksy, reprompt with `"chamber piano, sparse, ECM-label discipline, NOT folk, NOT lo-fi, NOT acoustic guitar"`.

**End-card overlay (FFmpeg overlay, t=13.0–15.0s, fade-in 0.4s):**
- Background: dim final frame to 35% via `eq=brightness=-0.35`
- Logotype rendered in PIL: `GLOBE-TROTTER` — uppercase, **explicit en-dash hyphen U+2010**, letter-spacing +120, font: Bodoni 72 Bold or Didot Bold (heavier than v4), color #F2EAD8 cream, drop shadow 2px @ 35% black
- Subline: `Since 1897` — small caps, letter-spacing +200
- Position: vertical center, both lines stacked, symmetrical margins
- Render at 2x then downscale for crisp edges

**FFmpeg composite outline:**
```bash
# 1. Concat MS1 + MS2 silent
ffmpeg -f concat -safe 0 -i list.txt -c:v libx264 -preset slow -crf 18 -an v5_silent.mp4
# 2. Build audio stem mix (5 stems, amix with adelay + afade per spec above) → v5_audio.wav
# 3. Mux audio
ffmpeg -i v5_silent.mp4 -i v5_audio.wav -c:v copy -c:a aac -b:a 192k -shortest v5_av.mp4
# 4. Overlay end-card PNG with alpha + brightness ramp
ffmpeg -i v5_av.mp4 -i endcard.png -filter_complex \
  "[0:v]eq=brightness='if(gte(t,13),-0.35*((t-13)/0.4),0)'[bg];[bg][1:v]overlay=0:0:enable='between(t,13,15)':alpha='if(gte(t,13),min(1,(t-13)/0.4),0)'" \
  -c:a copy v5_FINAL.mp4
```

---

## 6. Mechanical QA brief

QA agent compares finished v5 against this shot intent:

| t | Shot | Must-have | Must-NOT-have |
|---|---|---|---|
| 0.0–2.5 | NY side profile L→R | Composed expression, profile, case in R hand, diffused daylight | Smoke, breath vapour, smile, eye-contact, neon |
| 2.5–4.0 | NY front-view jump cut | Hard cut, lower angle, eyes lowered, no smile | Same negatives |
| 4.0–7.0 | Sedan door-close | Visible chauffeur gloved hand on door through entire close, hand stays on handle 0.5s after | Self-closing door, floating case, breath vapour |
| 7.0–9.5 | SBH tarmac approach | Immaculate tarmac, ecru coat, three-quarter rear, ground crew at attention | Dust elevation, heat haze, engine smoke, smile |
| 9.5–11.5 | Tarmac case hand-off | Two visible hands on case (hers + crewman white glove), brass hardware catch-light | Floating case, self-placing |
| 11.5–15.0 | Hotel window reveal | TWO hands gripping TWO pane handles simultaneously, both panes swing in unison, case on stand frame-right matches packshot | One pane mirroring, ghost hand, smile, palm-tree cliché |
| 13.0–15.0 | End-card | `GLOBE-TROTTER` with visible en-dash, Bodoni/Didot Bold, cream on dimmed bg, `Since 1897` subline | Hyphen missing, hairline weight, "Globe Trotter" two-word render |

QA must explicitly verify: case material (black fibreboard + brass-gold + tan leather + brass studs) consistent across all 6 shots; no audio bleed; no in-model text rendered; chauffeur and ground-crew agents visible for every physical action.

---

*Word count: ~1180.*
