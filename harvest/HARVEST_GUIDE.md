# Harvest Guide

How to populate the `prompts/` library from source sites. Run this when sources are new or the library is empty/stale. The library ships **empty** — harvest is the first thing to do.

## Workflow — per source URL

1. **Try `WebFetch` first.** Cheapest. If it returns real content (not an empty `<title>`-only shell), use it.
2. **If JS-rendered or blocked, use Firecrawl** — see `firecrawl-workflow.md`. `firecrawl-scrape` for a single page; `firecrawl-crawl` for a whole library; `firecrawl-map` to discover URLs first.
3. **Visual-only galleries** (no extractable text/code): ask the user for a screenshot or written description. Store as `Type: visual-description`.
4. **Link-aggregator sites:** use Firecrawl to extract the destination URLs, then scrape each linked site individually.
5. **Rate every extracted prompt** with an `Intensity` (1-5) and a `Best for tiers` list — see the rubric below.
6. **Categorize** each prompt into the correct `prompts/*.md` file using the entry format in that file's header comment.
7. **Update the source** in `sources/_sources.md` with its status (`harvested` / `partial`).

## Intensity rubric

Judge the visual + motion complexity of the component the prompt produces.

| Intensity | Motion | Visual | Examples | Best for tiers |
|---|---|---|---|---|
| 1 | None | Flat, static, system fonts, plain layout | Static navbar, plain text input, basic table | 1 |
| 2 | Hover states, simple fades | Light gradients, subtle shadows | Fade-in card, hover-lift button, simple sticky nav | 1, 2 |
| 3 | Scroll reveals, smooth transitions | Modern gradients, tasteful motion, clear hierarchy | Scroll-reveal feature grid, animated counter, gradient hero | 2, 3, 4 |
| 4 | Custom / scroll-driven animation, parallax | Distinctive typography, bold layout, layered motion | Pinned scroll story, parallax hero, marquee, custom cursor | 4, 5 |
| 5 | Heavy / continuous motion, generative | 3D, WebGL, particles, unconventional layout | WebGL particle hero, Three.js scene, generative background | 5 |

A prompt usually fits 1-3 adjacent tiers. A flat utility navbar → Intensity 1, tiers `[1]`. A WebGL particle hero → Intensity 5, tiers `[5]`. A gradient hero with a scroll reveal → Intensity 3, tiers `[2, 3, 4]`.

## Concrete first-harvest plan

| Source | How |
|---|---|
| Trickle prompt library | `WebFetch` the blog post — it is server-rendered. ~50 prompts in 5 categories (visual style, page functionality, interaction & UX, etc.). |
| VibeUI | `WebFetch` or `firecrawl-scrape` the homepage — ~92 prompts, server-rendered. |
| 21st.dev | `firecrawl-crawl` `21st.dev/community/components` (JS-rendered), or use the Magic MCP if installed. |
| VibeCodeComponents | `firecrawl-scrape` (JS-rendered). First confirm prompts are free, not paywalled — skip on ToS grounds if gated. |
| Framer Gallery | Link-aggregator. Scrape `framer.com/gallery` (JS-rendered), extract the showcased-site URLs, scrape a sample of those live sites, and write `visual-description` prompts from their hero/scroll/animation patterns. Use the gallery's category filters to target tiers; sites skew Tier 3-5. |

## Quality bar

- One prompt = one reusable component idea. Don't store whole-page dumps.
- Keep the `Prompt` body self-contained — it must make sense pasted into a fresh generation.
- Deduplicate: if two sources give near-identical prompts, keep the better-written one and note both sources.
- Always fill `Source` and `URL` — the variety rule (≥3 source libraries per site) and the generated README depend on them.
- Aim for spread across all five intensity levels so every tier has prompts to sample.
