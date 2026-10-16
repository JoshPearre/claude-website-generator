# Project status — claude-website-generator

Updated 2026-10-15. Shared continuation context for Claude Code and Codex.

- Repository: https://github.com/JoshPearre/claude-website-generator
- Handoff branch: `gx10/handoff-2026-10-15`; application baseline: `master` @ `98cfbbdc0913`.
- Read this file, AGENTS.md and existing project conventions. No old chat history is required.

## Purpose

Reusable Claude Code skill that builds Vite/React sites from tiered design prompts and captures demo assets.

## Current progress

- Tiered prompts, niche research, component registry guidance, demo mode and opt-in cinematic Tier C are committed.

## Important decisions

- Cinematic generation is explicit opt-in; paid generation requires the skill's cost confirmation.
- Provider credentials belong in the invoking environment, never generated site code.

## Unresolved problems

- Optional quality skills, MCP integrations and deployment credentials must be installed/configured on the GX10; availability not checked.

## Next steps

- Read SKILL.md and README.md; install the skill and only the optional providers needed for the next build.
- Exercise one local demo before reusing deployment automation.

## GX10 setup

- Clone to the documented Claude skills directory; README lists optional Vercel, Playwright, Firecrawl, image and Higgsfield integrations.

## Verification

- Consolidation checked Git history, source diffs and instruction links. New commits were screened for credentials and excluded content; original checkouts were preserved.
- GX10 application runtime, external accounts, production deployments and model serving have not been verified. Recorded earlier test results are dated evidence.
