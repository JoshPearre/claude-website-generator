---
name: website-generator
description: Generate a complete multi-file website project scaffold by composing UI prompts from a curated, design-intensity-tiered prompt library. Use this skill whenever the user wants to build, generate, scaffold, redesign, or create a website, web app, landing page, or marketing site — including phrases like "build me a website", "generate a site", "create a landing page", "make a site for my [business/niche]", or any request to produce a website for a specific niche or audience. The skill matches design intensity (minimal utility to experimental/maximalist) to the site's purpose and produces a real Vite + React project folder.
---

# Website Generator

Generates a complete, multi-file website project scaffold by composing UI prompts sampled from a curated prompt library in `prompts/`. Every prompt is tagged with a **design intensity (1-5)**, so the generated site's visual ambition matches its purpose — a hospital portal stays calm and functional; a designer's portfolio goes experimental. The output is a real project folder (Vite + React + Tailwind + Framer Motion by default), never a single-file artifact.

## When to use

Trigger on any request to build, generate, or scaffold a website, web app, landing page, or marketing site.

## Workflow overview

Intake (2 questions) → **niche design intelligence (study how the niche LOOKS → write a Niche Design Brief)** → pick a design tier **(brief confirms/adjusts the default)** → sample prompts from the library **(brief biases the categories)** → **Quality Trio (`impeccable teach` + `ui-ux-pro-max` + `design-taste-frontend`) to lock tokens — binding the brief's design direction (palette + fonts) when no brand is supplied** → compose a multi-file scaffold **(Tier 3-5: layer in motion — Framer Motion patterns + component registries (Magic UI · Cult UI · Skiper UI · Watermelon) per the brief's motion character, see "Motion & interactive components")** → source image assets **(keyed image API queried from the brief's style descriptor, downloaded into `public/`)** → **polish pass (`impeccable critique` + `polish`, plus `emil-design-eng` consult on Tier 3+)** → report what was used.

Follow the steps below in order. **Do not write any files until Step 4.** The **Quality Trio** call (see "Design quality" section below) happens between Step 3 and Step 4 and is required, not optional. The **polish pass** (Step 6) runs after Step 5 and is also required — the scaffold is not "done" until critique returns clean.

---

## Multi-site requests — total differentiation mandate (3+ designs)

**Trigger:** the user asks for **three or more** websites, designs, variations, options, versions, concepts, or mockups in a single request ("make me 3 sites", "give me 5 designs", "a few different versions", "some options for my restaurant").

When triggered you are **not** generating one site with variants — you run the **full pipeline (Steps 1–6) independently, once per site**, under one hard rule:

> **Every site in the set must be completely different from every other site in the set — on every axis below. Nothing is shared, reused, or reskinned across siblings.**

Differentiate each site on **all** of these, never a subset:

| Axis | What must differ across siblings |
|------|----------------------------------|
| **Design intensity** | Independently roll a **random tier 1–5 per site** — do **not** reuse the niche-default tier. Don't repeat an intensity until all five are used. Different intensity cascades into a different stack, motion level, and registry. |
| **Structure (HTML / JSX / DOM)** | Different section count and order, different page architecture, different component composition. Not the same skeleton reordered. |
| **CSS / design system** | Different palette, font pairing, spacing scale, and **style family** (e.g. minimal vs brutalist vs glassmorphism vs editorial). No palette or pairing repeats across the set. |
| **Every UI component** | No component pattern reused across siblings — a hero in site 1 may not reappear in site 2. Re-sample with **zero prompt overlap** between siblings (track what each site used; resample on collision). |
| **Layout / positioning** | Move things around: nav top vs side vs centered vs split; hero centered vs left vs asymmetric split; different grids and content order. |
| **Images** | Different source images and art direction per site — different **image-API queries** / Neurascapes collections / Canva compositions. **Vary the count:** some sites single-image, some multi-image (galleries, multiple photos per section). No image file shared across siblings. |
| **Copy / words** | Different headlines, body, CTAs, and tone per site. Not the same text reskinned. |

**Niche brief in multi-site mode:** when the batch shares one niche, derive the Niche Design Brief (Step 1.5) once, then give **each sibling a different design interpretation of it** — different reference exemplars, palette/accent, type character, motion character, and image-API queries. The brief informs imagery, motion, and reference vocabulary only; it does **not** lock the batch to one look — each sibling still **rolls its own random tier (1–5) and binds a different, unused design direction** per the rules above (those override the brief's single Tier and Design-direction fields).

**Mechanism:** loop the pipeline per site. For each site, independently (1) roll tier 1–5, (2) pick an **unused** design direction / palette / font / style, (3) sample a **non-overlapping** prompt set, (4) source a fresh image set with its own image count, (5) write fresh copy, (6) scaffold into its **own** project folder (`generated-site-<slug>-<n>/`), (7) run the Step 6 polish pass — then build the next site from scratch.

### Red flags — you are violating the mandate if you think:

- "I'll build one scaffold and swap the palette for the others" → that's one site reskinned, not three.
- "Same layout, different colors is different enough" → layout and positions must differ too.
- "I'll reuse the hero / nav / footer across all of them" → no component is reused across siblings.
- "They're all the same niche so they can share photos" → different images per site, and vary the count.
- "The same headline works for all three" → copy differs per site.
- "Sampling the same prompts again is fine" → zero prompt overlap across siblings.
- "I'll just give them all tier 3" → intensity is randomized 1–5 per site.

**All of these mean: stop, and generate the next site as if it were the only thing you were asked to build — from a blank folder.**

### Before delivering, verify every pair of sites differs on:

structure · palette / fonts / style · every component · layout positions · images (and image count) · copy · tier. If any two siblings match on a whole axis, regenerate the later one.

---

## Step 1 — Intake

Ask the user exactly two questions and wait for both answers:

1. **What's the niche?** (e.g. restaurant, SaaS dashboard, personal portfolio, wedding invite, law firm, gaming community, art portfolio, e-commerce, nonprofit.)
2. **What's it about / who's it for?** (one or two sentences of context.)

If either answer is ambiguous, ask one focused follow-up. Reach ~95% confidence about the niche and audience before continuing — guessing here wastes a whole generation.

## Step 1.5 — Niche design intelligence

Before picking a tier, **study how the best sites in this niche LOOK** and distill a **Niche Design Brief** that biases every downstream choice. This researches **visual/design conventions only — not content.** The client supplies real copy and facts later, so the generated site ships **niche-appropriate placeholder copy clearly marked for client replacement** (label it `[PLACEHOLDER — client to replace]`); do **not** research or invent factual claims, names, prices, or testimonials.

**No API key. Research runs on your built-in tools** — `WebSearch` + `WebFetch`/`defuddle`, the `firecrawl-*` skills (`firecrawl-search` / `firecrawl-scrape` / `firecrawl-map`), and the `research` / `deep-research` skills. You are already Claude with web access; nothing here needs a Claude or research API key. **Time-box it:** 4–6 reference sites, ~10 minutes, then lock the brief and move on. Capture **screenshots, not just markdown** — this is a *visual* study, so the rendered look is the signal.

**Research method (in order):**

1. **Category-leading brand sites (2–3 best-in-class).** Search `"best [niche] website design [year]"` / `"[niche] award winning website"`. Extract hero archetype, palette temperature, type personality, signature motion, photographic style. Shared traits = the genre convention; outliers = differentiation room.
2. **Inspiration galleries, filtered by industry.** Land-book (industry filters), SiteInspire (category + style), Awwwards / Godly / FWA (Tier 4–5 craft ceiling), One Page Love / Lapa Ninja (section rhythm + CTA patterns), Mobbin (SaaS/app/fintech UI). Tally frequencies across 8+ examples — **the mode is the niche convention.**
3. **Dribbble / Behance — art-direction layer.** Read as *direction*, not literal layout: color moods, illustration-vs-photography lean, texture/shape vocabulary.
4. **2–3 real competitor sites.** The lived convention — nav style, CTA placement, trust signals, photo subjects — so the site reads native, not alien.

**Synthesis rule:** Convention = the **mode** across galleries + competitors (so it reads *native*). Differentiation = one or two deliberate moves beyond the mode, borrowed from the award tier (so it reads *designed*, not templated slop). Capture both.

**Design-dimension checklist — capture each, route it to a real lever:**

| Dimension | Capture | Routes to |
|-----------|---------|-----------|
| **Color / palette** | temperature, saturation, contrast, accent behavior, light/dark default | design-direction token-lock + `--accent` override (Step 3.5) |
| **Typography** | serif/sans/mono display, weight contrast, type-as-hero? | the direction's font stacks (Step 3.5); type-forward → bias `animations` |
| **Imagery art-direction** | subject · lighting · mood · composition · photo/illustration/3D | the Step 5 style descriptor + image query seeds |
| **Layout & density** | grid type, density, section sequence | tier confirmation + which prompt categories to sample |
| **Motion & scroll** | still / light reveals / scroll storytelling / heavy | Framer patterns by tier + registry choice |
| **Shape / texture** | radius language, texture, border, icon style | direction posture + `backgrounds`/`interactive-elements` bias |
| **Native components** | hero · nav · card · section rhythm | prompt-category sampling bias + scaffold component order |

**The Niche Design Brief (artifact).** Emit it at the end of this step and **save it to the project root as `NICHE-BRIEF.md`** (write it before Step 4 creates the folder, then move it in; or create the folder now and write it there). Echo the key lines into the generated `README.md`. Fields:

- **Niche & read** — what genre signal it must read as, to whom
- **Tier** — 1–5 + name (confirms/adjusts the `tier-niche-map` default)
- **Design direction (token-lock)** — `modern-minimal` | `tech-utility` | `human-approachable` | `editorial-monocle` | `brutalist-experimental` (or brand/URL source if supplied)
- **Palette mood** — temperature, saturation, contrast, light/dark + **accent override** (`--accent` value or "keep direction default")
- **Type character** — serif/sans/mono display · weight contrast · type-as-hero? · letter-spacing
- **Imagery direction** — the Step 5 style descriptor: subject · lighting · mood · composition · photo/illustration/3D
- **Image query seeds** — 3+ search strings for the Step 5 keyed image API
- **Layout & density** — grid type · density · section sequence
- **Motion character** — still / light reveals / scroll storytelling / heavy + Framer patterns + registry (or none)
- **Shape / texture** — radius language · texture · border · icon style
- **Native components** — hero · nav · card · section order
- **`ui-ux-pro-max`** — style + UX focus (only when a direction is active)
- **Prompt categories to sample** — 5–8 of the 10
- **One deliberate departure** — the single bold move beyond the genre mode
- **Anti-references** — generic AI-slop traits to steer away from for this niche
- **Reference brands + galleries used**

**How the brief feeds the rest of the pipeline:**

- **Step 2 (tier):** the brief's density/expressiveness confirms or nudges the `tier-niche-map` default.
- **Step 3 (sampling):** sample the prompt categories the brief lists, and pick `hero-sections`/`navigation`/`cards-and-grids` prompts whose archetype matches the captured native components.
- **Step 3.5 (token-lock):** bind the brief's design direction into `:root`, apply its `--accent` override (this extends the existing user-triggered `--accent` lever to a research-derived value — same lever), honor its type character. `ui-ux-pro-max` contributes only style + UX focus when a direction is active (never stack two palettes).
- **Step 5 (images):** the brief's **imagery direction is the single style descriptor**, and its **image query seeds** are the keyed-API search strings.
- **Motion:** the brief's motion character selects the Framer pattern set and the one registry flavor (calm trust-niches stay at hover + light reveals — never bolt scroll spectacle on).

**Worked example — Mexican restaurant (marketing/menu site):**

```
## Niche Design Brief — Mexican restaurant
- Niche & read: marketing/menu site — warm, appetizing, hospitable to diners
- Tier: 3 — Modern Consumer (tier-niche-map default, confirmed)
- Design direction: human-approachable + accent override
- Palette mood: warm, saturated, high-appetite contrast, light default
  → --accent ≈ oklch(58% 0.16 40) terracotta (keep direction bg/fg)
- Type character: friendly sans display, strong weight contrast, readable body — NOT a thin luxury serif
- Imagery direction: overhead & tight-crop food close-ups, warm golden light, hand-and-plate candor, real photography (not illustration)
- Image query seeds: ["warm rustic Mexican tacos close-up natural golden light", "colorful salsa molcajete overhead terracotta", "lively taqueria interior warm string lights"]
- Layout & density: photo-forward; hero/menu/gallery/reserve/footer
- Motion character: light reveals + stagger (whileInView + staggerChildren on menu cards); registry: none
- Shape/texture: comfortable radii (12–18px), subtle warm grain, image-led cards, solid-warm icons
- Native components: hero=full-bleed food photo + reserve CTA · nav=minimal sticky, logo left · card=image-led dish cards
- ui-ux-pro-max: style=warm photographic minimalism · UX focus=touch targets, image performance, clear booking CTA
- Prompt categories: hero-sections, navigation, cards-and-grids, scroll-effects, forms-and-inputs, footers
- One deliberate departure: a single oversized hand-painted dish name as a typographic accent
- Anti-references: cartoon sombrero/cactus clichés, muddy browns, dark moody lighting that kills appetite
- Reference brands: Tacombi, Bartaco, Calo · galleries: Land-book "Food & Drinks", SiteInspire "Food", Lapa Ninja
```

## Step 2 — Select the design intensity tier

The tier system is the core of this skill. There are five tiers:

| Tier | Name | Intensity band | Character |
|------|------|----------------|-----------|
| 1 | Utility / Operational | 1-2 | Minimal, fast, functional. Hover states only, no decorative motion. High information density. |
| 2 | Restrained Professional | 2-3 | Polished and modern. Light motion, tasteful gradients, subtle scroll reveals. Trust-building. |
| 3 | Modern Consumer | 3 | Balanced. Modern animation, some interactivity, clear hierarchy. Most websites live here. |
| 4 | Expressive Brand | 3-4 | Bold. Custom animation, scroll-driven storytelling, distinctive type. Personality-forward. |
| 5 | Experimental / Maximalist | 4-5 | 3D, WebGL, generative effects, unconventional layout, heavy motion. Memorable over usable. |

**To pick a tier:** read `tiers/tier-niche-map.md` and match the user's niche to its default tier. If the niche is ambiguous (e.g. "restaurant" could be a POS dashboard *or* a marketing site), use the Step 1 "what's it about" answer to disambiguate. If it is still unclear, default to **Tier 3** and tell the user.

**Then output exactly one transparency line before generating:**

> Niche: [niche]. Selected design tier: [N] — [tier name]. Say "tone it down" or "go wild" to shift tiers.

If the user replies with a shift command, move one tier down ("tone it down") or up ("go wild") and **resample (Step 3) at the new tier** before creating any files.

**If the user has not named a brand or given a reference URL**, also auto-select a **design direction** here — the aesthetic anchor that supplies palette + fonts (see `directions/design-directions.md` for the soft niche/tier → direction map) — and name it in the same line, e.g. *"Direction: Modern minimal — name a brand or pick another direction to change the look."* The direction is bound to `:root` in Step 3.5; naming it now lets the user redirect early.

## Step 3 — Sample prompts from the library

The library is split into ten categories, each its own file in `prompts/` (see `prompts/_index.md` for the map):

`hero-sections` · `scroll-effects` · `3d-components` · `interactive-elements` · `navigation` · `cards-and-grids` · `forms-and-inputs` · `footers` · `animations` · `backgrounds`

Sampling rules:

1. Pick **5-8 categories** relevant to this niche (a portfolio needs hero + 3d-components + scroll-effects; a law firm needs hero + navigation + cards-and-grids + forms-and-inputs + footers).
2. **Read only the category files you picked** — never load the whole library at once. This keeps each invocation cheap.
3. From each chosen file, randomly select **1-2 prompts whose `Best for tiers` field includes the selected tier**.
4. **Variety rule:** the full selected set must span at least **3 different source libraries** (the `Source:` field). If it doesn't, resample until it does. This stops every site from looking like one library's house style.

If the library is still empty (no prompts harvested yet), tell the user to run the harvest first — see `harvest/HARVEST_GUIDE.md`.

## Step 4 — Compose the project scaffold

Default stack: **Vite + React + Tailwind + Framer Motion**. Use vanilla HTML/CSS/JS instead if the sampled prompts clearly call for it, or a leaner stack for Tier 1 (a static dashboard rarely needs Framer Motion). For **Tier 4-5 sites that sample 3D prompts**, also add **React Three Fiber** (`@react-three/fiber` + `@react-three/drei`) for code-based 3D, or **`@splinetool/react-spline`** to embed a Spline scene — add these only when a 3D prompt is actually used so non-3D sites stay lean.

**Motion & interactive components.** Framer Motion is already in the default stack — use it as the baseline for reveals, layout transitions, and gestures (the "Motion & interactive components" section maps which patterns to reach for per tier). For higher-impact pieces — animated heroes, particle/shader backgrounds, marquees, tactile buttons, scroll storytelling, charts, or whole blocks — pull from a **shadcn component registry** rather than hand-rolling. Four are wired in, each with a distinct flavor: **Magic UI** (ambient effects, animated text), **Cult UI** (tactile/shader heroes, iOS widgets), **Skiper UI** (Apple-style scroll storytelling), and **Watermelon** (app/dashboard UI, charts, blocks). See `references/component-registries.md` for the full catalogs, flavors, licenses, and family→category maps.

When the sampled set includes *any* registry component, wire shadcn into the scaffold once:

- Run `npx shadcn@latest init` in the project root (choose **Vite** — sets `rsc: false`, correct for an SPA; creates `components.json`).
- Add the `@/` path alias so registry imports resolve: set `resolve.alias['@']` → `./src` in `vite.config.js`, and add a `jsconfig.json` with `"paths": { "@/*": ["./src/*"] }`.
- Register the registries you'll pull from in `components.json` `registries` (`@magicui`, `@cult-ui`, `@skiper-ui` → their `/r/{name}.json` URLs; Watermelon installs by direct URL). The reference file has the exact map.
- Pull **only the components actually sampled**: e.g. `npx shadcn@latest add @cult-ui/<slug>` or `npx shadcn@latest add "https://registry.watermelon.sh/<name>.json"`. They install as `.tsx` under `src/components/ui/` and coexist with the scaffold's `.jsx` — Vite compiles both.
- **Tier 1-2:** skip the *effect* registries (Magic UI / Cult UI / Skiper UI) — those stay lean with Framer Motion or no motion. **Watermelon is the exception** — its forms, tables, charts, dashboards, and login blocks are functional (not decorative) and fit utility/operational sites and admin panels well.

Create the project in the **user's current working directory** (not inside this skill folder):

```
generated-site-<timestamp>/
├── package.json
├── vite.config.js
├── tailwind.config.js
├── index.html
├── .gitignore          # .env, node_modules/, dist/
├── .env.example        # only if the site needs runtime keys
├── src/
│   ├── main.jsx
│   ├── App.jsx
│   ├── components/     # one component per sampled prompt
│   └── styles/
├── public/             # generated images land here
└── README.md           # tier, niche, sampled prompts + sources
```

Before writing files, confirm there is a `.gitignore` and that `.env` is listed in it. Never hardcode secrets — if the site needs a key, put a blank entry in `.env.example` and reference it via `import.meta.env`. **This runtime-key pattern (`import.meta.env` / `VITE_`) is only for keys the deployed site genuinely needs at runtime — the Step 5 image-API key is *not* one of them: it is generation-time only and must never be exposed via `import.meta.env` or a `VITE_`/`NEXT_PUBLIC_` prefix.**

At the top of `src/App.jsx`, include a comment block logging the chosen tier, the niche, and every sampled prompt (title + source + URL). The generated `README.md` repeats this so the user can trace every design decision.

## Step 5 — Image assets (keyed image API first, then Neurascapes → Canva → placeholder)

Sites need images — hero visuals, section photos, atmospheric backgrounds, og:image. Source them through a fallback chain, top to bottom; the first tier that yields a usable image for a slot wins. **Reuse the single style descriptor from the Niche Design Brief (Step 1.5) for every image** so the whole set reads cohesive.

1. **List the image slots** the site needs and confirm the brief's style descriptor (subject · lighting · mood · composition · photo/illustration/3D) + one palette/temperature anchor. Build each query from the brief's **image query seeds** plus that locked style + mood lexicon — only the subject slot varies per slot. Pass geometry as the API `orientation` param (hero → `landscape`, card/avatar → `square` [Unsplash: `squarish`], sidebar → `portrait`), not in the text. Track chosen image ids in a `Set` to de-dupe.

2. **Keyed image API first — the primary niche-tailored source.** When an image-API key is present, search it with the brief's query seeds, then **download the chosen file's bytes into `public/`** (download, don't hot-link). Two providers, auto-selected by which key is set:
   - **Pexels (recommended default)** — free, generous limits, **no required attribution**. `GET https://api.pexels.com/v1/search?query=…&orientation=…&per_page=15` with header `Authorization: <PEXELS_API_KEY>`; download `src.large2x`.
   - **Unsplash (alternative)** — `GET https://api.unsplash.com/search/photos?query=…&orientation=…` with header `Authorization: Client-ID <UNSPLASH_ACCESS_KEY>`; download `urls.full`/`urls.regular`. **Unsplash adds two mandatory compliance steps:** (a) the instant an image is selected, fire one authenticated `GET` to the photo's `links.download_location` (tracking only — discard the response body); (b) **render visible attribution** — "Photo by [`user.name`](`user.links.html`) on [Unsplash](https://unsplash.com)" with `?utm_source=website_generator&utm_medium=referral` on both links — written into the generated `README.md`/`CREDITS.md` and a page credit. Pexels needs neither.
   - **If both keys are set, Pexels is used. If neither is set, skip silently to Neurascapes** — generation never hard-fails for a missing key.

3. **Neurascapes — for photographic / atmospheric slots the API missed.** [Neurascapes](https://www.neurascapes.com/) is a free, royalty-free library of curated AI photos; commercial use, no attribution. Server-rendered, so a plain `WebFetch` works (no API/MCP). Match a collection to the niche + style descriptor and **download into `public/`.**

4. **Canva — for designed graphics and missing matches.** When the slot needs a *designed* asset (og:image, branded composition, illustration, anything with text/layout), generate it with the **Canva MCP**: `generate-design` / `generate-design-structured` → `export-design` to PNG → save into `public/`. One-time browser login (free account) on first call is expected.

5. **Fallback placeholder (always succeeds).** If nothing supplies an image, leave a styled `<div>` in the brief's palette plus a comment naming exactly what image belongs there — the layout never breaks and the user can drop one in later.

Apply the same style descriptor across every tier above so API photos, Neurascapes, and Canva assets sit together cleanly.

> **Security — generation-time only; NEVER prefix an image key with `VITE_`/`NEXT_PUBLIC_` and never write it into the generated project (that inlines it into the public bundle).** The image-API key is read **only here, at generation time**, from a process env var (`PEXELS_API_KEY` / `UNSPLASH_ACCESS_KEY`), optionally loaded from a **gitignored `.env` in this skill folder** (see the skill's `.env.example`). The key calls `api.pexels.com` / `api.unsplash.com`, the **bytes are downloaded into `public/`**, and the key is dropped there forever — not into the project's `.env`, `vite.config`, a config file, or an `<img>` URL. The only artifact crossing into the shipped site is image bytes. This skill repo is **public** — never commit a real key.

## Step 6 — Polish pass (required before reporting done)

Once Step 5 finishes, the scaffold is "compiled" but not yet "designed". Run the polish pass before telling the user the site is done:

1. `Skill(skill="impeccable", args="critique")` — flags anti-patterns in the just-written code (generic gradients, cards-on-cards, lorem still in place, weak hierarchy, missing contrast).
2. `Skill(skill="impeccable", args="polish")` — applies the fixes critique surfaced.
3. Loop 1–2 until critique returns clean.
4. **Tier 3+ only:** `Skill(skill="emil-design-eng")` — consult on motion, hover/focus states, and transition timing for the hero, primary CTA, nav, and key cards. Skip on Tier 1-2 sites where motion is minimal.
5. **Production-bound only:** `Skill(skill="impeccable", args="audit")` — final 29-rule deterministic anti-pattern scan, JSON output suitable for a CI gate.

See the "Design quality" section below for the full call signatures and when to deviate.

## Motion & interactive components (Tier 3-5)

Motion is what separates a Tier 3-5 site from a static brochure. Two layers, used together.

### Layer 1 — Framer Motion (baseline, already in the stack)

Reach for these patterns by tier. Respect `prefers-reduced-motion` on all of them (`useReducedMotion()` → fall back to opacity-only or no motion):

| Pattern | API | Use for | Tier |
|---------|-----|---------|------|
| Entrance reveals | `initial` / `whileInView` + `viewport={{ once: true }}` | sections fading / sliding in on scroll | 3+ |
| Stagger | `staggerChildren` on a parent `variants` | lists, card grids, nav items cascading in | 3+ |
| Scroll-linked | `useScroll` + `useTransform` | parallax, progress bars, pinned scroll storytelling | 4-5 |
| Layout animation | `layout` prop + `<AnimatePresence>` | tabs, filters, reordering, modal / route transitions | 3+ |
| Gestures | `whileHover` / `whileTap` / `drag` | buttons, cards, draggable galleries | 3+ |
| Spring physics | `transition={{ type: 'spring', stiffness, damping }}` | anything that should feel tactile, not linear | 4-5 |

Keep durations honest (150–400ms for UI; longer only for hero set-pieces) and easing intentional — `emil-design-eng` reviews exactly this in the Step 6 polish pass.

### Layer 2 — Component registries (high-impact, per-component)

For pieces that are tedious or error-prone to hand-roll, pull from a **shadcn registry** instead of writing them from scratch. Four are wired in — **pick the one whose flavor matches the locked design direction** (don't pull from all four; see the restraint guardrail):

| Registry | Flavor — reach for it when… | Best tiers | License/cost |
|----------|------------------------------|-----------|--------------|
| **Magic UI** | ambient effects, animated text, globe/particles, marquee, shiny buttons | 3-5 | MIT, free |
| **Cult UI** | tactile/premium — shader & liquid-metal heroes, metal/neumorph buttons, iOS widgets (dynamic island, family button) | 3-5 | free + paid Pro |
| **Skiper UI** | Apple-style scroll storytelling — clip-path carousels, card-stack scroll, parallax/blur text | 3-5 (scroll) | free + paid |
| **Watermelon** | functional breadth — forms, tables, charts (Recharts), dashboards, whole blocks/login forms | 1-4 | MIT, free |

**Full catalogs, family→category maps, exact install commands, and per-registry caveats live in `references/component-registries.md` — read it once you've picked a registry.**

Magic UI also ships its own Claude skill — call `Skill(skill="magic-ui")` for its catalog (`references/components.md`), section recipes, and install contract. Its family map (kept inline because it's the default motion source):

| Your category | Magic UI family | Components |
|---------------|-----------------|-----------|
| `hero-sections` / `3d-components` | Hero & Visual Anchors | `globe`, `warp-background`, `animated-grid-pattern`, `retro-grid` |
| `backgrounds` | Ambient Effects | `particles`, `flickering-grid`, `dot-pattern`, `grid-pattern`, `light-rays` |
| `animations` | Text Motion | `blur-fade`, `text-animate`, `word-rotate`, `sparkles-text`, `typing-animation` |
| `interactive-elements` | Buttons & CTA | `shiny-button`, `shimmer-button`, `rainbow-button`, `ripple-button` |
| `cards-and-grids` / `navigation` | Layout & Social Proof | `marquee`, `avatar-circles`, `bento-grid` |

Cult UI, Skiper UI, and Watermelon are plain registries (no Claude skill) — install them per the contract in the reference file. Some components need extra deps (e.g. `cobe` + `motion` for Magic UI's `globe`, `recharts` for Watermelon charts) or global CSS keyframes (e.g. `marquee`) — the reference file and each component's page flag these.

### Restraint guardrail (required)

More motion is not more better. Magic UI's own guidance and `emil-design-eng` enforce the same rule:

- **Pick one registry flavor per site, not all four.** A Cult liquid-metal hero + a Magic UI globe + a Skiper scroll carousel stacked in one viewport reads as a demo reel, not a designed site — the variety this skill wants is *across* generations (each site feels different), not piled *within* one.
- **Start with 1 primary effect + 1 supporting effect per viewport** — one hero anchor (e.g. `globe`) plus one ambient layer (e.g. `particles`), not three stacked effects.
- **Never stack multiple expensive animated backgrounds** in one viewport — it wrecks performance and contrast.
- **Animated backgrounds must not reduce text contrast** — verify against the locked palette from the Quality Trio.
- The Step 6 polish pass runs `emil-design-eng` (Tier 3+) specifically to catch over-animation, janky timing, and motion that fights the content. If it flags excess, **cut motion — don't add more.**

## Design quality — call companion skills

This skill chooses *what* to build; the companion skills below make it look good. The **Quality Trio** (`impeccable` + `ui-ux-pro-max` + `design-taste-frontend`) is **required** on every generation, and the **polish pass** (`impeccable critique` + `polish`, plus `emil-design-eng` on Tier 3+) is **required** before reporting the site as done. Everything else is situational.

### Required: the Quality Trio (Step 3.5, before writing any files)

After sampling prompts in Step 3 and **before** composing the scaffold in Step 4, invoke all three required skills via the `Skill` tool **in this order** — each one informs the next, so order matters.

**1. `impeccable` — load design knowledge first.**

```
Skill(skill="impeccable", args="teach")
```

Loads Impeccable's seven reference files (typography, color, motion, spatial, interaction, responsive, UX writing) so every later decision speaks the same design vocabulary. Without this, the agent improvises and the result looks AI-generated. Then optionally run `Skill(skill="impeccable", args="shape")` to capture audience + brand voice + anti-references into a project-local `PRODUCT.md` / `DESIGN.md` that all later commands read from.

**2. `ui-ux-pro-max` — lock the design tokens.**

First resolve **where palette + fonts come from** (first match wins, stop there):

1. **User named a brand** → `awesome-design-md` / `design-md` (bind that DESIGN.md).
2. **User gave a reference URL** → `extract-design` (bind extracted tokens).
3. **Neither** → bind a **design direction** from `directions/design-directions.md` verbatim into `:root` (the one named in the Step 2 line, or re-pick from its soft map). Five hand-tuned OKLch palette + font sets — deterministic, anti-slop, honour the posture cues.

Then call `ui-ux-pro-max` for the rest:

```
Skill(skill="ui-ux-pro-max", args="--niche <niche> --tier <N> --stack vite-react-tailwind")
```

Pass the niche from Step 1, the tier from Step 2, the stack from Step 4. Pin what it returns — **but when a brand, reference, or direction is active above, take only its `Style` + `UX guideline focus`; do not also apply its palette/pairing (never stack two palettes):**

- **Color palette** — one of 161 curated palettes matching niche + tier *(use only as the rung-4 fallback when no brand/reference/direction applies)*
- **Font pairing** — one of 57 pairings (headline + body) *(same fallback rule)*
- **Style** — one of 50+ styles (glassmorphism, brutalism, bento, minimalism, etc.) matching the tier band
- **UX guideline focus** — priority 1–10 categories (accessibility, touch targets, animation timing, etc.)

**3. `design-taste-frontend` — anti-slop direction check.**

```
Skill(skill="design-taste-frontend")
```

Taste Skill v2 reads the brief and infers whether the chosen direction looks templated. It is the final gate before file writes — if it flags the direction as generic, **resample (Step 3) and re-run the trio**, do not just push through.

Write all locked tokens into `src/styles/tokens.css` (or `tailwind.config.js` `theme.extend`) and reference them in every component. The generated `README.md` must list the chosen design direction (or brand/reference source) plus the exact palette, font pairing, and style.

### Required: Step 6 — polish pass (after files exist)

After Step 5 (image assets) and **before** declaring the site done, run the polish pass to catch anything the scaffold improvised:

```
Skill(skill="impeccable", args="critique")
Skill(skill="impeccable", args="polish")
```

`/impeccable critique` flags anti-patterns (generic gradients, cards-on-cards, lorem still in place, weak hierarchy, missing contrast). `/impeccable polish` applies the fixes. Re-run both until critique returns clean. Run `/impeccable audit` for a final 29-rule deterministic anti-pattern scan if the site is going to production.

**For Tier 3+ generations, also invoke `emil-design-eng`:**

```
Skill(skill="emil-design-eng")
```

Encodes Emil Kowalski's philosophy on UI polish, component design, animation decisions, and the invisible details — timing curves, layered transitions, micro-interactions — that separate "fine" from "great". Use it case-by-case to review motion, hover/focus states, and transition timing on the components most worth getting right (hero, primary CTA, nav, key cards). Skip on Tier 1-2 sites where motion is minimal anyway.

### Tier-mapped style overrides (use one *instead of* `ui-ux-pro-max`'s style)

These are bundled with Taste Skill v2 and live as standalone skills. Pick one when the user's brief clearly calls for that aesthetic — it replaces the style choice from the trio, not the palette/font:

| Skill | Use when |
|-------|----------|
| `minimalist-ui` | Tier 1-2, editorial / publication / clean B2B — warm monochrome, flat bento, no gradients |
| `high-end-visual-design` | Tier 3-4, premium agency feel — calibrated fonts/spacing/shadows that read "expensive" |
| `industrial-brutalist-ui` | Tier 4-5, data-heavy dashboards / portfolios wanting declassified-blueprint vibes |
| `gpt-taste` | Tier 4-5, GSAP-heavy scroll storytelling with strict AIDA structure |
| `stitch-design-taste` | When the user wants a DESIGN.md emitted for Google Stitch as a deliverable |

### Image generation alternatives (Step 5 supplements to the image API / Neurascapes / Canva)

| Skill | Use when |
|-------|----------|
| `imagegen-frontend-web` | Section-by-section design refs needed before coding — one image per section |
| `imagegen-frontend-mobile` | Mobile app companion screens — premium phone-mockup framing |
| `image-to-code` | Visually-critical site — generate references first, then implement against them |
| `brandkit` | Need a brand-kit board / identity deck / logo system to ship alongside the site |

### Situational companions

- **`frontend-design`** — read first when the site is component-heavy. Production-grade frontend patterns; avoids generic AI aesthetics.
- **`redesign-existing-projects`** — when the user gives an existing site to upgrade rather than building from scratch. Audits current design, identifies generic AI patterns, applies high-end standards without breaking functionality.
- **`awesome-design-md`** / **`design-md`** — pull a brand DESIGN.md (Stripe, Linear, Notion, etc.) when the user names a brand or wants a specific aesthetic. Overrides `ui-ux-pro-max` style choice when invoked.
- **`theme-factory`** — apply one of 10 pre-set themes to the scaffold if the user wants a quick coherent look instead of a custom palette.
- **`extract-design`** — if the user gives a reference URL, extract its real design tokens and feed them into the theme (overrides `ui-ux-pro-max` palette + pairing).
- **`full-output-enforcement`** — invoke if the scaffold starts emitting `// rest of code` or `...` placeholders. Forces unabridged output.
- **`magic-ui`** — animated component registry (globes, particle fields, marquees, shimmer/shiny buttons, animated text). The primary motion source for Tier 3-5 — see "Motion & interactive components" above for the family → category map and install contract.
- **Component registries (Cult UI · Skiper UI · Watermelon)** — shadcn registries (not skills) for animated/functional components beyond Magic UI. Cult = tactile/shader heroes + iOS widgets; Skiper = Apple-style scroll storytelling; Watermelon = app/dashboard UI, charts, and blocks (and the only registry that also serves Tier 1-2). Full catalogs + install contract in `references/component-registries.md`.

## Companion skills manifest (`skills-lock.json`)

The design skills this workflow calls (`impeccable`, `design-taste-frontend` + its sub-skills, `emil-design-eng`, `magic-ui`) are installed **globally** in `~/.claude/skills/` and are invoked by name via the `Skill` tool — nothing here reads a lock file at runtime. For provenance and reinstall/update, this skill ships its own manifest at `skills-lock.json` (next to this file), listing all 16 companion skills with their GitHub source, in-repo path, and content hash.

- **To reinstall or update the companion skills:** run `npx skills update` (or re-run the per-source `npx skills add <source>` commands) from this skill's directory so the CLI picks up `skills-lock.json`.
- **Sources of record:** `pbakaus/impeccable` (Impeccable), `Leonxlnx/taste-skill` (Taste Skill v2 + the 12 bundled sub-skills), `emilkowalski/skill` (`emil-design-eng`), `magicuidesign/magicui` (`magic-ui`).
- If a companion skill ever fails to invoke, confirm it still exists in `~/.claude/skills/`; if not, reinstall from the source above.
- **Not tracked here:** the shadcn component registries (Cult UI, Skiper UI, Watermelon) are npm/CLI registries installed per-component into *generated projects*, not Claude skills — so they're absent from `skills-lock.json`. Magic UI appears in the manifest only because it *additionally* ships a Claude skill. The registries are documented in `references/component-registries.md`.

## Safety and cost

- Reach ~95% confidence before writing files; ask focused follow-ups when unsure.
- Never hardcode secrets. Use `.env` + `.env.example`, and verify `.gitignore` before creating files.
- Load only the prompt category files you need — never the whole library.
- The library grows via the harvest workflow in `harvest/` — see `harvest/HARVEST_GUIDE.md`.
