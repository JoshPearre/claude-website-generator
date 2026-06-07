# website-generator

A [Claude Code](https://claude.ai/code) skill that generates polished, multi-file Vite + React websites by composing UI prompts from a design-intensity-tiered prompt library.

Tell Claude "build me a website" and it asks two questions (niche + context), picks a design tier (1 minimal → 5 experimental), samples prompts from the library, runs a quality pass, and outputs a real Vite + React + Tailwind + Framer Motion project.

---

## Installing

Skills live in your global Claude skills directory. Drop this folder there:

**macOS / Linux**
```sh
git clone https://github.com/JoshPearre/claude-website-generator \
  ~/.claude/skills/website-generator
```

**Windows (PowerShell)**
```powershell
git clone https://github.com/JoshPearre/claude-website-generator `
  "$env:USERPROFILE\.claude\skills\website-generator"
```

Claude Code picks up skills from `~/.claude/skills/` automatically — no config needed.

---

## What you get

### Tier system (1–5)

| Tier | Style | Example niches |
|------|-------|----------------|
| 1 | Minimal / operational | Internal tools, docs, landing pages |
| 2 | Clean utility | Local businesses, portfolios, simple SaaS |
| 3 | Modern consumer | Startups, apps, agencies |
| 4 | Expressive brand | High-end studios, luxury products |
| 5 | Experimental / maximalist | Creative agencies, art portfolios |

The tier is auto-selected from `tiers/tier-niche-map.md` based on your niche, or you can override it.

### Design directions (5 fallbacks)

When no brand or reference is provided, Claude picks a deterministic palette + font + posture from `directions/design-directions.md`:

- **Modern Minimal** — Tier 1–2
- **Bold Tech** — Tier 3
- **Warm Editorial** — Tier 2–3
- **Premium Luxe** — Tier 4
- **Experimental Data** — Tier 5

### Component registries (4 shadcn registries)

One registry per site, selected based on tier and style:

| Registry | Focus | Tier |
|----------|-------|------|
| [Magic UI](https://magicui.design) | Animated text, globes, particles, shimmer | 3–5 |
| [Cult UI](https://cult-ui.com) | Shader heroes, iOS widgets, liquid-metal | 3–5 |
| [Skiper UI](https://skiper-ui.com) | Apple-style scroll storytelling | 4–5 |
| [Watermelon UI](https://watermelon-ui.com) | Dashboards, charts, forms | 1–4 |

Full catalogs and install contracts in [`references/component-registries.md`](references/component-registries.md).

### Quality gates

Before writing any files, the skill runs a **Quality Trio**:
- `impeccable teach` → `ui-ux-pro-max` → `design-taste-frontend`

After files exist, it runs a **polish pass**:
- `impeccable critique` → `impeccable polish` (repeats until clean)
- `emil-design-eng` for Tier 3+ (motion timing, micro-interactions)

---

## Setup

### Required

| What | How |
|------|-----|
| Claude Code | Install from [claude.ai/code](https://claude.ai/code) |

### Optional (unlocks more features)

| What | How |
|------|-----|
| Firecrawl key (harvest) | Copy `.env.example` → `.env`, add key from [firecrawl.dev](https://firecrawl.dev) (free tier available) |
| Canva MCP | One-time browser login — generates designed image assets |
| 21st.dev Magic MCP | Component generator — free key at [21st.dev/magic/console](https://21st.dev/magic/console), configured in Claude Code MCP settings |

Images default to [Neurascapes](https://neurascapes.com) (free royalty-free AI photos, no setup).

---

## Companion skills

The skill references 16 companion skills via `skills-lock.json`. Install them with:

```sh
# Install companion skills (run from Claude Code terminal)
npx skills install  # if you have a skills CLI
```

Or install manually — all sources are listed in [`skills-lock.json`](skills-lock.json):

| Source | Skills installed |
|--------|-----------------|
| [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | `impeccable` |
| [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) | `design-taste-frontend`, `ui-ux-pro-max`, `brandkit`, `high-end-visual-design`, and more |
| [emilkowalski/skill](https://github.com/emilkowalski/skill) | `emil-design-eng` |
| [magicuidesign/magicui](https://github.com/magicuidesign/magicui) | `magic-ui` |

The skill degrades gracefully if companions are missing — it will skip quality passes it can't run and note what's missing.

---

## Populating the prompt library

The `prompts/` directory ships with the prompts pre-written. To refresh or expand it from live design sites (Framer Gallery, 21st.dev, Vibe Code Components, etc.), run the harvest workflow:

```
See harvest/HARVEST_GUIDE.md
```

Requires a Firecrawl API key.

---

## Directory structure

```
website-generator/
├── SKILL.md                      # Main skill — workflow, tier logic, all steps
├── README.md                     # This file
├── skills-lock.json              # Companion skills manifest (16 skills, pinned)
├── .env.example                  # FIRECRAWL_API_KEY template
├── prompts/                      # Prompt library (10 UI categories × 5 tiers)
│   ├── _index.md
│   ├── hero-sections.md
│   ├── 3d-components.md
│   ├── scroll-effects.md
│   ├── animations.md
│   ├── interactive-elements.md
│   ├── cards-and-grids.md
│   ├── forms-and-inputs.md
│   ├── navigation.md
│   ├── backgrounds.md
│   └── footers.md
├── tiers/
│   └── tier-niche-map.md         # Niche → default tier lookup
├── directions/
│   └── design-directions.md      # 5 palette+font+posture anchors
├── references/
│   └── component-registries.md   # shadcn registry catalogs + install contracts
├── sources/
│   └── _sources.md               # Harvest source tracking
├── harvest/                      # Workflow for populating the prompt library
│   ├── HARVEST_GUIDE.md
│   └── firecrawl-workflow.md
└── examples/
    └── _examples.md
```

---

## License

MIT
