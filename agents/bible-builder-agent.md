# Bible-Builder Agent

## Purpose
Deduce a structured Brand Bible for a new client from their PUBLIC digital footprint. Removes subjectivity bias from human-input briefing.

## Inputs
- Brand URL (mandatory)
- Optional: Instagram / TikTok / YouTube handles
- Optional: known competitors for comparative anchoring

## Process

### 1. Scrape brand-owned assets
- Brand website: homepage + product pages + about/heritage page + journal/press if present
- Instagram: top 12 grid posts + 6 most recent Reels
- TikTok: top 12 videos
- YouTube: most recent 6 uploads + most viewed
- Use Playwright (headless Chromium with full UA + `domcontentloaded` wait) — same pattern as MFB scrape

### 2. Scrape ad-context assets
- Meta Ad Library (`https://www.facebook.com/ads/library`) — search brand name, filter Active. Note: JS-rendered, needs Playwright
- Google search top 10 for "[brand] press" / "[brand] campaign" — recent 6 months only
- Public press coverage (Vogue, Hypebeast, BoF, WWD if luxury fashion; equivalent for category)

### 3. Analyse + cluster
For each artefact category, extract:

**Voice & tone**
- Formality level (casual / refined / corporate / editorial)
- Sentiment (warm / authoritative / aspirational / playful / understated)
- Recurring vocabulary (heritage, bespoke, hand-built, eco, journey…)
- What they NEVER say (inferred from absence — e.g., no discount language = premium-positioned)

**Visual mood**
- Colour palette (3-5 dominant tones across imagery)
- Lighting style (golden hour / studio / natural / editorial)
- Compositional patterns (centred / offset / minimalist / dense)
- Texture / material codes
- Photography vs. illustration vs. CGI mix

**Character archetypes**
- Demographics cast (age ranges, gender, ethnicity skews — based on actual frequency, not aspiration)
- Wardrobe codes (suit / casual / sportswear / streetwear / heritage)
- Attitudes (poised / candid / dynamic / contemplative / smiling-to-camera vs. lifestyle-candid)
- "Solo / pairs / groups" frequency

**Universe / setting codes**
- Locations recurring (urban / rural / international / domestic)
- Eras evoked (timeless / contemporary / nostalgic / futuristic)
- Cultural references (British heritage, Japanese minimalism, Mediterranean luxury, etc.)
- Recurring props / motifs

**Cinematography (if videos exist)**
- Cuts per 30s
- Avg shot duration
- Framing variety distribution (% wide / medium / CU / cutaway)
- Stabilisation style (handheld / gimbal / locked-off)
- Voice usage (VO / direct-to-camera / dialogue / silent)
- Music style (lo-fi / orchestral / pop / no music)

**Inferred no-go list** (from CONSPICUOUS absence)
- E.g.: no children, no pets, no alcohol, no extreme weather, no humour, no neon, no cold blue tones, no kitsch — whatever they consistently avoid

**Ad-format ecosystem**
- What format gaps exist in their current paid creative (the strategic content opportunity)

### 4. Output
A structured markdown bible saved to `/brand-bibles/[client-slug].md` using the template at `/brand-bibles/_template.md`. Include:
- Source URLs cited per claim
- Confidence level per section ("verified / inferred / weak")
- Date built (bibles age — re-verify quarterly)

## Hard rules

- **Cite sources** for every observation — link the URL, screenshot, or post
- **Flag uncertainty** with explicit confidence — don't guess
- **Don't assert what isn't visible** — if no Reels exist, say "video format not yet established"
- **Distinguish brand-owned from client-aspirational** — what they currently do ≠ what they want to be doing
- Bias-check: cluster by frequency, not by your own aesthetic preferences
- For luxury / heritage brands: weight the absence list as heavily as the presence list — restraint defines them

## Output template structure

See `/brand-bibles/_template.md` — sections: Voice, Visual Mood, Character, Universe, Cinematography, No-go list, Ad-format gap analysis.
