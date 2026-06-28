# Scroll-Animation Keyframe Prompts (Nano Banana 2 / GPT Image 2 + Seedance/Kling)

> Prompt templates for the **3D scroll animation** mode (SKILL.md → "3D scroll animation —
> scroll-scrubbed hero"). The flow is: **start still → end still (using the start as a
> reference image) → image-to-video transition**. Then ffmpeg + canvas per
> `scroll-animation-best-practices.md`.

## Golden rules (from the technique, learned the hard way)

1. **Background must match the website section** the animation sits on. The product should look
   like it *floats* — if the still's background differs from the page, the visitor sees the
   image's rectangle/edges. Decide the section background color first, then bake it into every
   keyframe prompt ("…on a clean **<exact bg>** background"). A soft **contact shadow on that
   matching background** is fine — it grounds the product; ask for **no shadow/reflections** only
   when you want the pure "floating in space" look (e.g. the X-ray headphones). Either way the
   background stays one flat color so the rectangle never shows.
2. **Generate the start frame first**, get it right, then **feed it back as the reference image**
   for the end frame. Because the reference does the heavy lifting, the end-frame prompt can be
   short ("X-ray of the same headphones showing internal components, keep the white background, no text").
3. **At least 2K** on the stills.
4. **Keep the transition motion simple** (push-in, explode, X-ray dissolve, assemble-from-nothing).
   Wild camera moves smear when scrubbed. When the video tool offers to "enhance the prompt," decline.
5. **Solid, opaque product** unless the transition is specifically an X-ray/translucency reveal.

## Higgs Field commands (the user's setup)

> **Verify before relying on these.** The model IDs and flags below are representative — run
> `higgsfield generate create --help` (or use the `higgsfield-generate` skill) to confirm the
> current model names, flags, and output/download semantics first.
>
> **Pick ONE aspect ratio** derived from the hero section/canvas and use it for the start still,
> end still, AND the video (`2:3`/`9:16` for a tall product like a can, `16:9` for a wide hero,
> `1:1` only for a square section). Mismatched aspects distort on the canvas.

```bash
AR=2:3   # derive from your hero section; reuse the SAME value for all three calls
# 1) Start frame (still)
higgsfield generate create nano_banana_2 --prompt "<START PROMPT, on <bg> background>" --resolution 2k --aspect_ratio $AR --wait
# 2) End frame (still) — pass the start frame as a reference for consistency
higgsfield generate create nano_banana_2 --prompt "<END PROMPT, keep the <bg> background, no text>" --image <start_frame_id_or_url> --aspect_ratio $AR --wait
# 3) Transition video (start + end frame) — Seedance best, Kling cheaper
higgsfield generate create seedance_2_0 --start-image <start_id> --end-image <end_id> --duration 5 --aspect_ratio $AR --wait
```

Fallbacks if Higgs is unauthenticated: GPT Image 2 (`gpt_image_2`) / `imagegen-frontend-web` for stills; static-hero fallback if no image-to-video is available.

---

## Template — START frame (premium product photography)

```
high-end product photograph of a <PRODUCT> viewed from a <ANGLE>, fully assembled and pristine.
<MATERIAL/FINISH DETAILS>. <SIGNATURE DETAIL>. The <PRODUCT> sits centered on a clean <BG COLOR>
background with soft diffused contact shadows beneath it. Studio lighting with a soft key light
from the upper left and gentle rim lighting along the <SIDE> edge to define the <FORM>. Shot as if
on a 70mm macro lens with shallow depth of field. Ultra-detailed, photorealistic.
```

## Template — END frame (exploded / X-ray / assembled), referencing the start image

```
<same PRODUCT> from the identical <ANGLE>, now shown as <TRANSITION DESTINATION:
exploded view with every layer separated and evenly spaced | an X-ray revealing internal
components | assembled from nothing on an empty stage>. <LAYER/COMPONENT DETAIL>. Each part casts
a soft shadow. Same studio lighting and the same clean <BG COLOR> background. Same 70mm macro
perspective. Ultra-detailed, photorealistic. No text.
```

## Template — TRANSITION video prompt (image-to-video, keep it simple)

