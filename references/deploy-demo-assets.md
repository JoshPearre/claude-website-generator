# Deploy & Demo Assets — Step 7 contract

> **Canonical implementation guide for SKILL.md → "Step 7 — Deploy & demo assets".**
> Covers the noindex snippets, the Vercel deploy paths, and the exact screenshot /
> scrolling-GIF capture + compression commands. Same tooling philosophy as the 3D
> scroll mode: MCP tool first, local CLI/script fallback, never hard-fail — a skipped
> stage is a run-report line, not a stopped run.

---

## 1. Noindex — demo deploys only

A demo deploy is a stranger's business on your URL; it must never end up in Google. Client builds skip this whole section.

**Meta tag** — into the generated `index.html` `<head>`:

```html
<meta name="robots" content="noindex, nofollow" />
```

**`X-Robots-Tag` header** — write `vercel.json` at the project root (covers every route *and* asset, and survives client-side routing):

```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [{ "key": "X-Robots-Tag", "value": "noindex, nofollow" }]
    }
  ]
}
```

**robots.txt** — keep the Step 4 baseline file *allowing* crawls; drop only its `Sitemap:` line. Do **not** add `Disallow: /` on a demo: a crawler blocked by robots.txt never fetches the page, so it never sees the noindex — the meta + header are the mechanism.

(Vercel *preview* deployments also get an automatic platform-level `X-Robots-Tag: noindex` — treat that as belt-and-suspenders, not a reason to skip the explicit tags: the demo must stay noindexed even if someone promotes the deployment or moves hosts.)

