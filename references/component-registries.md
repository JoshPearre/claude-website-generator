# Component registries (shadcn) — catalog & install contract

This skill pulls high-impact React components from a small set of **shadcn registries** instead of hand-rolling effects that are tedious or error-prone to write from scratch. SKILL.md's "Motion & interactive components (Tier 3-5)" section points here for the full catalogs, the per-registry flavor, and the exact install contract.

## What a "registry" is (and why it's safe to use)

A shadcn registry is **not** an npm component package. When you run `npx shadcn add ...`, the CLI fetches the component's **source** (a `.tsx` file + any hook/util it needs) and writes it directly into your project under `src/components/ui/`. You then own that code outright — read it, edit it, delete it. There's no opaque runtime dependency layer and no post-install script executing on your machine beyond the normal `npm install` of standard libraries the component imports (e.g. `framer-motion`, `cobe`, `recharts`).

Practical consequence: treat an `add` like pasting code from the library's site. Skim what lands the first time you use an unfamiliar component, the same as any copy-paste UI. That's the whole trust model, and it's the same one Magic UI already uses here.

## How registries relate to the prompt library and tiers

The `prompts/` library decides *what* to build (layout, content, structure). Registries supply *polished, motion-rich pieces* to build it with. They don't replace sampling — they give a sampled prompt a stronger implementation. Reach for them on **Tier 3-5** for the animated/effect libraries, and on **Tier 1-4** for Watermelon's functional UI.

A site should pick **one or two registries that share a flavor**, not pull from all four. Mixing a liquid-metal Cult hero with a Magic UI globe and a Skiper scroll carousel and Watermelon charts in one viewport reads as a demo reel, not a designed site. The variety this skill wants is *across* generations (different sites feel different), not *within* one viewport.

---

## One-time wiring (do this once per project, only if a registry component is sampled)

The default scaffold is **Vite + React + Tailwind + Framer Motion** with `.jsx` files. Registry components install as `.tsx` and coexist fine — Vite compiles both.

1. **Init shadcn** (skip if `components.json` already exists):
   ```bash
   npx shadcn@latest init
   ```
   Choose **Vite** as the framework. This sets `"rsc": false` and `"tsx": true` in `components.json` — correct for a Vite SPA. Some registry docs show sample `components.json` with `"rsc": true` (that's their Next.js default) — **keep your Vite-generated `rsc: false`; don't copy theirs.** Registry components sometimes carry a `"use client"` directive at the top; in Vite that's a harmless no-op.

2. **Make the `@/` alias resolve** (needed so `@/components/ui/...` and `@/lib/utils` imports work):
   - `vite.config.js`: `resolve.alias['@']` → `./src`
   - add `jsconfig.json` with `"paths": { "@/*": ["./src/*"] }`

3. **Register the namespaced registries you'll pull from** in `components.json`. shadcn v3 resolves `@namespace/component` through this map:
   ```json
   "registries": {
     "@magicui":   "https://magicui.design/r/{name}.json",
     "@cult-ui":   "https://cult-ui.com/r/{name}.json",
     "@skiper-ui": "https://skiper-ui.com/r/{name}.json"
   }
   ```
   Watermelon installs by **direct URL** (no namespace needed): `https://registry.watermelon.sh/{name}.json`. You may add `"@watermelon": "https://registry.watermelon.sh/{name}.json"` if you prefer the namespace form.

> **Source of truth for the exact command:** every component's own page (and Watermelon's GitHub README) shows the canonical `npx shadcn add ...` line. When in doubt, copy it from there rather than guessing a slug — registries occasionally version or rename.

**Tier 1-2 note:** skip the *effect* registries (Magic UI / Cult UI / Skiper UI) — those sites stay lean with Framer Motion or no motion. But Tier 1-2 **may** use **Watermelon** for functional pieces (forms, tables, charts, dashboards, login blocks): those are utility components, not decorative motion, and a dashboard or admin panel benefits from solid copy-paste source.