```
Continuous transition: the <PRODUCT>'s <outer shell / body> gradually <explodes into its layers /
becomes transparent revealing the inner components / assembles from its parts>, smooth and
controlled, no camera spin. <ENDING STATE>.
```

---

## Worked example A — 75% mechanical keyboard (white background)

**Start:** high-end product photograph of a premium 75% mechanical keyboard viewed from a 3/4
top-down angle, fully assembled and pristine. The keyboard has a machined aluminum case with a
matte black anodized finish and subtle chamfered edges that catch the light. Double-shot PBT
keycaps in a clean white-on-dark colorway with crisp legends. A visible rotary encoder knob in the
top right corner with a knurled metal texture. A coiled aviator USB-C cable extends from the back
in matching black. The keyboard sits centered on a clean white background with soft diffused
contact shadows beneath it. Studio lighting with a soft key light from the upper left and gentle
rim lighting along the right edge to define the aluminum contours. Shot as if on a 70mm macro lens
with shallow depth of field softening the far keys slightly. Ultra-detailed, photorealistic.

**End (reference = start):** high-end product photograph of the same premium 75% mechanical
keyboard from the identical 3/4 top-down angle, now shown as a fully exploded view with every layer
separated and floating in space with even vertical spacing between them. From top to bottom: the
white PBT keycaps float as a complete grid at the top, below them the individual mechanical
switches showing their cross-stem housings, below that the switch plate in brushed steel, below
that the PCB circuit board with visible RGB LEDs and microcontroller chip, below that a silicone
dampening layer, and at the bottom the aluminum case shell with its hollow interior visible. The
coiled aviator cable floats detached near the case. Each layer casts its own soft shadow on the
layer below it. Same studio lighting with soft key light from upper left and rim lighting on the
right. Centered on a clean white background. Same 70mm macro lens perspective with shallow depth of
field. Ultra-detailed, photorealistic.

## Worked example B — over-ear headphone (white background, X-ray transition)

**Start:** Create a product photograph of a premium over-ear headphone, single ear cup shown from a
three-quarter front angle, slightly tilted to show depth. The design is sleek and minimal — matte
black soft-touch housing with brushed aluminum accents on the hinge and outer ring. A plush memory
foam ear cushion in dark charcoal protein leather is visible. The headband curves away gracefully at
the top, wrapped in matching soft-touch material. Every surface is completely solid and opaque. Soft
studio lighting with a gentle key light from the upper left and a subtle rim light defining the edges
of the housing. The headphone floats in completely clean white space — no surface, no shadow, no
reflections, pure flat white background everywhere. The feel is premium consumer electronics — like
a Sony WH-1000XM or Bose 700 product shot.

**End (reference = start):** X-ray image of the headphones showing the component parts in great
detail — drivers, circuit boards, wiring. Keep the all-white background, no text.

## Worked example C — Sensori functional beverage can (cream background)

**Start:** high-end product photograph of a tall slim 12oz beverage can for the brand "SENSORI",
viewed straight-on at a slight 3/4 angle, fully sealed and pristine. Brick-red (#ad362e) aluminum
body with the vertical "SENSORI" wordmark in a cream serif, minimal premium labeling. Cold
condensation micro-droplets on the metal. The can stands centered on a clean cream (#f7f4f1)
background with a soft diffused contact shadow beneath it. Studio lighting, soft key light from
upper left, gentle rim light on the right edge to define the cylinder. 70mm macro lens, shallow
depth of field. Ultra-detailed, photorealistic.

**End (reference = start):** the same SENSORI can from the identical angle, now mid-"ingredient
burst" — the can stays centered and sealed while its botanicals float outward around it in even
radial spacing: blood-orange slices, cucumber rounds, a strawberry, a small red chili, and a few
sparkling-water bubbles, each casting a soft shadow. Same studio lighting, same clean cream
(#f7f4f1) background, same 70mm macro perspective. Ultra-detailed, photorealistic. No text.

**Transition video:** Continuous transition: the SENSORI can stays centered and still while its
botanicals (blood orange, cucumber, strawberry, chili) and sparkling bubbles gently float and fan
outward around it, smooth and controlled, no camera spin.
