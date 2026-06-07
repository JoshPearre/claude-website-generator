<!-- Framer Gallery harvest: 16 patterns | status: full -->

## hero-sections

### Oversized Concatenated Display Headline
- **Source:** Framer Gallery
- **URL:** https://www.residence.co/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** typography, hero, editorial, large-type, statement
- **Best for tiers:** 3, 4
- **Prompt:**
  > A hero built entirely from typography: a single enormous sentence set in a tight grotesk (Neue Haas / Inter-style) at a display size of 4-8vw, line-height around 0.95, letter-spacing pulled negative (-0.03em). Word spacing is collapsed so the sentence reads as one dense visual block ("Anewhomeforcreativity"). The headline fills most of the viewport with generous margin on a flat off-white or near-black background — no imagery, no decoration. A small label in 12-14px caps sits above it as an eyebrow. On load, the headline fades up and the baseline shifts ~20px into place over ~600ms with an ease-out curve. The effect reads as a confident editorial statement; the page leads with words, not pictures.

### Stacked Single-Word Display Lines
- **Source:** Framer Gallery
- **URL:** https://milopet.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** typography, hero, kinetic, rhythm, big-type
- **Best for tiers:** 3, 4
- **Prompt:**
  > A section headline broken into three or four single words, each on its own line, set in an ultra-bold condensed display face at huge scale (8-14vw), all caps ("CUIDA / CON / CRITERIO"). Lines are left-aligned and stacked tight (line-height ~0.9) so they form a heavy typographic column. Each word animates in independently as it scrolls into view — a quick mask-reveal where the word slides up from behind a clipping edge, staggered ~80ms per line. Color is high-contrast: dark words on a light field or vice versa, sometimes one accent-colored word. Used as a rhythmic punctuation block between content sections, giving the page a bold magazine cadence.

### Floating Product-UI Screenshot Hero
- **Source:** Framer Gallery
- **URL:** https://obsidianos.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** hero, saas, product-shot, depth, layered
- **Best for tiers:** 3, 4
- **Prompt:**
  > A SaaS hero where a concatenated large headline and a single pill CTA sit centered at top, and below them a cluster of overlapping product-UI screenshots floats with soft depth. The main app screenshot is large and slightly tilted toward the viewer; one or two smaller cropped UI cards (a notification, a chart panel) overlap its corners with a gentle drop shadow and subtle scale offset. On scroll the screenshot cluster rises and parallaxes faster than the text, and the cards drift apart a few pixels. Background is a calm gradient or solid tint. The result feels premium and product-forward — selling the interface itself as the visual.

## animations

### Scroll-Triggered Mask Reveal on Type and Media
- **Source:** Framer Gallery
- **URL:** https://www.framer.com/gallery/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** scroll-reveal, mask, intersection-observer, motion
- **Best for tiers:** 3, 4
- **Prompt:**
  > A uniform reveal language applied site-wide: every heading, paragraph and image enters via a clip-path or overflow-hidden mask. Text rises from behind an invisible baseline (translateY ~24px to 0, opacity 0 to 1) and images wipe in as a clip rectangle expands from bottom to full. Transitions run ~500-700ms on a custom cubic-bezier ease-out, fired once when the element crosses ~85% of viewport height, with sibling elements staggered 60-100ms. Nothing pops in abruptly — the whole page assembles itself in a smooth, choreographed cascade as the user scrolls.

### Word-by-Word Headline Fade Sequence
- **Source:** Framer Gallery
- **URL:** https://definedvc.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** text-animation, stagger, hero, kinetic-type
- **Best for tiers:** 3, 4
- **Prompt:**
  > A large multi-line headline animates one word (or one line) at a time. Each word starts at opacity 0 with a small downward offset and a slight blur; as the section enters the viewport they resolve in reading order with a ~70-90ms stagger, blur clearing to sharp. The effect draws the eye left-to-right across the statement. Pair with a tight display typeface and a flat background so the motion is the only ornament. Best for a primary mission or value-proposition headline.

### Marquee Logo / Client Strip
- **Source:** Framer Gallery
- **URL:** https://definedvc.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** marquee, logos, social-proof, infinite-scroll, motion
- **Best for tiers:** 3, 4
- **Prompt:**
  > A horizontal strip of partner or client logos that scrolls continuously and seamlessly. Logos are monochrome SVGs (single dark or single light fill) at uniform optical size, evenly spaced on a quiet background band. The track translates at a slow constant speed (~30-50px/s) and loops by duplicating the logo set; motion pauses on hover. Often two rows scroll in opposite directions. A short caption above ("30+ partners shaping the frontier") frames it. Reads as understated, always-moving social proof.

## cards-and-grids

### Filterable Animated Portfolio Grid
- **Source:** Framer Gallery
- **URL:** https://definedvc.com/
- **Type:** visual-description
- **Intensity:** 4
- **Tags:** grid, filter, portfolio, layout-animation, tags
- **Best for tiers:** 3, 4
- **Prompt:**
  > A portfolio grid of company/project cards, each card showing a logo lockup, a one-line descriptor, and one or more category tag chips ("Physical AI", "Agent Economy", "Next-gen Compute"). A row of tag filters sits above; selecting one re-flows the grid with an animated layout transition — non-matching cards fade and scale down, remaining cards smoothly slide to new positions over ~400ms (FLIP-style). Cards have a flat bordered or softly tinted background, a subtle hover lift, and the logo gains slight contrast on hover. Asymmetric sizing (some cards span two columns) keeps the grid editorial rather than uniform.