---

## Magic UI — ambient effects & animated text (Tier 3-5)

- **License/cost:** MIT, free.
- **Reach for it when:** you want a hero anchor or ambient background (globe, particles, retro/animated grid), animated text (blur-fade, word-rotate, sparkles), a marquee, or a shiny/shimmer/rainbow button.
- **Install:** has its own Claude skill — call `Skill(skill="magic-ui")` for the catalog + recipes, then `npx shadcn@latest add @magicui/<slug>`.
- **Family → category map:** see the table in SKILL.md "Layer 2". Some components need extra deps (e.g. `cobe`+`motion` for `globe`) or global CSS keyframes (e.g. `marquee`) — its `components.md` flags these.

## Cult UI — tactile, shader-driven, iOS-flavored (Tier 3-5)

- **License/cost:** open source (free components via the registry); a paid **Cult Pro** tier (pro.cult-ui.com) adds premium blocks/templates. The free registry is plenty — don't reach for Pro unless the user asks.
- **Reach for it when:** the brief wants something *physical and premium* — a shader/gradient hero, a button that feels like brushed metal or soft neumorphism, or an Apple-style expandable widget. This is the flavor Magic UI doesn't have.
- **Install:** `npx shadcn@latest add @cult-ui/<slug>` (after registering `@cult-ui`). Cult also publishes an MCP server (`cult-ui.com/docs/mcp-server`) for IDE use — not needed for this skill, but available.
- **Stack note:** Cult components are built on shadcn theme variables, so they inherit your locked palette tokens cleanly.
- **Family → category map:**

  | Prompt category | Cult UI components |
  |---|---|
  | `hero-sections` / `backgrounds` | `hero-dithering`, `hero-liquid-metal`, `hero-color-panels`, `hero-heatmap`, `hero-static-radial-gradient`, `bg-media` |
  | `interactive-elements` (buttons) | `texture-button`, `metal-button`, `neumorph-button`, `border-beam-button`, `cosmic-button`, `bg-animate-button`, `gradient-button-group` |
  | `interactive-elements` (widgets) | `dynamic-island`, `family-button`, `toolbar-expandable`, `morph-surface`, `side-panel`, `expandable-screen` |
  | `cards-and-grids` | `texture-card`, `expandable-card`, `tweet-grid` |
  | `animations` / headings | `gradient-heading`, `text-gif` |
  | `navigation` / social proof | `logo-carousel` |

## Skiper UI — Apple-style scroll storytelling (Tier 3-5, scroll-heavy)

- **License/cost:** **free + paid.** Many components are free; premium ones show a symbol on the `/components` page and require a Pro account. Default to the free set; only use a premium slug if the user has Skiper Pro.
- **Reach for it when:** the site is built around *scroll* — a product launch, portfolio, or agency page where sections animate, pin, blur, or stack as you scroll. This is the flavor that pairs with the `scroll-effects` prompt category.
- **Install:** `npx shadcn add @skiper-ui/<slug>` (e.g. `@skiper-ui/skiper40`). Components land in `src/components/ui/skiper-ui/`. Many are numbered (`skiperNN`) — **copy the exact command from each component's page**, since the number is the slug.
- **Family → category map:**

  | Prompt category | Skiper UI components (representative) |
  |---|---|
  | `scroll-effects` | text-scroll-animation (`skiper31`), card-stack-scroll, scroll-with-blur (`skiper44`), parallax variants |
  | `hero-sections` | apple-feature-block (`skiper76`), feature hero variants |
  | `interactive-elements` | clip-path carousel, `dynamic-island`, apple-play-button, theme-toggle, rolling-text |
  | `cards-and-grids` | card-stack, clip-path carousel |
  | `animations` | rolling-text, marquee |

## Watermelon UI — breadth, app/dashboard UI & charts (Tier 1-4)

