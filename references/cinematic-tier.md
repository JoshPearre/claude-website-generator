# Tier C — Cinematic Story Experience (the full playbook)

SKILL.md's "Tier C" section defines when this mode runs and the step order; this file holds the
working detail: the reference exemplars, the discovery questions, the vibe-direction and
master-prompt templates, the mockup workflow, the credit-estimate gate, segment chaining, the
chapter-loop implementation, and the UI-overlay sampling rules.

**The one-line thesis:** take the context of a business, make a stylized cinematic film for it,
and build the website *around* that film — scroll advances the story, and each facet of the
business is a chapter. Story-first, never "random parallax video".

---

## Reference exemplars (study these before directing)

| Site | What to steal | Role |
|---|---|---|
| **harbor-dev-v3.vercel.app** | Dark aerial 3D container-port world, neon-green data racks in fog, stat bar + bento cards floating over the moving scene. **This is the CODE reference for scroll↔video binding** — when the scrub or crossfade feels choppy, fetch this site and study how it drives the animation from scroll before writing another line. (Origin: in the source video, one "look at how Harbor dev does it" pass fixed a choppy scroll in one step.) | Code + look |
| **stewardengine.com** | Miniature-diorama storytelling: model country house, clouds part, windows light up as "demand comes home", golden streams skimmed at the gate by OTAs. Editorial serif headlines riding the film. The gold standard for *story logic* — every camera beat argues the product's case. | Story + look |
| **headlandmarketing.com** | Stormy night ocean as a metaphor ("Seven disciplines, one standard") with each service discipline surfacing as a chapter over the same sea. The multifaceted-business pattern: one world, N chapters. | Multifacet mapping |
| **incomestreamsurfer.com** | The source video's own result: near-black ocean, luminous grid under the surface, blue→violet→magenta streams converging; each original page section became a camera stop. | End-to-end example |

---

## C1 — Story intake (discovery questions)

Given the business details (or an existing site), establish the story before any generation.
Ask (AskUserQuestion, one round) whatever the brief doesn't already answer:

1. **What is the film for?** — a website scroll experience (this tier), or a video that mimics
   a scroll (a deliverable clip, not a site)? Confirm it's the former.
2. **Mood** — let the vibe options (C2) carry this; only ask directly if the brief is silent.
3. **How many scroll chapters?** — count the facets of the business. Read their existing
   site/brief and propose the mapping (e.g. 7 sections → 7 camera stops). Multifaceted
   businesses get one chapter per facet; single-service businesses get a 3-act arc
   (problem → transformation → payoff).
4. **A figure/mascot in the world, or purely environmental?** — environmental is the safe
   default; a recurring object/mascot needs reference-image discipline to stay consistent.

Then write the **story map**: `chapter n → business facet → what the camera sees → what it
argues`. Steward's "windows light up = bookings arriving" is the bar: every beat must *mean*
something about the business, or it's a screensaver, not a story.

## C2 — Vibe selection (required, before any generation)

Draft **2–3 named vibe directions** and present them via AskUserQuestion — the user is the
director; never pick silently. Each option must include:

- **Name + one-line story logic** — why this world tells *their* story
- **Palette anchor** — 2–3 hex codes that will be written verbatim into every prompt
- **The master-prompt opening line** — the actual first sentence of the C5 video prompt, so
  the user is choosing between real prompts, not adjectives

Worked vibe templates (adapt, don't copy blindly — the niche brief's imagery direction and
anti-references still apply):

