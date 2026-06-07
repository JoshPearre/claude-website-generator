# Example Sessions

Illustrative runs of the `website-generator` skill, showing the intake → tier → sample → scaffold flow.

## Example 1 — Law firm

```
User:  build me a website
Skill: 1. What's the niche?
       2. What's it about / who's it for?
User:  1. Law firm
       2. A small family-law practice; wants to look trustworthy and established.
Skill: Niche: law firm. Selected design tier: 2 — Restrained Professional.
       Say "tone it down" or "go wild" to shift tiers.
       [samples hero-sections, navigation, cards-and-grids, forms-and-inputs, footers
        — tier-2 prompts spanning 3+ source libraries; generates
        generated-site-20260515-1430/]
```

## Example 2 — Designer portfolio (with a tier shift)

```
User:  generate a site for my portfolio
Skill: 1. What's the niche?
       2. What's it about / who's it for?
User:  1. Personal portfolio
       2. I'm a motion designer showing off showreel work.
Skill: Niche: personal portfolio (designer). Selected design tier: 5 — Experimental/Maximalist.
       Say "tone it down" or "go wild" to shift tiers.
User:  tone it down
Skill: Shifted to tier 4 — Expressive Brand. Resampling.
       [samples hero-sections, 3d-components, scroll-effects, interactive-elements,
        animations, backgrounds — tier-4 prompts; generates the scaffold]
```

## Example 3 — Restaurant (ambiguity resolved by context)

```
User:  make me a website for a restaurant
Skill: 1. What's the niche?
       2. What's it about / who's it for?
User:  1. Restaurant
       2. We need a system for staff to take and track table orders.
Skill: [niche "restaurant" is ambiguous — the context says a staff ordering tool,
        not a marketing site]
       Niche: restaurant (POS / ordering dashboard). Selected design tier: 1 — Utility/Operational.
       Say "tone it down" or "go wild" to shift tiers.
       [samples navigation, cards-and-grids, forms-and-inputs — tier-1 prompts;
        leaner stack, no Framer Motion]
```

These are illustrative — real prompt selection is random within the tier band, subject to the variety rule (≥3 source libraries).