### Two-Up Comparison / Module Cards
- **Source:** Framer Gallery
- **URL:** https://obsidianos.com/
- **Type:** visual-description
- **Intensity:** 2
- **Tags:** cards, feature, saas, badges, two-column
- **Best for tiers:** 2, 3
- **Prompt:**
  > Two large feature cards side by side, each representing a product module. Each card carries a status badge in the corner ("Free", "Coming Soon"), a generous product illustration or UI crop filling the upper half, a heading, a one-sentence description, and a subtle text link with arrow. Cards use rounded corners (16-24px), a faint border or soft tint contrast against the page, and equal height. On hover the whole card lifts a few pixels with a softening shadow and the arrow nudges right. Clean, calm, conversion-oriented SaaS layout.

### Editorial News / Article Card Row
- **Source:** Framer Gallery
- **URL:** https://www.residence.co/
- **Type:** visual-description
- **Intensity:** 2
- **Tags:** cards, news, blog, thumbnail, editorial
- **Best for tiers:** 2, 3
- **Prompt:**
  > A horizontal row of three article cards near the page foot. Each card is a large landscape image with a bold title overlaid or directly beneath, plus a small monochrome arrow/logo glyph. Cards sit edge-to-edge with thin gutters; images have a slight zoom-in on hover while the title stays fixed, and a small "More news" link with an arrow icon closes the row. Typography on titles is heavy and tight, matching the site's display style. Minimal chrome — image and headline do the work.

## navigation

### Minimal Inline Text Nav with Pill CTA
- **Source:** Framer Gallery
- **URL:** https://obsidianos.com/
- **Type:** visual-description
- **Intensity:** 2
- **Tags:** navbar, minimal, sticky, pill-button, header
- **Best for tiers:** 2, 3, 4
- **Prompt:**
  > A slim sticky top bar: wordmark or small logo on the left, three to five plain-text nav links centered or right-aligned in 14-15px medium weight, and a single high-contrast pill button ("Get Started") at the far right. The bar is transparent over the hero and transitions to a solid or lightly blurred backdrop with a hairline bottom border once the user scrolls past ~80px, easing over ~250ms. Link hovers are a quiet opacity or color shift; some links open a dropdown panel. Restrained, modern, and gets out of the way.

### Anchor-Link Scrolling Section Nav
- **Source:** Framer Gallery
- **URL:** https://milopet.com/
- **Type:** visual-description
- **Intensity:** 2
- **Tags:** navbar, anchor-links, smooth-scroll, single-page
- **Best for tiers:** 2, 3
- **Prompt:**
  > A single-page nav whose links are in-page anchors ("How it works", "Coverage", "FAQ", "Contact"). Clicking a link smooth-scrolls to the matching section with an eased motion; the link for the section currently in view gets a subtle active state (underline or fill). A persistent primary action ("Calculate your price") is styled as a filled pill, always visible. Pairs with a long, section-stacked landing page where the nav doubles as a table of contents.

## interactive-elements

### Animated Itemized Cost / Receipt Breakdown
- **Source:** Framer Gallery
- **URL:** https://milopet.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** interactive, data-viz, counter, storytelling, mockup
- **Best for tiers:** 3, 4
- **Prompt:**
  > A faux invoice/receipt rendered as a styled card that builds itself as the user scrolls into it. Line items ("Vet consult 150 EUR", "X-ray 80 EUR", "Surgery 300 EUR") appear one by one with a quick fade-and-rise stagger, then a divider draws and a TOTAL number counts up to its value. A second beat reveals the punchline — the total struck through and replaced by "0 EUR" with a color flip to an accent green. The card has soft rounded styling, mono or tabular figures for alignment, and turns a pricing argument into a small animated narrative.

### Numbered Step Sequence with Large Imagery
- **Source:** Framer Gallery
- **URL:** https://milopet.com/
- **Type:** visual-description
- **Intensity:** 3
- **Tags:** how-it-works, steps, scroll-reveal, process, alternating
- **Best for tiers:** 3, 4
- **Prompt:**
  > A "how it works" section as a vertical sequence of numbered steps. Each step pairs an oversized numeral ("1", "2", "3") and a short bold heading with a large product/photo image. Steps alternate or stack full-width; as each enters the viewport the numeral and text mask-reveal while the image scales up slightly from ~0.94 to 1 and fades in. A connecting visual rhythm (consistent spacing, repeated numeral treatment) carries the eye downward. Closes with a CTA. Communicates a process clearly while keeping motion lively.

## backgrounds

### Flat High-Contrast Monochrome Canvas
- **Source:** Framer Gallery
- **URL:** https://www.residence.co/
- **Type:** visual-description
- **Intensity:** 1
- **Tags:** background, minimal, monochrome, editorial, flat
- **Best for tiers:** 3, 4
- **Prompt:**
  > An entirely flat background — a single near-white (#FAFAF8) or near-black (#0A0A0A) field with no gradients, textures, or imagery behind content. All visual interest comes from oversized typography, generous negative space, and the occasional full-bleed image block that interrupts the void. Section transitions may invert the scheme (light section followed by dark section) for rhythm. The restraint makes type and motion feel deliberate and gallery-like; ideal as the canvas for large-type, editorial Framer-style sites.

### Section Color-Block Inversion Scroll
- **Source:** Framer Gallery
- **URL:** https://milopet.com/
- **Type:** visual-description
- **Intensity:** 2
- **Tags:** background, color-blocks, contrast, sectioning, scroll
- **Best for tiers:** 3, 4
- **Prompt:**
  > The page is divided into full-width horizontal bands, each a different solid color (a light neutral, a brand accent, a deep dark, a soft pastel). Adjacent sections deliberately contrast, so scrolling moves the viewer through distinct color rooms. Type color flips for legibility per band. Sometimes the active section's background color also tints the sticky nav. No gradients — pure flat blocks. Gives a long landing page strong visual segmentation and a confident, playful brand cadence.