```
1. "Dark Voyage" — premium tech / finance / SaaS
   Story logic: the business is the light and signal in a vast dark market.
   Palette anchor: near-black navy base, electric blue #3B82F6 → violet #8B5CF6 accents.
   Opening line: "A single continuous cinematic aerial drone shot, one unbroken camera move
   with no cuts, flying slowly forward over a vast dark ocean at night."

2. "Miniature World" — hospitality / real estate / local service / anything with a place
   Story logic: the business's world as a hand-built diorama that comes alive as demand arrives.
   Palette anchor: warm cream base #F5EFE4, brass/gold accent #B08D4C, deep green support #2F4A3E.
   Opening line: "A single continuous macro camera move, no cuts, gliding slowly across a
   handcrafted miniature diorama of [the business's world], tilt-shift depth of field,
   soft studio light."

3. "Golden Hour Craft" — trades / food / fitness / human-craft businesses
   Story logic: real work, elevated — the craft shot like a title sequence.
   Palette anchor: warm amber #E8A33D, charcoal shadow #1C1B19, one saturated brand accent (e.g. #C4402F).
   Opening line: "A single continuous cinematic dolly shot, no cuts, moving slowly through
   [the workspace] at golden hour, volumetric light through haze."
```

**Palette precedence (never stack two palettes — same rule as Step 3.5):** if the user supplied
a brand or reference URL (Step 3.5 rungs 1–2), the vibe's palette anchor is **derived from those
brand tokens** — pick 2–3 of the brand's own hexes for the film; the brand still wins the
token-lock. Only when no brand/reference exists does a vibe introduce its own anchor, which then
binds as the design direction's accent path in Step 3.5 so film and UI never disagree.

## C3 — Draft mockups (required questions, stills before any video)

Mockups are **Higgsfield-generated still images of the site design** (not coded drafts) —
cheap stills that de-risk expensive video. Ask, in one AskUserQuestion round:

1. **"How many draft mockups — 1, 3, or 5?"** (3 is the recommended default; 3+ mockups fall
   under the multi-site differentiation spirit: each mockup varies layout, type, and overlay
   treatment, all inside the chosen vibe.)
2. **"How should the image set work?"** — with the **estimated Higgsfield total for each
   option computed via `higgsfield_check_cost` and shown in the option text**:
   - **One shared set** — one set of stills mocking up the different UI components (hero,
     chapter card, nav, CTA) reused across all mockups. Cheapest. (~`mockups + 3–4` stills)
   - **A fresh set per mockup** — every mockup gets its own component stills. Highest
     fidelity, highest cost. (~`mockups × 4–5` stills) — state the total across ALL mockups.
   - **Hero-only** — one hero still per mockup, no component set. This is the
     **hero-cinematic (C-lite) path**: only the hero gets the film; the rest of the site is a
     standard Tier 4–5 build.

**The user's option selection IS the still-spend authorization** — it authorizes exactly the
stills enumerated and priced in the chosen option, nothing more. A redirect or "one more
mockup" is a new plan: re-price it and re-ask before generating. There is no other pre-C4
spend path in this tier.

**These mockups are NOT the multi-site mandate.** Tier C draft mockups are Higgsfield concept
*stills* inside the one chosen vibe — asking for 3 or 5 of them does not trigger the multi-site
total-differentiation mandate (that governs 3+ full *site builds*). Mockups still vary layout,
type, and overlay treatment, but deliberately share the vibe's world and palette.

Generate the mockups with the routing table's still model (**GPT Image 2** via Higgsfield —
it's the design/text-capable one), on the vibe's palette anchor, at the hero's aspect ratio.
**If Higgsfield is absent or unauthenticated, Tier C cannot run** (its video pipeline requires
it): say so plainly and offer either a standard Tier 4–5 build, or concept stills via the
fallback still models (`imagegen-frontend-web` / Gemini per the routing table) while the user
connects it — never fail silently and never substitute an unpriced generator for the paid path.
Present the mockups; the user picks the direction (or redirects). **The chosen mockup becomes
the visual contract for C5–C6** — the hero keyframes and the coded build must match it.

## C4 — Clip plan + the estimate-and-confirm gate (hard requirement)

Map the story to concrete generations, then price everything **before generating any video**:

