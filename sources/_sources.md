# Harvest Sources

Tracks every site considered as a prompt-library source, its status, and how to harvest it. The harvest workflow appends here (HARVEST_GUIDE.md step 7).

## Status legend
`harvested` · `pending` · `partial` · `skipped` · `reference`

| Source | URL | Status | Render | Harvest method | Notes |
|---|---|---|---|---|---|
| 21st.dev | https://21st.dev/community/components | harvested + live MCP | Hybrid (JS) | firecrawl-crawl (snapshot), **Magic MCP (preferred, live)** | 28 prompts harvested as the offline fallback; the **Magic MCP** is the preferred live source when configured — search + generate code + logos + refine. Setup/contract in `references/21st-dev-mcp.md`. |
| VibeUI | https://vibeui.online/ | harvested | Server-rendered | firecrawl-scrape / WebFetch | ~92 free copy-paste UI layout prompts. |
| Trickle prompt library | https://trickle.so/blog/trickle-vibe-coding-prompt-library | harvested | Server-rendered | WebFetch / firecrawl-scrape | Blog post; ~50 categorized copy-paste prompts. |
| VibeCodeComponents | https://vibecodecomponents.com/ | harvested | JS-rendered | firecrawl-scrape | 200+ component prompts. Verify free vs. paid before relying on it. |
| Framer Gallery | https://www.framer.com/gallery/ | harvested | JS-rendered | Link-aggregator → scrape gallery, extract showcased-site URLs, analyze each | Public, no login. Visual-only — store as `Type: visual-description`. Framer-built sites skew Tier 3-5 (expressive, animation-heavy); the gallery's category filters (Portfolio, Agency, SaaS, etc.) help target tiers. |
| Ten Fold "$10k website" guide | https://guides.tenfoldmarketing.com/10k-website | reference | Server-rendered | Read for methodology | No prompts; source of the "business / audience / vibe + constrained library" method. |
| Mobbin | https://mobbin.com/discover/sites/latest | skipped | JS (login-walled) | Manual screenshots only | Paid, login-required, visual-only. ToS risk — do not scrape. |
| Spline templates | https://app.spline.design/templates | skipped | JS (login-walled) | — | Login-walled 3D app. No web UI code/prompts. |
| Spline community | https://app.spline.design/community | skipped | JS (login-walled) | — | Login-walled 3D app. No web UI code/prompts. |
| Emergent | https://app.emergent.sh/home | skipped | JS (login-walled) | — | App-builder gallery — preview/clone whole apps. Login-walled; cloned apps aren't discrete UI prompts (wrong granularity for this library). |
| MotionSites | https://motionsites.ai/ | skipped | JS | Manual only | Paid product; hero prompts behind paywall. ToS risk. |
| Cult UI | https://www.cult-ui.com | reference | JS (SSR install docs) | shadcn registry — see `references/component-registries.md` | Tactile/shader heroes, metal/neumorph buttons, iOS widgets. Tier 3-5. Open source + paid Pro. Installed per-component, not harvested as prompts. |
| Skiper UI | https://skiper-ui.com | reference | JS-rendered | shadcn registry — see `references/component-registries.md` | Apple-style scroll storytelling (clip-path carousels, card-stack, scroll-blur text). Tier 3-5. Free + paid Pro. `@skiper-ui/<slug>`. |
| Watermelon UI | https://ui.watermelon.sh | reference | JS (registry on GitHub) | shadcn registry — see `references/component-registries.md` | 260+ components; app/dashboard UI, charts (Recharts), blocks. Tier 1-4. MIT, fully free. GitHub: WatermelonCorp/watermellon-registry. |

## Visual-only sources

For login-walled or visual-only sites (Mobbin, Spline, MotionSites), the only safe path is the user supplying screenshots or written descriptions. Store those as `Type: visual-description` entries in the relevant category file, with the source name and URL recorded.

## Component registries (integrated, not harvested as prompts)

Some sources are **shadcn component registries** rather than prompt libraries — they supply installable React components, not text prompts. They're wired into SKILL.md's "Motion & interactive components" section and catalogued in `references/component-registries.md`, so they carry `Status: reference` here (provenance only) and are **not** run through the harvest. Current registries: **Magic UI** (via its Claude skill), **Cult UI**, **Skiper UI**, **Watermelon UI**.

If you later want their *patterns* as text prompts too (e.g. to feed the Step 3 variety rule), add a `pending` row and run the harvest as normal — the registry integration and a prompt-harvest can coexist.

**21st.dev is also wired as a live MCP**, distinct from the shadcn registries above: the **Magic MCP** (`@21st-dev/magic`) is preferred over the harvested 21st.dev snapshot whenever it's configured, and the snapshot remains the offline fallback. It isn't a shadcn registry and isn't harvested — see `references/21st-dev-mcp.md`.

**Higgsfield is an asset-generation MCP**, not a prompt/component source — so it isn't harvested and has no row above. It's a Step 5 *image + video* source (Kling/Seedance/Veo/Sora): primary imagery when connected, plus the Tier 4-5 scroll-video pipeline, falling back to the keyed image API chain when absent. Keyless (account/credit auth) — see `references/higgsfield.md`.

## Adding a source

Append a row above with `Status: pending`, then run the harvest (see `../harvest/HARVEST_GUIDE.md`). Update the status to `harvested` (or `partial`) once prompts land in `prompts/`.
