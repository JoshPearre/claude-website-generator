# Scroll-Driven Frame Animation — Best Practices

> **Canonical implementation guide for the skill's "3D scroll animation" mode.**
> When that mode is triggered (see SKILL.md → "3D scroll animation — scroll-scrubbed hero"),
> follow this document precisely for the canvas/ffmpeg/preload/scroll implementation.
> This is the same core technique used on Apple's AirPods, MacBook, and iPhone product pages.

---

## The Pipeline

```
Video (MP4) → FFMPEG → Image Sequence (WebP) → Canvas + Scroll Logic
```

The MP4 itself comes from the keyframe→video step (Higgs Field: Nano Banana 2 stills →
Seedance 2.0 / Kling 3.0 transition). This guide covers everything *after* you have the MP4.

---

## 1. Use FFMPEG to Extract Frames from Video

Start with a rendered video (3D animation, screen recording, etc.) and extract it into individual frames:

```bash
ffmpeg -i animation.mp4 -vf "fps=30" frames/frame-%04d.webp
```

- Choose your frame count based on how smooth you need it vs. total file size
- 120–200 frames is a good sweet spot for most scroll sections
- You can reduce further by lowering FPS or trimming the video first

---

## 2. Use WebP, Not JPG

WebP is the right format for this:

- **25–35% smaller** than equivalent-quality JPEG at the same visual fidelity
- Supported in all modern browsers
- Optional alpha channel support if you need transparency
- Optional lossless mode for UI/text-heavy frames

In one project, 160 frames at 1928x1076 totaled only **~19MB** (~119KB per frame). The same frames as JPG would be closer to 25–30MB.

You can control quality during the FFMPEG export:

```bash
ffmpeg -i animation.mp4 -vf "fps=30" -quality 80 frames/frame-%04d.webp
```

---

## 3. Render on a Canvas, Not a Video Element

**Do not use `<video>` for scroll-driven animations.** Videos cannot be scrubbed frame-accurately — `video.currentTime` is unreliable, browsers decode asynchronously, and you'll get flickering or blank frames.

Instead, use a `<canvas>` with pre-loaded `Image` objects:

```js
const canvas = document.getElementById('frame-canvas');
const ctx = canvas.getContext('2d');

// Drawing a frame is essentially a GPU blit — nearly instant
ctx.drawImage(frames[currentFrame], 0, 0, width, height);
```

This gives you **pixel-perfect, instant frame switching** on every scroll position.

**Match the canvas buffer to the frames.** Set `canvas.width/height` to the frame's natural pixel size (read `img.naturalWidth/Height` off the first loaded frame) and let CSS scale the *display* size — drawing native-resolution frames and downscaling in CSS stays crisp on retina/wide screens and never stretches. (If you instead pin a small fixed canvas size, multiply the backing store by `window.devicePixelRatio` and draw with aspect-preserving cover/contain math.)

---

## 4. Preload All Frames Before the Experience Starts

Load every frame upfront behind a loading screen. If frames load mid-scroll, you'll get blank flashes or jumps.

**Batch the loading** — don't fire all requests at once:

```js
const TOTAL = 160;
const BATCH = 20;

for (let i = 0; i < TOTAL; i += BATCH) {
  const batch = [];
  for (let j = i; j < Math.min(i + BATCH, TOTAL); j++) {
    batch.push(loadFrame(j));
  }
  await Promise.all(batch);
  updateLoadingProgress(Math.min(i + BATCH, TOTAL), TOTAL);
}
```

- Browsers limit concurrent connections (~6 per domain), so 160 parallel requests would actually be slower
- Batches of 15–20 keep the pipeline saturated without overwhelming the browser
- Show a percentage/progress bar so users know something is happening
- Only remove the loading overlay once **every** frame is ready

---

## 5. Use `position: sticky` + a Tall Scroll Container

This is the structural trick that makes it all work:

```css
.scroll-container {
  height: 400vh; /* the "scroll runway" — how far the user scrolls */
}

.sticky-wrapper {
  position: sticky;
  top: 0;
  height: 100vh; /* pins the canvas in the viewport */
  overflow: hidden;
}
```

