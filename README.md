# website-generator

A [Claude Code](https://claude.ai/code) skill that generates polished, multi-file Vite + React websites by composing UI prompts from a design-intensity-tiered prompt library.

Tell Claude "build me a website" and it asks two questions (niche + context), picks a design tier (1 minimal → 5 experimental), samples prompts from the library, runs a quality pass, outputs a real Vite + React + Tailwind + Framer Motion project — then deploys it to Vercel and captures outreach-ready demo assets (screenshots + a scrolling GIF).

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

- **Modern minimal** (Linear/Vercel) — Tier 2–3
- **Tech utility** (Datadog/GitHub) — Tier 1–2
- **Human approachable** (Airbnb/Duolingo) — Tier 2–4
- **Editorial** (Monocle/FT) — Tier 3–4
- **Brutalist experimental** (Are.na/Yale) — Tier 4–5

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

### Deploy & demo assets (Step 7)

Every run ends with a **live URL and outreach-ready visuals**. The skill deploys via the Vercel CLI (`vercel deploy --yes`; Vercel MCP as fallback, local preview as the floor), then captures into `demo-assets/` inside the project:

- a full-page **desktop screenshot** (1440px) and a **phone-frame shot** (390px)
- a **3–4s scrolling GIF** — first frame is the hero (Outlook shows only frame 1), compressed under ~200KB for email embedding (ffmpeg palette trick)
- **Lighthouse scores** on full-pipeline runs

**Demo deploys** (speculative outreach) always ship `noindex` (meta + `X-Robots-Tag`); client builds skip it. The run report always ends with the live URL and asset paths. Details: [`references/deploy-demo-assets.md`](references/deploy-demo-assets.md).

### Demo Mode (outreach fast path)

Say **"demo mode"**, pass **`--demo`**, or hand the skill a **`lead.json`** (the shared AgenticOS lead contract: business name, niche, city/state, NAP, Google photos, rating/reviews, booking link) and it skips the intake questions and builds a deployed, noindexed demo for that business in **under 15 minutes — batchable**. The speed trades: cached niche briefs (`briefs/`, seeded for restaurant/salon/contractor/dentist/gym), fixed Tier 3, no 3D scroll hero, single polish pass, best-effort quality skills, no Lighthouse. The full pipeline stays the default — Demo Mode is purely additive.

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
| Image API (Pexels/Unsplash) | Copy `.env.example` → `.env`, add `PEXELS_API_KEY` ([pexels.com/api](https://www.pexels.com/api/), free) or `UNSPLASH_ACCESS_KEY` — sources niche-tailored photos at generation time |
| Canva MCP | One-time browser login — generates designed image assets |
| Vercel CLI (deploy) | `npm i -g vercel` + one-time `vercel login` — Step 7 deploys every finished site and captures its URL. Optional `VERCEL_TOKEN` for headless runs |
| Playwright MCP (demo assets) | Step 7 screenshots + scrolling GIF; falls back to a tiny Node Playwright script |
| 21st.dev Magic MCP | **Live** 21st.dev components (search + generate code + logos + refine) — supersedes the frozen 21st.dev snapshot when configured. Key + setup below. |
| Higgsfield MCP | AI **image + video** generation (Kling/Seedance/Veo/Sora) — primary Step 5 imagery + Tier 4-5 scroll video. Keyless (account/credit auth). Setup below. |

Images default to [Neurascapes](https://neurascapes.com) (free royalty-free AI photos, no setup); add a Pexels/Unsplash key above for sharper, niche-tailored stock photos.

---

## Niche design intelligence

Before picking a tier, the skill **studies how the niche LOOKS** — leading brand sites, industry-filtered galleries (Land-book, SiteInspire, Awwwards, Mobbin), and real competitors — using built-in web tools (`WebSearch`/`WebFetch`, the `firecrawl-*` skills, `research`/`deep-research`). **No API key required.** It researches *visual/design conventions only*; real copy comes later from the client, so the site ships niche-appropriate placeholder copy marked `[PLACEHOLDER — client to replace]`. The output is a `NICHE-BRIEF.md` in the generated project root that drives tier, token-lock, prompt sampling, imagery, and motion.

## Image API (optional — sharper, niche-tailored photos)

Step 5 can pull real stock photos tailored to the niche brief. Provide **one** key (generation-time only):

| Provider | Key | Notes |
|----------|-----|-------|
| Pexels (default) | `PEXELS_API_KEY` | Free, no attribution required — [pexels.com/api](https://www.pexels.com/api/) |
| Unsplash (alt) | `UNSPLASH_ACCESS_KEY` | Requires attribution + download-trigger — [unsplash.com/developers](https://unsplash.com/developers) |

Copy `.env.example` → `.env` and paste your key (or export it in your shell). **The key is read only while the skill runs; images are downloaded into the generated project's `public/`, and the key is never written into the generated site or committed.** `.env` is gitignored — never commit a real key (this repo is public). If no key is set, images fall back to Neurascapes → Canva → placeholder.

---

## 21st.dev Magic MCP (optional — live components)

The skill ships a **frozen snapshot** of 21st.dev (28 prompts baked into `prompts/`). Connect the [**21st.dev Magic MCP**](https://github.com/21st-dev/magic-mcp) (`@21st-dev/magic`) and the skill prefers the **live** server instead — searching the whole current library and generating real component code, where the snapshot only points at URLs. The four tools thread across the workflow: `inspiration` (Step 3 search) · `builder` (Step 4 generate code) · `logo_search` (Step 5 logos) · `refiner` (Step 6 polish). When it isn't configured, the skill falls back to the snapshot + shadcn registries — it never hard-fails.

**Get a key** (free to start, then credit-metered — see below): [21st.dev/magic/console](https://21st.dev/magic/console).

**Register it with Claude Code** (the MCP lives in *your* environment, not this skill folder):

```sh
export MAGIC_API_KEY=...   # paste your 21st.dev key
claude mcp add magic -e API_KEY=$MAGIC_API_KEY -- npx -y @21st-dev/magic@latest
```

Or copy [`mcp.json.example`](mcp.json.example) → `.mcp.json` in the directory you generate sites from (it uses `${MAGIC_API_KEY}` expansion, so it's safe to commit — the key stays in your shell). Full setup, tool contract, and fallback rules: [`references/21st-dev-mcp.md`](references/21st-dev-mcp.md).

> **Cost:** the API key is free to create and there's a free tier to try it, but generation is **credit-metered** (a small monthly free allowance, then paid plans from ~$20/mo) — see [21st.dev pricing](https://help.21st.dev/magic-chat/pricing). It's free to get started, not unlimited-free. The skill works fine without it via the snapshot fallback.

---

## Higgsfield MCP (optional — AI image & video)

Connect the [**Higgsfield MCP**](https://higgsfield.ai/mcp) and the skill generates site visuals directly: it becomes the **primary Step 5 imagery source** (ahead of the Pexels/Unsplash → Neurascapes → Canva chain), and on Tier 4-5 it can build a **scroll-driven video** set-piece — generate a start frame and an end frame, then a first-last-frame video model (Kling Omni FLF / Seedance / Image2Video) fills the motion between them, scrubbed by scroll position. When it isn't connected, Step 5 falls back to the normal chain — no hard fail.

**It's keyless.** The official server authenticates through your Higgsfield **account** (browser auth) and bills your Higgsfield **credits** — there's no API key to set or commit.

```sh
claude mcp add --transport http --scope user higgsfield https://mcp.higgsfield.ai/mcp
```

Then authenticate in the browser when prompted; `claude mcp list` should show `higgsfield … √ Connected`. Full setup, the tool contract, and the scroll-video pipeline: [`references/higgsfield.md`](references/higgsfield.md).

> **Cost:** images are cheap-ish, but **video is real credits per clip** — the skill gates video on a cost check and treats it as opt-in, Tier 4-5 only. It never spends credits on its own without the work calling for it.

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
├── .env.example                  # FIRECRAWL_API_KEY + image-API keys + optional VERCEL_TOKEN
├── mcp.json.example              # Copyable .mcp.json for the 21st.dev Magic MCP
├── briefs/                       # Niche-brief cache (Demo Mode; seeded ×5)
│   ├── _index.md
│   ├── restaurant.md
│   ├── salon.md
│   ├── contractor.md
│   ├── dentist.md
│   └── gym.md
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
│   ├── component-registries.md   # shadcn registry catalogs + install contracts
│   ├── 21st-dev-mcp.md           # 21st.dev Magic MCP setup, tools, fallback contract
│   ├── higgsfield.md             # Higgsfield MCP setup, image+video tools, scroll-video
│   ├── scroll-animation-best-practices.md  # 3D scroll mode: canvas/ffmpeg/preload contract
│   ├── scroll-keyframe-prompts.md          # 3D scroll mode: ready-to-use keyframe prompts
│   └── deploy-demo-assets.md     # Step 7: Vercel deploy, noindex, screenshots + GIF recipes
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