| Piece | Count | Model (first choice) |
|---|---|---|
| Hero master shot | ceil(hero_seconds / model_cap) segments — e.g. 30s on a 15s-cap model = 2 | Seedance (best) / Kling FLF (cheaper) |
| Chapter loops | 1 per facet chapter (5–10s each) | Seedance / Veo (ambient loops) |
| Keyframe stills | **from the chaining strategy below — never a fixed number** | GPT Image 2 / Nano Banana Pro (reference mode) |
| Upscale (optional) | 1 per final asset, only if planned here | connector's upscale op |

**Count keyframes from the chosen chaining strategy:**

- **Continuation chaining (default — image-to-video with a start image, e.g. Seedance):**
  generate **1 hero start keyframe + 1 keyframe per chapter**. Hero segment boundaries are
  free — each next segment is seeded with the previous segment's ffmpeg-extracted last frame.
- **FLF chaining (Kling FLF or any first-last-frame model):** every segment needs a first AND
  a last frame — generate the hero's **`segments + 1` shared boundary stills**, and for
  seamless chapter loops pass the **same still as both first and last frame**.

If an **upscale** is wanted (native output below ~1080p), it must be a priced row here; if the
connector exposes no upscale operation, drop the row — never improvise an unpriced upscale in
C5. Call `higgsfield_check_cost` for **every planned generation**, then print the estimate
table (the numbers below are placeholders — compute the real rows from the strategy):

```
Cinematic clip plan — estimated Higgsfield spend
  Hero master shot   2 segments × 15s (Seedance)   ~N credits
  Chapter loops      4 × 8s (Seedance)             ~N credits
  Keyframe stills    5 (GPT Image 2)               ~N credits
  Upscale            (only if planned)             ~N credits
  ─────────────────────────────────────────────────
  Total                                            ~N credits
Reply "generate" to spend this, "trim" to cut chapters/segments, or "hero only" to drop to C-lite.
```

**Show credits only.** Add a ≈$ line only when the user's actual credit-pack pricing is known —
never derive dollars from anecdotes (the "200 credits ≈ $13" data point from the source video
is volatile context, not a conversion rate; `check_cost` is the sole source of truth).

**Wait for the explicit go. No video generation happens before it — ever.** If the user trims,
re-price and re-confirm. Re-run the gate if the plan changes mid-generation (a failed segment
that needs a paid retry counts as a plan change).

## C5 — Generation

**Master shot prompt skeleton** (the timeline-of-beats is the trick — one beat per chapter):

```
A single continuous cinematic [aerial drone / macro / dolly] shot, one unbroken camera move
with no cuts, [vibe opening — world + motion direction]. Stylized premium 3D render aesthetic
like a high-end tech website background — NOT photorealistic documentary. [World description
with the vibe palette embedded as hex: e.g. "near-black deep navy water with a faint luminous
grid beneath the surface, electric blue #3B82F6 to violet #8B5CF6 to magenta #D946EF glow
accents".]

Timeline: 0–5s [chapter 1: what the camera sees — the hero beat]. 5–9s [chapter 2 beat].
9–13s [chapter 3 beat]. … ending on a calm, wide, symmetrical composition [the CTA resting
shot].

Constant smooth forward camera motion throughout, slow and weighty, no camera shake, no cuts,
no text, no logos, no people, [no boats / no cars / scene-appropriate negatives]. Consistent
color grade: deep blacks, [palette anchor hexes], subtle film grain, soft volumetric fog,
high contrast, cinematic depth of field.
```

Rules:
- **Brand hexes go inside the video prompt verbatim** — that's how the film lands on-palette.
- **Each chapter beat gets ~4–5 seconds** and must show the thing its overlay will talk about.
- Decline any "enhance prompt" option; keep motion simple — wild camera moves smear on scrub.