- The tall container (400vh) creates **3 viewport-heights of scroll distance** to map across your frames
- The sticky wrapper **pins the canvas in place** while the user scrolls through
- Scroll progress is simple math, **clamped to `[0,1]`** (it goes negative before the section and `>1` after — clamp or you'll index `frames[-1]`/undefined): `progress = Math.min(Math.max(-rect.top / (rect.height - window.innerHeight), 0), 1)`
- Adjust the container height to control pacing — taller = slower animation, shorter = faster

---

## 6. Separate Scroll Calculation from Rendering

This is critical for performance. **Never draw to the canvas inside the scroll handler.** Scroll events can fire hundreds of times per second on trackpads.

Instead, use two separate systems:

```js
let currentFrame = 0;
let drawnFrame = -1;

// Scroll handler: ONLY calculates which frame to show
window.addEventListener('scroll', () => {
  const progress = getScrollProgress();
  currentFrame = Math.min(Math.floor(progress * TOTAL), TOTAL - 1);
}, { passive: true });

// rAF loop: ONLY draws when the frame actually changed
function tick() {
  if (currentFrame !== drawnFrame) {
    ctx.drawImage(frames[currentFrame], 0, 0, FW, FH);
    drawnFrame = currentFrame;
  }
  requestAnimationFrame(tick);
}
tick();
```

Key details:
- The scroll listener is **`{ passive: true }`** so it never blocks scrolling or causes jank
- The `drawnFrame !== currentFrame` check means you **never redraw the same frame twice**
- `requestAnimationFrame` naturally throttles drawing to the display refresh rate

---

## 7. Tie Content Overlays to Scroll Position, Not Timers

If you have info cards, labels, or callouts that appear during the animation, map them to **scroll ranges**:

```js
const phases = [
  { el: document.getElementById('phase-1'), start: 0.08, end: 0.24 },
  { el: document.getElementById('phase-2'), start: 0.28, end: 0.46 },
  { el: document.getElementById('phase-3'), start: 0.50, end: 0.68 },
  { el: document.getElementById('phase-4'), start: 0.72, end: 0.92 },
];

// Inside scroll handler:
for (const phase of phases) {
  if (progress >= phase.start && progress <= phase.end) {
    phase.el.classList.add('visible');
  } else {
    phase.el.classList.remove('visible');
  }
}
```

- The **user controls the pacing** — content always feels responsive regardless of scroll speed
- Use CSS transitions for fade-in/out so it stays smooth
- Leave small gaps between phase ranges (e.g., 0.24 → 0.28) so cards don't overlap

---

## 8. Polish Details

A few finishing touches that elevate the experience:

**Radial gradient mask** — soft-edge the canvas so frames float into the background instead of having a hard rectangle:

```css
#frame-canvas {
  -webkit-mask-image: radial-gradient(ellipse 65% 60% at 52% 50%, black 40%, transparent 75%);
          mask-image: radial-gradient(ellipse 65% 60% at 52% 50%, black 40%, transparent 75%);
}
```

**Subtle rotation on scroll** — adds a sense of 3D even with a 2D image sequence:

```js
const rotation = -4 + progress * 12; // sweeps -4deg to +8deg
canvas.style.transform = `translate(-50%, -50%) rotate(${rotation}deg)`;
```

**Scroll progress bar** — a thin accent-colored bar at the top of the viewport shows overall page progress.

**Glassmorphic overlay cards** — `backdrop-filter: blur()` on the content cards so the animation shows through, reinforcing the layered feel.

---

## Quick Reference

| Decision | Do This | Not This |
|---|---|---|
| Image format | WebP | JPG/PNG |
| Playback element | `<canvas>` | `<video>` |
| Frame loading | Batched preload, all upfront | Lazy load on scroll |
| Scroll handling | `{ passive: true }`, set state only | Draw inside scroll handler |
| Rendering | Separate `requestAnimationFrame` loop | Draw in scroll event |
| Layout | `position: sticky` + tall container | JavaScript-driven positioning |
| Content timing | Scroll-position ranges | setTimeout / CSS animations |
| Frame count | 120–200 frames | 500+ (too heavy) or 30 (too choppy) |

---

## React/Vite adaptation note

In the skill's default Vite + React stack, implement the above as a `ScrollFrameCanvas` component:
- Put the extracted frames in `public/frames/frame-0001.webp …` and reference them by URL.
- Use a `useEffect` to run the batched preloader (track progress in state for the loading overlay), then start the rAF loop; clean both up on unmount.
- Use Framer Motion's `useScroll` for the progress value **or** the raw `getBoundingClientRect` math above — either is fine; the non-negotiable is keeping the draw in a rAF loop, never in the scroll/`useMotionValueEvent` callback.
- Overlay phase cards as absolutely-positioned siblings whose visibility is derived from the **same clamped scroll-progress value** — do **not** use `whileInView` (it keys off viewport intersection, not frame progress, so it desyncs from the animation).
- Honor `useReducedMotion()` → **skip the frame preload entirely** and render the static **`hero-start.webp`** poster (the reliable asset even if frame extraction was skipped/failed); no scroll runway.
