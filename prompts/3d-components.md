# 3D Components

WebGL / Three.js / Spline embeds, 3D objects, and immersive depth effects.

<!--
Prompt entry format — copy this block for each harvested prompt:

### [Prompt Title]
- **Source:** [Library name]
- **URL:** [original URL]
- **Type:** [text-prompt | visual-description | code-snippet]
- **Intensity:** [1-5]
- **Tags:** [webgl, three-js, spline, particles, 3d-model, depth, etc.]
- **Best for tiers:** [tier numbers, e.g. 4, 5]
- **Prompt:**
  > [The actual prompt text or visual description]
-->

<!-- harvested 2026-05-15 -->

### Futuristic Dashboard
- **Source:** Trickle
- **URL:** https://trickle.so/blog/trickle-vibe-coding-prompt-library
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, dashboard, holographic, neon, charts
- **Best for tiers:** 4, 5
- **Prompt:**
  > Create a futuristic dashboard using 3D holographic charts and glowing neon accents.

### Cyberpunk E-commerce
- **Source:** Trickle
- **URL:** https://trickle.so/blog/trickle-vibe-coding-prompt-library
- **Type:** text-prompt
- **Intensity:** 4
- **Tags:** cyberpunk, ecommerce, holographic, neon
- **Best for tiers:** 4, 5
- **Prompt:**
  > Design a cyberpunk e-commerce site with holographic cards and neon-lit cityscape backgrounds.

### Animated 3D Card
- **Source:** 21st.dev
- **URL:** https://21st.dev/community/components/shailendrakumar19999/animated-3d-card/default
- **Type:** code-snippet
- **Intensity:** 4
- **Tags:** 3d, card, tilt, perspective, hover
- **Best for tiers:** 3, 4
- **Prompt:**
  > A card that reacts in 3D to the pointer — tilting on X/Y axes with perspective and parallax-shifted inner layers tracking the cursor. Subtle depth shadow and glare. On hover the card lifts; on leave it springs back flat.

### 3D Orb
- **Source:** 21st.dev
- **URL:** https://21st.dev/community/components/vaib215/3d-orb/default
- **Type:** code-snippet
- **Intensity:** 5
- **Tags:** 3d, orb, webgl, color-shift, hi-fi
- **Best for tiers:** 4, 5
- **Prompt:**
  > A glossy, color-changing 3D orb that floats and slowly rotates, cycling through a gradient palette across its surface. WebGL/shader-driven with soft reflections and ambient glow. A premium hi-fi focal element for hero sections or backgrounds.

### Crystalline Cube
- **Source:** 21st.dev
- **URL:** https://21st.dev/community/components/dhiluxui/crystalline-cube/default
- **Type:** code-snippet
- **Intensity:** 5
- **Tags:** 3d, shader, cube, shadertoy, lighting
- **Best for tiers:** 5
- **Prompt:**
  > A raymarched crystalline cube shader with advanced 3D lighting, refraction, and glassy facets — Shadertoy-style. The cube slowly rotates with shifting internal light caustics. Pure GLSL fragment shader rendered to a canvas. Striking experimental centerpiece.

### Cobe Globe (Interactive)
- **Source:** 21st.dev
- **URL:** https://21st.dev/community/components/shuding/cobe-globe-interactive
- **Type:** code-snippet
- **Intensity:** 4
- **Tags:** 3d, globe, cobe, interactive, webgl
- **Best for tiers:** 4, 5
- **Prompt:**
  > A lightweight WebGL globe (Cobe library) that auto-spins and can be dragged to rotate freely with inertia. Dotted landmass rendering with a configurable accent color and atmospheric glow. Compact, performant, drop-in 3D element for SaaS pages.

<!-- curated 2026-05-15 — hand-authored React Three Fiber / Spline / WebGL patterns -->

### Rotating 3D Product Viewer
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, product, react-three-fiber, gltf, orbit-controls, interactive
- **Best for tiers:** 4, 5
- **Prompt:**
  > Build an interactive 3D product viewer with React Three Fiber. Load a GLTF/GLB model, auto-rotate it slowly, and let the user drag to orbit and scroll to zoom (OrbitControls). Soft studio lighting, a subtle contact shadow, neutral background so the product stays the focus. Pause auto-rotation while the user is interacting; resume on release.

### WebGL Particle Hero
- **Source:** Curated
- **URL:** https://threejs.org
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, particles, webgl, hero, mouse-reactive, generative
- **Best for tiers:** 5
- **Prompt:**
  > Create a full-viewport hero with a WebGL particle field (Three.js / React Three Fiber). Thousands of points drift slowly and push away from the cursor, then ease back. Optionally the particles coalesce into a shape or the headline text. Dark background, a single accent color, headline and CTA layered on top with a soft glow.