**Segment chaining (when the hero shot exceeds the model's per-generation cap, e.g. 15s):**

1. Generate segment 1 from the full master prompt (start keyframe as image input).
2. Download it; extract the **true last frame** —
   `ffmpeg -sseof -3 -i seg1.mp4 -update 1 -q:v 1 seg1_last.png`
   (decodes the final ~3s, overwriting the file each frame, so what remains is the last one) —
   and QC it (no smear, on-palette).
3. Generate segment 2 with the last frame as the start image and the prompt prefixed:
   **"Continue this exact scene: [same master prompt, next timeline beats]."**
4. Repeat per segment; concat with a list file and stream copy:
   `printf "file 'seg1.mp4'\nfile 'seg2.mp4'\n" > list.txt`
   `ffmpeg -f concat -safe 0 -i list.txt -c copy public/scroll-source.mp4`
   Stream copy is lossless and works here because all segments come from the same model and
   settings (identical codec/resolution/fps); if a mismatch sneaks in, re-encode once instead.
   Upscale only if it was a **priced, confirmed row in the C4 plan** — never as an
   afterthought.

**Chapter loops:** one short clip per facet chapter, prompted for a seamless loop — same world,
same grade, camera nearly static: "…subtle ambient motion only, [fog drifting / water moving /
lights flickering], composition holds, designed to loop seamlessly, first and last frames
match." Generate each with the chapter's keyframe as image input so all chapters share the
world. **A prompt alone can't guarantee matching loop endpoints** — prefer an FLF model with
the *same keyframe as first and last frame*; on i2v-only models, fix any visible pop in post by
crossfading the tail into the head (~0.5s, ffmpeg `xfade`) before encoding. Encode muted loops
targeting ≤ ~3MB each — H.264 `-an -crf 28 -preset slow -movflags +faststart` (or WebM
`-c:v libvpx-vp9 -crf 40 -b:v 0 -an`) at 1280–1600w, then **check the actual file size** and
raise CRF / step the width down (1600→1280→960) until it fits. Poster = the chapter keyframe.

## C6 — Build (scrub hero + crossfading chapter loops + sampled UI overlays)

**Hero:** the existing flip-book pipeline — extract WebP frames and scrub them on a `<canvas>`
per `references/scroll-animation-best-practices.md` (never scrub `<video>.currentTime`) —
with one Tier C adjustment: **scale the extraction to the master's real length.** That guide's
example command caps at `-t 6`; here, set `-t` to the full hero duration and pick
`fps ≈ 200 / duration_seconds` so the whole film lands inside the 200-frame preload cap (a 30s
master → ~6–7 fps extraction). Scrub smoothness is frames-per-scroll-distance, not source fps —
200 preloaded frames across the hero runway stays smooth. For masters beyond ~30–40s, split the
scrub into per-chapter segments or trim the cut; never blow the 200-frame budget. The hero
runway is `~100vh × (segments + 1)` tall; overlay copy beats tied to scroll ranges.

**Chapters:** below the hero, one full-viewport section per facet:

- Each chapter's loop is a full-viewport
  `<video autoplay muted loop playsinline preload="metadata">` behind the content,
  `object-fit: cover`, marked decorative: `aria-hidden="true"` + `pointer-events: none`.
- **Layering — pick ONE model, don't mix:** default to a `position: sticky` full-viewport
  wrapper per chapter (simplest correct stacking). `position: fixed` layers also work, but
  then every chapter must manage z-index and explicit mount/unmount — only choose it when the
  design needs cross-chapter persistence.
