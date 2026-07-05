# Experience Blueprints — named, interchangeable page experiences (Tier 2–5)

Whole-page interaction archetypes, distilled from award-tier reference sites (mont-fort.com,
floema.com, oryzo.ai, GQ "The Extraordinary Lab") and a survey of recent Awwwards SOTD/SOTM
winners. A **blueprint** is bigger than a component and smaller than a site: it's the signature
experience the site is built around. Step 2.5 of SKILL.md rolls one per site — randomly among
the tier-allowed options — so variety comes from the *roll*, and craft comes from following the
implementation contract here.

**The award-winner rule that governs everything below:** foundation beats effects. Every
SOTD/SOTM site nails typography, imagery, spacing, and easing *first*; the signature experience
sits on top. A blueprint on a weak foundation reads as a gimmick. Lock the Quality Trio tokens
before any blueprint work.

---

## Foundation layer (every Tier 2+ site, before any blueprint)

These are the craft signals that separate award sites from nice sites — cheap, and non-negotiable:

1. **Choreographed entrance** — the hero is never static-on-load: stagger the headline mask-reveal,
   image unveil, and nav fade as one composed sequence (300–900ms total, overlapping — not serial).
2. **Smooth scroll** — Lenis (`lenis` on npm): `new Lenis({ lerp: 0.1 })` + RAF loop. Makes every
   scroll effect feel 2× more expensive. Tier 3+ default; skip on Tier 1–2 utility sites.
   When GSAP ScrollTrigger is also used, wire them: `lenis.on('scroll', ScrollTrigger.update)`.
3. **Easing quality** — never `linear`, never default `ease`. UI: `cubic-bezier(0.33, 0.66, 0.66, 1)`
   or spring physics. Entrances: `ease-out` family. Durations 150–400ms for UI, longer only for
   hero set-pieces.
4. **Designed hover/press states on everything interactive** — links, cards, buttons, images.
   A magnetic or underline-draw hover on nav links alone lifts perceived craft.
5. **Progressive disclosure** — reveal sections as they enter (IntersectionObserver / `whileInView`),
   one focal point per viewport. 7 of 8 surveyed winners do scroll reveals; none dump everything above
   the fold.
6. **Reduced motion** — every blueprint below MUST have a `prefers-reduced-motion` fallback
   (static composition, opacity-only reveals). Non-negotiable.

**Anti-patterns (observed on losers, absent on winners):** scroll-hijacking that fights native
scroll; autoplaying sound; parallax on mobile (jank + battery); multiple stacked animated
backgrounds; YouTube iframes as heroes; effects that drop text contrast.

---

## The blueprint catalog

Ten blueprints. Each entry: what it is → how it's built → tiers → difficulty / perf risk →
fallback. Stack shorthand: **FM** = Framer Motion, **GSAP** = gsap + ScrollTrigger (free, npm
`gsap`), **R3F** = @react-three/fiber + @react-three/drei, **Lenis** = smooth scroll.

### B1 — Persistent 3D World  `[Tier 4–5 · L · perf: medium-high]`
*(mont-fort.com — the mountain that lives behind everything; playful register: threejs.paris and poch.studio — oversized 3D characters/objects the pointer plays with)*
One WebGL scene mounted OUTSIDE the router/page content (fixed, z-0); content scrolls over it.
Navigation (tabs, routes, section anchors) doesn't swap the scene — it **tweens the camera** to a
new position/target (GSAP to camera.position + lookAt, 1–1.6s, expo.out). Pointer move adds
±2–3° parallax on camera yaw/pitch, lerp-damped (~0.05–0.1 factor) so it trails the mouse.
Scroll within a page maps to a secondary camera dolly or fog/lighting shift.
- Build: R3F `<Canvas>` in a fixed full-viewport div; scene = low-poly environment, product, or
  abstract geometry in the locked palette; `useFrame` lerp for pointer parallax; route/tab state
  drives a GSAP camera timeline. Pause rendering when tab hidden (`document.visibilitychange`)
  and stop the RAF when the canvas is fully covered.
- Fallback: static rendered still of the scene as a fixed background image.

### B2 — Camera Fly-Through  `[Tier 4–5 · L · perf: medium-high]`
*(the "3D walk through the website")*
Scroll progress drives a camera along a curve THROUGH a scene — rooms, product parts, floating
gallery frames — with overlay info cards fading at defined progress ranges. The visitor "walks"
the site by scrolling.
- Build: R3F + drei `ScrollControls pages={N}` + `useScroll`, camera on a `CatmullRomCurve3`
  (`curve.getPointAt(offset)` + lookAt next point), or GSAP ScrollTrigger scrubbing a camera
  timeline inside a pinned section. Overlay cards: HTML absolutely positioned, opacity keyed to
  scroll ranges (e.g. `{start:0.15, end:0.3}`), never timers.
