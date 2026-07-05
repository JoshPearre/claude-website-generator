# Higgsfield MCP — AI image & video generation

This skill can generate site visuals — hero/section imagery and **scroll-driven video** — through the **[Higgsfield](https://higgsfield.ai/mcp) MCP** when it's configured. SKILL.md's Step 5 (assets) and the Motion section point here for setup, the tool contract, the scroll-video pipeline, and the credit/fallback rules.

## What it is

Higgsfield's official MCP is a **hosted** server exposing 30+ image and video models through one endpoint — image models (Soul, Flux, Seedream, Nano Banana, OpenAI image) and video models (**Kling**, **Seedance**, Veo, Sora, Wan, Grok Video). It covers the whole visual pipeline: stills *and* motion. Generation is **asynchronous** — a generate call returns a job, you poll for completion, then download the asset.

## Setup (one-time, by the user)

The official MCP is **keyless** — no API key to manage. You connect by URL and authenticate through your Higgsfield **account**, and generations spend your Higgsfield **credits**.

```bash
claude mcp add --transport http --scope user higgsfield https://mcp.higgsfield.ai/mcp
```

Then authenticate in the browser when prompted (Claude web/desktop: Settings → Connectors → Add custom connector → URL `https://mcp.higgsfield.ai/mcp` → Connect). Verify with `claude mcp list` → `higgsfield … √ Connected`. There is **no `.env` entry** and nothing secret to commit — auth is account/session-based.

> A community, API-key variant also exists (`jfikrat/higgsfield-mcp`). This skill targets the **official keyless** server above; if a user runs the key-based one instead, the tool names below still apply — only the auth differs.

## Detection & fallback contract

Tools surface as `mcp__<server>__<tool>` (e.g. `mcp__higgsfield__higgsfield_generate_image`), where `<server>` is the name the user registered (`higgsfield` above).

- **If the Higgsfield tools are present** → use Higgsfield as the **primary visual source** per the rules below.
- **If absent** → fall back silently to the Step 5 chain the skill already ships (keyed image API → Neurascapes → Canva → placeholder). Never block generation; note once in the run report that Higgsfield wasn't available.

## The tools (async job model)

| Tool (`mcp__<server>__…`) | Does | Notes |
|---|---|---|
| `higgsfield_generate_image` | text/ref-image → still image | pick an image model; returns a **job** |
| `higgsfield_generate_video` | text/image → video (incl. **first-last-frame** & image-to-video) | pick a video model; returns a **job** |
| `higgsfield_check_cost` | estimate credit cost of a generation | call **before** video gen to respect the budget |
| `higgsfield_wait_for_job` / `higgsfield_get_job` | poll/await a job to completion | gives the asset URL when done |
| `higgsfield_list_jobs` / `higgsfield_cancel_job` | manage in-flight jobs | cancel runaway/duplicate jobs |

Exact tool names may vary slightly by server version — discover them at runtime and match on the `higgsfield_*` / `*generate_image* / *generate_video*` shape rather than hardcoding.

## How it threads through the workflow

### Step 5 — imagery (primary source when present)

When Higgsfield is connected, it sits **at the top** of the Step 5 fallback chain (ahead of Pexels/Unsplash → Neurascapes → Canva → placeholder):

1. Build the prompt from the **Niche Design Brief's** style descriptor (subject · lighting · mood · composition · photo/illustration/3D) + the brief's image query seeds — the same single descriptor used for cohesion across the set.
2. `higgsfield_generate_image` per slot (hero, section photos, og:image, textures), then poll, then **download the returned bytes into `public/`** (download — don't hot-link a generation URL).
3. Anything Higgsfield can't serve falls through to the existing chain. Apply the brief's one style descriptor across all sources so generated and stock assets sit together.

### Motion — scroll-driven video (opt-in, Tier 4-5 only)

This is the "keyframes → fill the in-between" pipeline. **Opt-in per site** — it spends real credits and adds a video asset per section, so reserve it for Tier 4-5 set-pieces, not every section:

1. **Keyframes.** Generate a **start frame** and an **end frame** with `higgsfield_generate_image` (or use a hero still you already generated as the start frame). Keep both on the locked palette + style descriptor so the clip stays on-brand.
2. **Tween to video.** Feed both frames to `higgsfield_generate_video` using a **first-last-frame / image-to-video** model (e.g. **Kling Omni FLF**, **Seedance**, Image2Video) — Higgsfield generates the motion between them. That *is* the "fill in the filler frames" step; you don't render frames yourself.
3. **Cost-gate it.** Call `higgsfield_check_cost` first and respect the user's credit budget; generate sequentially, not a fan-out of dozens of clips.
4. **Wire it for scroll.** Download the clip into `public/`, drop it in a `<video muted playsinline preload="auto">`, and **scrub `currentTime` from scroll position** (Framer Motion `useScroll` → map progress to `video.currentTime`), pinning the section while it plays. Respect `prefers-reduced-motion` (show a static frame instead). This is a Tier 4-5 scroll set-piece — the restraint guardrail still applies (one hero set-piece, not a reel).

### Logos & designed graphics

Higgsfield is for **photographic/generated imagery and video**. Brand **logos** still come from the Magic MCP's `logo_search` (SVG/JSX/TSX) per `21st-dev-mcp.md`; designed/branded compositions (og:image with text/layout) still go to Canva. Use the right tool per asset type.

## Credit & quality notes

- **Credits are real money.** Image gen is cheap-ish; **video is not** — every scroll clip is a paid generation. Default to images; gate video on `higgsfield_check_cost` + explicit opt-in, and tell the user the estimated spend before generating a batch.
- **Async, so don't block.** Kick the job, poll, then proceed; cancel duplicates with `higgsfield_cancel_job`.
- **Tokens still win.** Generated visuals are art-directed by the Niche Design Brief's single style descriptor and the locked palette — they don't get to introduce an off-brand look.
- **Download, don't hot-link.** Generation URLs are ephemeral; the only artifact crossing into the shipped site is the downloaded file in `public/`.
- **Security.** The official MCP is account/session-authed — there's no key to leak, so nothing Higgsfield-related is ever committed.

---

## Model-routing cheat table (added 2026-07-05 — pick by JOB, not by habit)

One consolidated routing view so every generation session routes media the same way. First
match wins; every row still obeys the cost gate (`higgsfield_check_cost` before video spend,
one generated set-piece per site).

| Job | First choice | Fallbacks (in order) |
|---|---|---|
| Keyframe stills · text-on-image · og:image | **GPT Image 2** (via Higgsfield) | Gemini API (Nano Banana / Imagen, `GEMINI_API_KEY`) → Canva MCP |
| End frame that must keep the product pixel-identical to the start | **Nano Banana Pro** (Higgsfield, reference mode — start frame as reference) | Gemini API Nano Banana with the start frame as image input |
| Scroll-scrub transition clip (B3 / B10 — needs first+last frame) | **Kling FLF** (cheap) / **Seedance** (best) via Higgsfield | **Veo 3.1 via Gemini API** (last-frame control) → manual Flow MP4 (free with a Gemini Pro sub — drop in as `public/scroll-source.mp4`) → static start-frame hero |
| Cinematic ambient hero loop / b-roll (no exact end frame needed) | **Veo** (Higgsfield roster or Gemini API) | Seedance → static hero |
| Photographic stills at volume (sections, galleries) | **Higgsfield gen** (when MCP connected) | Pexels (`PEXELS_API_KEY`) → Neurascapes → Canva → placeholder |
| Real-business demo sites | **`googlePhotos` from lead.json FIRST** | then the stills chain above |
| Logos / brand marks | **`logo_search`** (21st.dev Magic MCP, SVG) | styled placeholder slot |

**Wallet reminder:** Higgsfield = credit balance (check before video). Gemini API = pay-per-use
Google wallet — a consumer Gemini Pro subscription does NOT cover it; the sub's included credits
are only usable manually in the Gemini app / Flow (which is the zero-marginal-cost path).