### Spline Scene Hero Embed
- **Source:** Curated
- **URL:** https://docs.spline.design
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, spline, hero, embed, interactive
- **Best for tiers:** 4, 5
- **Prompt:**
  > Embed an interactive Spline 3D scene as the hero using `@splinetool/react-spline`. The scene fills the section; headline, subheading, and CTA sit on top in a readable panel. The Spline object responds to cursor movement (look-at / parallax). Include a lightweight loading state and a static poster-image fallback for reduced-motion or slow connections.

### Scroll-Driven 3D Camera Journey
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, scroll, camera, react-three-fiber, storytelling
- **Best for tiers:** 5
- **Prompt:**
  > Build a scroll-driven 3D scene with React Three Fiber where page scroll moves the camera along a path through a 3D space. As the camera passes waypoints, the matching section content fades in beside the relevant 3D object. Map scroll progress to camera position with smooth damping. Respect `prefers-reduced-motion` with a static fallback.

### 3D Tilt Cards
- **Source:** Curated
- **URL:** —
- **Type:** text-prompt
- **Intensity:** 3
- **Tags:** 3d-transform, tilt, cards, hover, parallax, css
- **Best for tiers:** 3, 4
- **Prompt:**
  > Create a grid of cards that tilt in 3D toward the cursor on hover (CSS `perspective` + `rotateX/rotateY`), with a glare highlight that tracks the pointer and inner content lifting on a slight Z-translate for depth. Smooth spring return when the cursor leaves. Pure CSS 3D transforms — no WebGL needed, so it stays light.

### Floating Ambient 3D Objects
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 4
- **Tags:** 3d, ambient, background, floating, low-poly
- **Best for tiers:** 4, 5
- **Prompt:**
  > Add a background layer of soft, low-poly 3D shapes (spheres, torus, rounded cubes) that drift and rotate slowly behind the content with React Three Fiber. Muted, brand-tinted materials and gentle lighting; the shapes parallax slightly with scroll and cursor. Keep them low-contrast so foreground text stays readable.

### Interactive 3D Globe with Connection Arcs
- **Source:** Curated
- **URL:** https://threejs.org
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, globe, arcs, network, data-viz
- **Best for tiers:** 4, 5
- **Prompt:**
  > Build an interactive 3D globe (Three.js) for a "global reach" or network section. The globe auto-rotates, responds to drag, and renders animated arcs between location points to convey connections. Glowing markers at each location, a subtle atmosphere halo, dark backdrop. Pair it with a headline and stats alongside.

### WebGL Image Distortion Gallery
- **Source:** Curated
- **URL:** https://threejs.org
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, shader, webgl, hover, distortion, gallery
- **Best for tiers:** 5
- **Prompt:**
  > Create an image gallery where each image renders on a WebGL plane with a custom shader that ripples and distorts on hover and with mouse velocity, easing back to flat when the cursor leaves. Works for a portfolio or project grid. Provide a plain `<img>` fallback when WebGL is unavailable.

### Interactive 3D Text
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, typography, text, react-three-fiber, interactive
- **Best for tiers:** 5
- **Prompt:**
  > Render a headline as extruded 3D typography with React Three Fiber (drei `Text3D`). The text gently floats and rotates toward the cursor, with a glossy or metallic material and dramatic lighting. Use for a portfolio or event hero. Include a flat 2D text fallback for accessibility and reduced-motion.

### 3D Product Configurator
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, configurator, product, react-three-fiber, materials, ecommerce
- **Best for tiers:** 4, 5
- **Prompt:**
  > Build a 3D product configurator with React Three Fiber: a centered model the user can orbit, plus a control panel to swap colors, materials, or parts in real time. Each change transitions the model's materials smoothly. Studio lighting, contact shadow, and a clear CTA. Ideal for premium DTC product pages.

### Animated Gradient Blob Mesh
- **Source:** Curated
- **URL:** https://threejs.org
- **Type:** text-prompt
- **Intensity:** 4
- **Tags:** 3d, blob, mesh-gradient, shader, background
- **Best for tiers:** 3, 4, 5
- **Prompt:**
  > Create an animated organic "blob" — a 3D sphere with a vertex-displacement shader that morphs slowly, wrapped in a flowing multi-color gradient. Use it as a hero focal point or a section background. Soft, dreamy motion; the blob reacts subtly to the cursor. A lighter alternative to a full 3D scene — one mesh, big visual payoff.

### Scroll-Pinned Rotating 3D Object
- **Source:** Curated
- **URL:** https://r3f.docs.pmnd.rs
- **Type:** text-prompt
- **Intensity:** 5
- **Tags:** 3d, scroll, pinned, react-three-fiber, product-reveal
- **Best for tiers:** 4, 5
- **Prompt:**
  > Pin a section while a central 3D object (product, logo, abstract shape) rotates and transforms in sync with scroll — each scroll step reveals a new angle or feature callout. Built with React Three Fiber + scroll progress. Feature text fades in beside the object at each step; the section unpins and resumes normal scroll after the sequence.