- **License/cost:** **MIT, fully free.** Open source at `WatermelonCorp/watermellon-registry`. No paid tier to worry about.
- **Reach for it when:** you need *functional* UI fast — form controls, tables, tabs, sidebars, dialogs, **charts**, or whole **blocks** (dashboards, login forms, page sections). This is the one registry that serves **Tier 1-2** (utility/operational, dashboards, admin panels) as well as Tier 3-4 marketing blocks. It's the breadth complement to the three effect libraries.
- **Stack note:** built for **React 19 + Tailwind CSS v4 + Radix + Framer Motion.** If the scaffold is on Tailwind v3, some components assume v4's CSS-first tokens — prefer a **Tailwind v4** scaffold when sampling Watermelon, or pick components that don't rely on v4-only theming. Charts pull in `recharts`.
- **Install:** `npx shadcn@latest add "https://registry.watermelon.sh/<name>.json"` — e.g. `.../button.json`, `.../animated-accordion.json`, `.../chart.json`.
- **Family → category map:**

  | Prompt category | Watermelon components |
  |---|---|
  | `forms-and-inputs` | button, input, checkbox, select, switch, slider, ai-input |
  | `cards-and-grids` | card, table, badge, avatar, alert, accordion |
  | `navigation` | breadcrumb, tabs, sidebar, carousel |
  | `interactive-elements` (feedback) | dialog, alert-dialog, toast (sonner), progress |
  | `cards-and-grids` / dashboards (charts) | area, bar, line, pie, radar (via Recharts) |
  | whole sections (blocks) | full page sections, dashboards, login forms |

---

## Choosing between them (quick heuristic)

- **Ambient effect / animated text / globe / marquee?** → Magic UI
- **Tactile, premium, shader hero or iOS widget?** → Cult UI
- **Scroll-driven storytelling?** → Skiper UI
- **Functional UI, dashboard, charts, or a whole block — including Tier 1-2?** → Watermelon UI

Pick the registry whose flavor matches the locked design direction, pull the **one or two** pieces the design actually needs, and let the restraint guardrail in SKILL.md keep it from becoming a demo reel.

---

## Added 2026-07-05 — three more registries + the motion stack

Same rules as the four above: pick ONE registry flavor per site, matched to the locked design
direction and the rolled experience blueprint (`references/experience-blueprints.md`).

### Aceternity UI — the contemporary award-site look
- **Flavor:** spotlight + background-beam heroes, 3D card tilts (perspective hover), sticky-scroll
  reveals, lamp/aurora effects, hero parallax, bento showcases. The house style of the modern
  dark-gradient award site — deploy with restraint on anything Tier 3.
- **Install:** shadcn-compatible registry — `npx shadcn@latest add https://ui.aceternity.com/registry/<slug>.json`
  (browse slugs at ui.aceternity.com/components). Components are React + Tailwind + Framer Motion
  (`motion` package); some need `clsx`/`tailwind-merge` (standard shadcn utils).
- **Best tiers:** 3–5. **License:** components free; templates paid.
- **Caveat:** its defaults skew dark-on-dark — re-bind the locked palette tokens; never ship its
  demo colors.

### ReactBits — animated text + micro-interaction variety
- **Flavor:** split/blur/decrypt/shiny text reveals, count-ups, hover cards, dock menus, threads —
  a wide grab-bag of small signature moves (strong for the B9 Kinetic Type blueprint).
