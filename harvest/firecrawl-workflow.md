# Firecrawl Workflow

Firecrawl renders JavaScript-heavy pages that plain `WebFetch` can't read. In this environment Firecrawl is available as **CLI-based skills**, not an MCP server — invoke them with the Skill tool.

> Note: an earlier draft of this skill assumed a Firecrawl *MCP* (`firecrawl_scrape` / `firecrawl_crawl`). This environment provides the equivalent **Firecrawl skills** instead — same capability, different entry point.

## Available Firecrawl skills

| Skill | Use for |
|---|---|
| `firecrawl-scrape` | A single page → clean markdown. The default. |
| `firecrawl-crawl` | A whole site / section → many pages at once. |
| `firecrawl-map` | Discover all URLs on a site before scraping. |
| `firecrawl-search` | Web search with full page content. |
| `firecrawl-build-onboarding` | One-time: get a Firecrawl API key into a project `.env`. |

## API key

Firecrawl needs `FIRECRAWL_API_KEY`. A free tier exists — get a key at https://firecrawl.dev.

- Copy `.env.example` to `.env` in this skill folder and fill `FIRECRAWL_API_KEY`.
- `.env` is git-ignored (see `.gitignore`) — never commit it.
- The Firecrawl skills may also manage their own credentials; the `.env` entry is the canonical record for this skill.

## Usage patterns for harvesting

**Single library page (e.g. VibeUI):**
Invoke `firecrawl-scrape` on `https://vibeui.online/` → returns markdown → extract each prompt into the right `prompts/*.md` file.

**Whole component library (e.g. 21st.dev):**
1. `firecrawl-map` on `https://21st.dev/community/components` to list component URLs.
2. `firecrawl-crawl` the section, or `firecrawl-scrape` each URL.
3. Extract component code/prompts; rate intensity; categorize.

**Server-rendered page (e.g. the Trickle blog post):**
Try `WebFetch` first — if it returns real content, Firecrawl isn't needed. Save Firecrawl for JS-heavy pages.

**Link-aggregator / showcase gallery (e.g. Framer Gallery):**
`firecrawl-map` or `firecrawl-scrape` to extract the destination / showcased-site links, then scrape each linked site and write `visual-description` prompts from their patterns.

## Cost discipline

- Use `WebFetch` whenever it works; reach for Firecrawl only for JS-rendered or blocked pages.
- Scrape a few representative pages, not an entire domain, unless a full crawl is clearly needed.
- Harvest is a deliberate batch job — run it intentionally, not on every site generation.

## Alternatives

If Firecrawl is unavailable, `defuddle` (clean web-page extraction) and `nimble-web-expert` (live web access) can substitute for single-page reads.
