---
name: website-generator
description: Generate a complete multi-file website project scaffold by composing UI prompts from a curated, design-intensity-tiered prompt library. Use this skill whenever the user wants to build, generate, scaffold, redesign, or create a website, web app, landing page, or marketing site — including phrases like "build me a website", "generate a site", "create a landing page", "make a site for my [business/niche]", or any request to produce a website for a specific niche or audience. The skill matches design intensity (minimal utility to experimental/maximalist) to the site's purpose and produces a real Vite + React project folder.
---

# Website Generator

Generates a complete, multi-file website project scaffold by composing UI prompts sampled from a curated prompt library in `prompts/`. Every prompt is tagged with a **design intensity (1-5)**, so the generated site's visual ambition matches its purpose — a hospital portal stays calm and functional; a designer's portfolio goes experimental. The output is a real project folder (Vite + React + Tailwind + Framer Motion by default), never a single-file artifact.

## When to use

Trigger on any request to build, generate, or scaffold a website, web app, landing page, or marketing site.

## Workflow overview

Intake (2 questions) → pick a design tier → sample prompts from the library → **Quality Trio (`impeccable teach` + `ui-ux-pro-max` + `design-taste-frontend`) to lock tokens — binding a deterministic design direction when no brand is supplied** → compose a multi-file scaffold **(Tier 3-5: layer in motion — Framer Motion patterns + component registries (Magic UI · Cult UI · Skiper UI · Watermelon), see "Motion & interactive components")** → source image assets → **polish pass (`impeccable critique` + `polish`, plus `emil-design-eng` consult on Tier 3+)** → report what was used.

Follow the steps below in order. **Do not write any files until Step 4.** The **Quality Trio** call (see "Design quality" section below) happens between Step 3 and Step 4 and is required, not optional. The **polish pass** (Step 6) runs after Step 5 and is also required — the scaffold is not "done" until critique returns clean.

---

## Step 1 — Intake

Ask the user exactly two questions and wait for both answers:

1. **What's the niche?** (e.g. restaurant, SaaS dashboard, personal portfolio, wedding invite, law firm, gaming community, art portfolio, e-commerce, nonprofit.)
2. **What's it about / who's it for?** (one or two sentences of context.)

If either answer is ambiguous, ask one focused follow-up. Reach ~95% confidence about the niche and audience before continuing — guessing here wastes a whole generation.

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

Before writing files, confirm there is a `.gitignore` and that `.env` is listed in it. Never hardcode secrets — if the site needs a key, put a blank entry in `.env.example` and reference it via `import.meta.env`.

At the top of `src/App.jsx`, include a comment block logging the chosen tier, the niche, and every sampled prompt (title + source + URL). The generated `README.md` repeats this so the user can trace every design decision.

## Step 5 — Image assets (Neurascapes first, Canva fills the gaps)

Sites need images — hero visuals, section photos, atmospheric backgrounds, og:image. Source them from a **mix of two providers**, picking whichever fits each slot:

1. **List the image slots** the site needs, then derive one short **style descriptor** (palette + art direction + mood) from the theme. Reuse it for every image so the whole set looks cohesive.
2. **Neurascapes first — for photographic / atmospheric imagery.** [Neurascapes](https://www.neurascapes.com/) is a free, royalty-free library of curated AI-generated photos grouped into themed collections; its license allows commercial use with no attribution. It is server-rendered, so a plain `WebFetch` works — it has no API/MCP, so just fetch the site and pick from what's there. For each photographic, lifestyle, or mood slot, find a collection/image matching the niche + style descriptor and **download the match into `public/`** (download, don't hot-link — the site should own its assets).
3. **Canva fills the gaps — for designed graphics and missing matches.** When Neurascapes has no good match, or the slot needs a *designed* asset (og:image, branded composition, illustration, anything with text or layout), generate it with the **Canva MCP**: `generate-design` / `generate-design-structured` → `export-design` to PNG → save into `public/`. The Canva MCP authenticates via a one-time browser login (free Canva account); the first call prompting for it is expected.

A normal build ends up with a **mix** — real AI photos from Neurascapes where they fit, Canva-generated assets where they don't. Apply the same style descriptor to both so they sit together cleanly.

**Fallback:** if neither source supplies an image, leave a clearly-described placeholder — a styled `<div>` plus a comment naming exactly what image belongs there — so the site still runs and the user can drop one in later.

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

### Image generation alternatives (Step 5 supplements to Neurascapes / Canva)

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