- **Install:** copy-paste from reactbits.dev, or the jsrepo CLI (`npx jsrepo add <block>` per the
  site's per-component instructions). JS/TS + Tailwind/plain-CSS variants per component.
- **Best tiers:** 3–5. **License:** MIT, free.
- **Caveat:** quality varies more than the curated registries — run pulled components through the
  Step 6 polish pass and re-bind tokens.

### motion-primitives — restrained, production-lean motion
- **Flavor:** text effects, morphing dialogs, carousels, accordions, animated backgrounds that stay
  tasteful — the registry for Tier 2–4 sites that want polish without spectacle.
- **Install:** shadcn-compatible — `npx shadcn@latest add "https://motion-primitives.com/c/<slug>.json"`
  (browse at motion-primitives.com). React + Tailwind + `motion`.
- **Best tiers:** 2–4. **License:** MIT, free.

### The motion stack itself (not registries — npm deps per blueprint)
- **Lenis** (`npm i lenis`) — smooth scroll, ~3KB. Default on Tier 3+. `const lenis = new Lenis({ lerp: 0.1 })`
  + RAF loop; connect to ScrollTrigger via `lenis.on('scroll', ScrollTrigger.update)`.
- **GSAP + ScrollTrigger** (`npm i gsap`) — free including all plugins. Reach for it when the
  blueprint needs pinning, scrubbed timelines, or camera tweens (B1/B2/B6/B7); Framer Motion +
  CSS sticky covers lighter cases. `gsap.registerPlugin(ScrollTrigger)`.
- **React Three Fiber + drei** (`npm i three @react-three/fiber @react-three/drei`) — B1/B2/B3/B5
  WebGL builds. DRACO-compress GLTFs; cap `dpr={[1, 2]}`; pause offscreen.

### Kokonut UI — animated app/product components (added 2026-07-05, verified)
- **Flavor:** 100+ animated, product-feeling components — Apple Activity Card, card stacks,
  quick-pay/transfer cards, glass & liquid-glass effects. Reads "polished app", between Magic UI's
  ambient effects and Watermelon's functional blocks.
- **Install:** shadcn-compatible — `npx shadcn@latest add @kokonutui/<slug>` (browse kokonutui.com).
  React + TS + Tailwind v4 + Motion.
- **Best tiers:** 3–4. **License:** free; Pro = paid templates.
- **Caveat:** components assume Tailwind v4 — on the default Vite scaffold confirm the Tailwind
  major first, and re-bind locked tokens as always.

### Motion rebrand note (2026)
Framer Motion is now **Motion** — `npm i motion`, `import { motion } from "motion/react"`. The
`framer-motion` package still works but is legacy; new scaffolds should use `motion`. (Its docs'
"Curtains" page-transition examples are Motion+ paid — don't cite them as free references.)

### anime.js v4 — lightweight scroll/drag alternative (verified)
`npm i animejs` — MIT, ~6KB gzipped. `onScroll` observer + draggable + spring easings + SVG morph.
Reach for it on Tier 2–3 sites where GSAP would be overkill but you want scroll-linked accents
beyond Framer's `useScroll` (e.g. a single SVG line-draw or counter). One motion engine per
concern — don't run GSAP and anime.js on the same page.

### Bklit UI — design-engineered charts (added 2026-07-05, verified at bklit.com)
- **Flavor:** the charts specialist — 17 chart types: area, bar, candlestick, choropleth, composed,
  funnel, gauge, heatmap, line, LIVE line, pie, profit/loss, radar, ring, sankey, scatter, sunburst.
  Design-engineered (Vercel OSS program; endorsed by shadcn). Has a Studio chart-builder at
  bklit.com/studio with video export.
- **Install:** shadcn TRUSTED registry — namespace `@bklit` is on the shadcn registry index
  (auto-configured on new projects): `npx shadcn@latest add @bklit/<slug>` (e.g. `@bklit/area-chart`).
  Manual `components.json` entry if needed: `"@bklit": "https://ui.bklit.com/r/{name}.json"`.
  Loading labels ship via `@bklit/shimmering-text` (auto-included with line/area/heatmap).
- **Best tiers:** 1–4 (any site with data: dashboards, finance, fitness stats, "results" sections).
- **License:** open source (Vercel OSS program).
- **Registry-restraint carve-out:** like Watermelon, Bklit is FUNCTIONAL, not decorative — it may
  coexist with the site's one effect registry. When a site needs charts, prefer Bklit over
  Watermelon's Recharts wrappers; Watermelon keeps forms/tables/blocks.