**Optional password gate** (off by default — a demo the prospect can't open converts nobody):

- **Vercel Deployment Protection** — dashboard toggle (Project → Settings → Deployment Protection); plan-dependent.
- Or a tiny client-side gate component (a password from the run report + a `sessionStorage` flag). Soft privacy only — it hides the page from casual visitors, it is not security.

## 2. Deploy — Vercel CLI → Vercel MCP → local preview

First match wins. **Always capture the resulting URL** — the run report ends with it.

1. **Vercel CLI (preferred).** `vercel --version` to check it exists (`npm i -g vercel` if the user wants it — **ask before installing**, same rule as ffmpeg). From the **project folder**:
   - Demo deploys: `vercel deploy --yes` → a preview URL (`https://<project>-<hash>-<scope>.vercel.app`). Exactly right for outreach.
   - Client builds, go-for-launch: `vercel --prod --yes`.
   - `--yes` accepts project defaults non-interactively. Auth is a one-time `vercel login` (browser); for headless/CI runs pass the token per shell — POSIX: `--token "$VERCEL_TOKEN"` · PowerShell: `--token $env:VERCEL_TOKEN` (see the skill's `.env.example` — generation-time only, never written into the project).
   - The deployment URL prints on stdout (last line) — capture it.
2. **Vercel MCP (fallback).** If the CLI is absent but the tools surface (`mcp__<server>__deploy_to_vercel` or similar), deploy through the tool and capture the URL from its result. `get_deployment` / `get_deployment_build_logs` debug a failed build.
3. **Local preview (never-fail floor).** Start `npm run preview` **in the background** — it's a long-running server, so never wait on it in the foreground; read the actual URL/port from its startup output (default `http://localhost:4173`, but Vite bumps the port if it's taken). Capture the Section 3–5 assets against it, stop the server, and note in the run report: deploy skipped + why (no CLI, no MCP, or auth declined).

## 3. Screenshots — Playwright MCP path (preferred)

Tools surface as `mcp__<server>__browser_*` (the Playwright MCP). Sequence:

1. `browser_navigate` → the live URL (or local preview).
2. Wait ~1s (`browser_wait_for`) so hero entrance animations settle — a half-faded hero ruins every asset downstream.
3. `browser_resize` → **1440×900**, then `browser_take_screenshot` with `fullPage: true` → save `demo-assets/desktop-full.png`.
4. `browser_resize` → **390×844**, wait for reflow, `browser_take_screenshot` (viewport) → `demo-assets/phone-390.png`.
5. **Scroll frames for the GIF** — back at 1440×900: ~30 steps from top to bottom, screenshot each into `demo-assets/scroll-frames/frame-001.png …`. Step with **instant** scrolling so CSS `scroll-behavior: smooth` can't blur the positions:

```js
// evaluate per step i of FRAMES (i = 0 … FRAMES-1)
const total = document.documentElement.scrollHeight - innerHeight;
window.scrollTo({ top: Math.round(total * (i / (FRAMES - 1))), behavior: "instant" });
```

Frame `001` is scroll position 0 — the hero money-shot (Section 5). If ~30 round-trips through the MCP is too slow for the session, capture the two stills via MCP and run the Section 4 script just for the frames.

## 4. Screenshots — Node Playwright script (fallback; one run captures everything)

For when the Playwright MCP isn't configured. One-time setup inside the generated project (the browser download is ~130MB — **ask before installing**, same rule as ffmpeg):

```sh
npm i -D playwright && npx playwright install chromium
```

Save as `demo-assets/capture.mjs`, run with `node demo-assets/capture.mjs <url>`:

```js
import { chromium } from "playwright";
import { mkdirSync } from "node:fs";

const url = process.argv[2] ?? "http://localhost:4173";
const FRAMES = 30;
mkdirSync("demo-assets/scroll-frames", { recursive: true });

const browser = await chromium.launch();
const page = await browser.newPage({ viewport: { width: 1440, height: 900 } });
await page.goto(url, { waitUntil: "networkidle" });
await page.waitForTimeout(1200); // hero entrance motion settles

await page.screenshot({ path: "demo-assets/desktop-full.png", fullPage: true });

await page.setViewportSize({ width: 390, height: 844 });
await page.waitForTimeout(600);
await page.screenshot({ path: "demo-assets/phone-390.png" });

await page.setViewportSize({ width: 1440, height: 900 });
await page.waitForTimeout(600);
const total = await page.evaluate(() => document.documentElement.scrollHeight - innerHeight);
for (let i = 0; i < FRAMES; i++) {
  await page.evaluate(
    (y) => window.scrollTo({ top: y, behavior: "instant" }),
    Math.round((total * i) / (FRAMES - 1))
  );
  await page.waitForTimeout(100); // let whileInView reveals fire
  await page.screenshot({ path: `demo-assets/scroll-frames/frame-${String(i + 1).padStart(3, "0")}.png` });
}
await browser.close();
```

## 5. Scrolling GIF — ffmpeg palette trick (target: under ~200KB)

**Why the fuss:** the GIF gets **embedded in outreach email**, and Outlook (plus several other clients) renders **only frame 1** — so frame 1 must be the hero money-shot, and the file must be small enough that email doesn't clip or lag.

**Hard rules:**

1. **Frame 1 = the hero at scroll 0, entrance animations finished.** The capture sequences above guarantee this — don't reorder or trim the head.
2. **3–4 seconds total.** ~30 frames at 8–10fps lands there.
3. **Under ~200KB.** Two-pass palette first, then shrink until it fits.

**Two-pass palette (GIF's 256-color quantization done right):**

```sh
ffmpeg -y -framerate 10 -i demo-assets/scroll-frames/frame-%03d.png \
  -vf "fps=10,scale=480:-1:flags=lanczos,palettegen=max_colors=128" demo-assets/palette.png

ffmpeg -y -framerate 10 -i demo-assets/scroll-frames/frame-%03d.png -i demo-assets/palette.png \
  -lavfi "fps=10,scale=480:-1:flags=lanczos [x]; [x][1:v] paletteuse=dither=bayer:bayer_scale=4" \
  demo-assets/scroll-demo.gif
```

**Size-budget loop** — check the file size; while over ~200KB, apply in order and re-encode:

1. width `scale=480` → `400` → `360`
2. `fps=10` → `8` (keep `-framerate` matching)
3. `palettegen=max_colors=128` → `96` → `64`
4. frame count 30 → 24 — trim from the **end**, never the head

**Animated WebP alternative** (2–3× smaller at equal quality — for web/social embeds, **not** email; email clients want GIF):

```sh
ffmpeg -y -framerate 10 -i demo-assets/scroll-frames/frame-%03d.png \
  -vf "fps=10,scale=480:-1" -loop 0 -quality 70 demo-assets/scroll-demo.webp
```

If ffmpeg is missing, same rule as the 3D scroll mode: **ask before installing system software** (e.g. `winget install Gyan.FFmpeg` on Windows) or point at an existing/portable build — never silently install.

## 6. Checklist — what Step 7 hands the run report

| Asset | Path | Rule |
|---|---|---|
| Live URL | — | preview for demos (+ noindex); `--prod` only for client go-live |
| Desktop screenshot | `demo-assets/desktop-full.png` | 1440px wide, full page, hero settled |
| Phone screenshot | `demo-assets/phone-390.png` | 390×844 viewport, hero |
| Scrolling GIF | `demo-assets/scroll-demo.gif` | 3–4s, frame 1 = hero money-shot, < ~200KB |
| Lighthouse (full pipeline only) | `demo-assets/lighthouse.json` | 4 category scores into the run report |

`demo-assets/scroll-frames/` and `palette.png` are intermediates — fine to keep, fine to delete.