- Keep the path simple (3–5 waypoints), motion slow, and add fog — it hides pop-in and reads
  cinematic. DRACO-compress any GLTF; target < 2MB total scene payload.
- Fallback: the scene's keyframe stills as a pinned image sequence (B6 downgrade).

### B3 — Exploded Assembly  `[Tier 4–5 · L · perf: high]`
*(GQ watch teardown; ylem.watch — the luxury register: a monochrome watch mechanism that disassembles as you scroll; the user's "exploding objects that reveal information")*
A product/object separates into its parts as the visitor scrolls — each separation phase pauses
at an info card explaining that part — then optionally reassembles at the end. The single best
"show, don't tell" pattern for products and processes.
- Build (real 3D): R3F, model with named parts; each part's position lerps from assembled to
  exploded along its axis/normal as pinned-scroll progress advances; stagger parts so they peel
  in sequence; info cards bound to progress ranges. Static baked lighting (no per-frame shadow
  updates).
- Build (generated video): the existing scroll-scrub pipeline (SKILL.md "3D scroll animation")
  IS this blueprint's cheaper implementation — start frame assembled, end frame exploded/X-ray,
  Higgsfield interpolates, flip-book canvas scrubs it. Prefer this when no 3D model exists.
- Fallback: exploded-view still + sequential card reveals.

### B4 — Immersive Reveal  `[Tier 3–5 · S–L scalable · perf: low→high]`
*(floema.com — the site that unveils itself)*
The page treats every appearance as an event: preloader counts in → hero image unmasks (clip-path
or shader wipe) → headline reveals line-by-line from behind masks → each subsequent image/section
unveils as it enters. At the top tier, images are WebGL planes whose shader distorts with scroll
velocity (liquid warp on fast scroll, settling when still).
- Tier 3 build (CSS/FM only): `clip-path: inset()` transitions on images; headline split into
  lines inside `overflow:hidden` wrappers, translateY 100%→0 staggered; Lenis; parallax depth via
  two scrub speeds (bg 0.5×, fg 1.1×).
- Tier 5 build: + curtains.js or R3F image planes, UV-offset fragment shader driven by scroll
  velocity; velocity also nudges letter-spacing or skew for the "alive" feel. Aggressive register:
  RGB-split / displacement-map glitch on images and type (the music/agency look — e.g. the HXD
  artist-roster site) — reserve for youth/culture/creative niches; it actively fights trust niches.
- Fallback: opacity/transform reveals only.

### B5 — 3D Slider / Gallery Tunnel  `[Tier 3–5 · M · perf: medium]`
*(oryzo.ai product gallery; infinite WebGL carousels)*
A draggable/scrollable gallery with real depth: side items recede and tilt (coverflow), or plane
images bend with drag velocity, or the user scrolls INTO a tunnel of frames.
- Tier 3 build: CSS 3D coverflow — `perspective` on the track, per-item `rotateY/translateZ` from
  distance-to-center, FM `drag="x"` with spring snap; wheel/drag both work.
- Tier 4–5 build: WebGL plane carousel (R3F): images as planes on a curve, drag velocity feeds a
  vertex-shader bend; or drei `ScrollControls` tunnel with frames at increasing -Z.
- Product-viewer variant: scroll-linked rotation of a single model/image-sequence (turntable).
- Fallback: flat swipe carousel with scale/opacity emphasis.

### B6 — Pinned-Scene Scrollytelling  `[Tier 4–5 · M · perf: medium]`
*(GQ Extraordinary Lab; every great editorial feature)*
The page is chapters: each section pins for 200–400vh while its media transforms — image sequences
advance, diagrams draw, copy swaps — then releases to the next chapter. Vignette/blur transitions
between chapters. Audio narration is possible but strictly opt-in (never autoplay).
- Build: GSAP ScrollTrigger `pin: true, scrub: 0.8–1.2` timelines per chapter (or CSS sticky +
  FM `useScroll` for lighter cases); media = crossfading images, canvas frame sequence, or SVG
  line-draw; chapter transitions via clip-path/backdrop-filter.
- Fallback: unpinned sections with standard reveals.

### B7 — Horizontal Drift  `[Tier 3–4 · S–M · perf: low-medium]`
A horizontal gallery/timeline INSIDE the vertical page: section pins, vertical scroll translates
the track sideways through panels (work items, menu, process steps), then releases.
- Build: sticky wrapper + track `translateX` = f(pinned progress) (GSAP ScrollTrigger or FM
  `useTransform`); panel count × 100vw runway; progress dots. Disable on mobile — stack panels
  vertically instead.
- Fallback: native `overflow-x` scroll-snap strip.

### B8 — Sticky-Stack Cards  `[Tier 3–4 · S · perf: low]`
Sections/cards stack onto each other as you scroll — each new card slides over the previous,
which scales down (~0.95) and dims slightly, building a deck. Great for services, features,
testimonials, menus.
- Build: each card `position: sticky; top: 0` in a tall container; previous card's
  scale/brightness keyed to next card's entry progress (FM `useScroll` per card). Pure CSS + FM;
  no GSAP needed.
- Fallback: plain stacked sections.

### B9 — Kinetic Type Hero  `[Tier 3–5 · S–M · perf: low-medium]`
*(the most frequent Awwwards signature: type IS the visual)*
Oversized display type (12–20vw) as the hero's only subject: lines mask-reveal on load, words
rotate/swap, a marquee strip drifts at the fold, scroll skews/weights the letters (variable
font axis or skew keyed to velocity). Tier 5 adds shader/hover distortion on the letters.
- Build: SplitText-style line/char wrappers (hand-rolled — split into spans) + FM stagger; marquee
  = duplicated track + linear loop (pause on hover); variable-font axis (wght/slnt) keyed to
  scroll velocity.
- Pairs well with an otherwise restrained page — this is the budget award-look.
- Fallback: static display type (still strong if the typography tokens are good).

### B10 — Scroll-Scrub Video Hero  `[Tier 4–5 · M · perf: medium]`
The existing Higgsfield flip-book pipeline (SKILL.md "3D scroll animation — scroll-scrubbed
hero" + `references/scroll-animation-best-practices.md`). In blueprint terms: a generated-video
set-piece scrubbed by scroll. Counts as the site's ONE heavy blueprint when rolled or when
`higgs_hero` is ON.

---

## Tier gating + the roll (how Step 2.5 uses this file)

| Tier | Blueprint budget | Allowed rolls |
|------|------------------|---------------|
| 1 | none — foundation craft only (and even Lenis is optional) | — |
| 2 | none heavy; foundation layer + subtle B4-lite reveals | B4 (CSS-lite only) |
| 3 | **one light blueprint** | B4 (CSS build) · B5 (CSS build) · B7 · B8 · B9 |
| 4 | **one heavy + optionally one light** (different sections) | heavy: B1 · B2 · B3 · B6 · B10 · B5 (WebGL) — light: any Tier-3 roll |
| 5 | **one signature heavy, fully expressed** + supporting light patterns; full creative liberty | any, including shader builds of B4/B5/B9 |

**Roll rules:**
- Roll **randomly among the allowed blueprints**, biased by the Niche Design Brief's motion
  character and imagery (photo-forward niches favor B4/B5; product objects favor B3/B10;
  place/space businesses favor B1/B2; editorial favors B6/B9). Log the roll + why in the run report.
- **Service-trust niches stay simple.** Tier 1–2 niches (trades, medical, legal, local services)
  never get a heavy blueprint by default — foundation craft only — unless the user EXPLICITLY
  asks ("go wild", "make it cinematic", names a blueprint). This mirrors the tier map's intent:
  the plumber's customer needs the phone number, not a fly-through.
- **One heavy blueprint per site, ever.** Two heavies in one page is a demo reel (same restraint
  guardrail as the registries). Light patterns may support in OTHER viewports.
- **Multi-site mandate:** blueprints are a differentiation axis — no two siblings share one
  (see SKILL.md multi-site table).
- **User override always wins** — an explicit request for a specific experience ("I want the
  exploding product", "camera fly-through") replaces the roll at any tier, including Tier 1–2.

**Dependencies by blueprint (add ONLY what the roll needs — Step 4):**
- Lenis: all Tier 3+ rolls
- GSAP + ScrollTrigger: B1 (camera tweens) · B2 (alt build) · B6 · B7 (alt build)
- R3F + drei: B1 · B2 · B3 (real-3D build) · B5 (WebGL build)
- ffmpeg + Higgsfield: B10 (and B3's generated-video build)
- Nothing beyond FM/CSS: B4 (T3) · B5 (T3) · B7 · B8 · B9

**Performance contract (all blueprints):** pause/kill RAF loops when offscreen or tab-hidden;
DRACO-compress models; cap canvas DPR at 2; heavy blueprints render a static fallback on
`prefers-reduced-motion` AND on mobile when the pattern is flagged mobile-hostile (B1/B2 reduce
to still or simplified scene under ~768px); never let an effect reduce text contrast below the
locked palette's ratios.