- **Crossfade on scroll, not on timers.** GSAP ScrollTrigger with `scrub` is the Tier C
  default driver (Framer `useScroll` + `useTransform` is acceptable only on the C-lite path
  where GSAP isn't already in the build): entering a chapter fades its world in over ~0.4 of
  the transition zone while the previous fades out.
- **Playback control both directions:** `play()` a chapter's video when its section enters the
  crossfade zone; `pause()` it once fully faded out (decode cost is real). At most **two videos
  decoding during a crossfade, one otherwise**.
- Wire **Lenis** with the full GSAP ticker lifecycle — the one-liner alone is not enough:
  ```js
  const lenis = new Lenis()
  lenis.on('scroll', ScrollTrigger.update)
  gsap.ticker.add((t) => lenis.raf(t * 1000))
  gsap.ticker.lagSmoothing(0)
  ```
- **If the result feels choppy, study harbor-dev-v3.vercel.app before adding code** — match
  its scroll-binding approach (it is the known-good live implementation of this pattern). If
  the site is down or has changed, this document and `scroll-animation-best-practices.md`
  remain the canonical contract — the URL is a comparison aid, never a dependency.

**UI overlays — the random component sampling:** the chapters' foreground content is sampled
from this skill's own library, exactly like a normal build:

- Run the **Step 3 sampling** at the Tier 4–5 band: pick 4–6 categories relevant to the
  chapters (`cards-and-grids`, `interactive-elements`, `animations`, `navigation`,
  `forms-and-inputs`, `scroll-effects` are the usual set), randomly select 1–2 prompts per
  category, and keep the **3-source variety rule**. Registries and the Magic MCP participate
  as normal.
- **Each chapter gets at least one interactive component** (a stat strip, bento card, marquee,
  form, count-up — whatever its facet needs), choreographed to the chapter's scroll range so
  components arrive *as the background moves* — entrances tied to scroll progress, not timers.
- Overlay treatment: glassmorphic/scrimmed panels so text passes contrast on a moving
  background (`backdrop-filter` + a 30–50% scrim in the vibe's base color). **Verify contrast
  against the brightest frame of each loop**, not the average.
- The restraint guardrail bends but doesn't break here: the film IS the one heavy set-piece
  (hero scrub + chapter loops are a single continuous world, not N competing effects). Do
  **not** add particle/shader/animated backgrounds on top of it; overlays stay content-serving.

**Fallbacks & a11y (all required):**
- `prefers-reduced-motion` → static keyframe posters everywhere (no scrub, no loops), **and**
  the sampled overlay motion degrades with it: scroll-choreographed entrances become
  opacity-only or none (content must never stay hidden behind an un-fired animation), and
  marquees/count-ups/auto-playing micro-motion stop.
- Mobile / Save-Data → **decide the asset tier before assigning `src` or preloading** (never
  download desktop encodes and then discard them): smaller loop encodes (≤720w) or the poster
  stills; the scrub hero already has its static-start-frame fallback.
- A chapter whose loop failed to generate ships its keyframe still as a static background —
  the layout never breaks (same never-hard-fail contract as the Step 5 image chain).

## C7 — Quality + ship

Nothing new: Quality Trio (Step 3.5) binds the vibe's palette anchor as the design direction's
accent path; Step 6 polish pass runs in full (`emil-design-eng` is mandatory here — motion IS
the product); Step 7 deploys and captures demo assets. The run report adds two lines:

```
Vibe:        <chosen vibe name> (of N offered)
Film:        <hero segments × length + chapter loop count> · <credits actually spent> credits
```

## Hard guards (repeated from SKILL.md because they're load-bearing)

- **Explicit opt-in only.** Tier C never comes from the Step 2.5 roll, the tier-niche map, or
  a multi-site tier randomization. "Make it cinematic" alone upgrades `higgs_hero`; only
  "cinematic experience / cinematic website / story site / film website / Tier C" (or an
  explicit user yes to a Tier C offer) enters this tier.
- **Never in Demo Mode or batch runs — and Demo Mode wins collisions deterministically.**
  "Demo mode, make me a cinematic website" (or a `lead.json` + cinematic language) stays Demo
  Mode: decline Tier C in one line and offer it as a separate run for a business the user names.
  An explicit scroll-hero override inside Demo Mode uses the standard `higgs_hero` pipeline,
  never Tier C. No exceptions, not even for a high-value lead.
- **Every credit has a confirmation attached.** Stills are authorized by the C3 option
  selection (each option is priced before it is offered); video — and any upscale — only after
  the C4 gate. There is no unpriced, unconfirmed spend path anywhere in this tier.
- **The C4 estimate-and-confirm gate is unskippable**, including on "just build it" runs — that
  phrase waives design checkpoints, never spend.
