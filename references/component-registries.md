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
