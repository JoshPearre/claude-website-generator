# 21st.dev Magic MCP — live component integration

This skill can pull 21st.dev components from a **live MCP server** instead of (or on top of) the frozen text snapshot in `prompts/`. SKILL.md's Steps 3–6 point here for the setup, the tool catalog, and the fallback contract.

## Static snapshot vs. live MCP — why the MCP is the better integration

The `prompts/` library carries **28 hand-harvested 21st.dev entries** (see `harvest/staging/21st-dev.md`). Those are a *frozen snapshot*: titles + URLs + a written prompt, captured once, that go stale and only cover what was harvested. They point at a component; they don't deliver its code.

The **Magic MCP** (`@21st-dev/magic`) talks to 21st.dev live. It searches the *whole current* library, returns component JSON + previews, and — uniquely — **generates real React/TypeScript component code on demand**. So when the MCP is configured it is the **preferred** 21st.dev source; the 28 static entries remain the **offline fallback** (and keep feeding the Step 3 source-diversity variety rule). The skill never hard-fails for a missing MCP — it degrades to the snapshot + the shadcn registries, exactly like it degrades when an image key or a companion skill is missing.

## Setup (one-time, by the user)

1. **Get a free API key** — https://21st.dev/magic/console
2. **Register the server with Claude Code.** The MCP is configured in the user's Claude Code environment (or their project), **not** in this skill folder — a skill cloned into `~/.claude/skills/` cannot ship a live server. Pick one:

   **a) CLI (recommended), key from an env var so it never lands in shell history:**
   ```bash
   export MAGIC_API_KEY=...          # paste your 21st.dev key
   claude mcp add magic -e API_KEY=$MAGIC_API_KEY -- npx -y @21st-dev/magic@latest
   ```

   **b) Project-scoped `.mcp.json`** in the directory you generate sites from — copy `mcp.json.example` from this repo to `.mcp.json` there. It uses `${MAGIC_API_KEY}` expansion, so the file is safe to commit (the key stays in your environment):
   ```json
   {
     "mcpServers": {
       "magic": {
         "command": "npx",
         "args": ["-y", "@21st-dev/magic@latest"],
         "env": { "API_KEY": "${MAGIC_API_KEY}" }
       }
     }
   }
   ```

> The MCP server reads its key from the **`API_KEY`** env var. The examples above map your shell's `MAGIC_API_KEY` into it so the literal key is never written to a committed file. **This repo is public — never commit a real key.**

## Detection & fallback contract (how the skill decides)

The tools surface in Claude Code as **`mcp__<server>__<tool>`**, where `<server>` is the name you chose when registering (`magic` above → `mcp__magic__21st_magic_component_builder`).

- **If those tools are present** → the Magic MCP is configured. Prefer it for 21st.dev sourcing as described below.
- **If they are absent** → the MCP isn't configured. Fall back silently to the **static 21st.dev prompts** in `prompts/` plus the **shadcn registries** in `references/component-registries.md`. Do not block generation, and do not nag the user for a key — note in the run report that the live MCP wasn't available.

## The four tools

| Tool (`mcp__<server>__…`) | Input | Returns | Step |
|---|---|---|---|
| `21st_magic_component_inspiration` | a search/description string | JSON for matching 21st.dev components + preview snippets | **3 — sample** |
| `21st_magic_component_builder` | a natural-language component description (the `/ui` flow) | a complete, ready-to-paste React/TS component | **4 — compose** |
| `21st_magic_component_refiner` | an existing component + what to improve | a redesigned component + apply instructions | **6 — polish** |
| `logo_search` | a brand/company name + format (`SVG`/`JSX`/`TSX`) | brand logo source (via SVGL) | **5 — assets** |

## How it threads through the workflow

- **Step 3 (sample).** When the MCP is present, run `21st_magic_component_inspiration` with the Niche Design Brief's vocabulary (native components + motion character + style descriptor) to pull *live* 21st.dev candidates for the categories you picked, in place of the frozen 21st.dev entries. The **variety rule still counts 21st.dev as one source** — span ≥3 source libraries across the full set as usual; don't let the MCP turn every section into a 21st.dev component.
- **Step 4 (compose).** Treat `21st_magic_component_builder` as a fifth, more powerful component source alongside the shadcn registries (Magic UI · Cult UI · Skiper UI · Watermelon). Where a registry hands you a fixed component, the builder **generates bespoke code** to a description — reach for it when no registry piece fits the sampled prompt, or when you want the 21st.dev implementation of a sampled idea rather than just its URL. Builder output is **not exempt** from the rest of the pipeline: re-bind the locked palette/font tokens (Step 3.5) onto it, and it still passes through the Step 6 polish pass like any hand-written component.
- **Step 5 (assets).** Use `logo_search` for brand/client/partner logos (the "as seen in" / footer / nav logo slots) — SVG/JSX/TSX, not raster. It complements the photographic chain (image API → Neurascapes → Canva → placeholder); it does **not** replace it. If the MCP is absent, fall back to a styled placeholder logo slot as usual.
- **Step 6 (polish).** After the `impeccable` critique/polish loop, optionally pass the hero / primary CTA / key cards through `21st_magic_component_refiner` for a 21st.dev-grade redesign — then re-run `impeccable critique` so the refined output still clears the anti-pattern gate. Keep the restraint guardrail: refine the few components worth getting right, don't re-skin the whole site.

## Restraint & quality notes

- **One flavor per site still holds.** The Magic MCP doesn't lift the "pick one registry flavor, don't build a demo reel" rule — a builder-generated hero + a Cult liquid-metal hero + a Magic UI globe in one viewport is still a demo reel. Pick the source whose flavor matches the locked direction.
- **Tokens win.** Anything the builder/refiner returns is re-bound to the design direction's `:root` tokens; generated code never gets to introduce its own palette or font.
- **Trust model = same as the registries.** Builder/refiner output is code you own outright once pasted — skim an unfamiliar component the first time, exactly as with a shadcn `add` (see `component-registries.md`).
