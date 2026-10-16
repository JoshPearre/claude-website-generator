# Project status

Updated 2026-10-15 for the GX10 Linux handoff. Evidence baseline: `master` at `98cfbbd`.

## Purpose
Reusable Claude Code skill for generating Vite/React websites from a tiered design-prompt library, with optional media generation and demo deployment.

## Current progress
`master` contains the tier library, demo workflow, and July cinematic Tier C additions. The older MCP-integration branch has no unique commits beyond master.

## Important decisions
Keep Tier C explicitly selected and cost-confirmed. Preserve brand/token rules, fallback paths when optional MCP tools are unavailable, and noindex for speculative demos.

## Unresolved problems
README registry counts and provider/model details may lag the library. Optional skills, provider authentication, and a representative generation have not been checked on Linux.

## Next steps
Clone master; review `SKILL.md` and `references/` before changing the workflow. Verify one small generated site locally before enabling paid generation or deployment.

## Linux start and verification
This is a skill repository, not an app to start with npm. Clone it to a normal development directory; link/install the skill into each assistant's supported skill location. Keep provider credentials in local environment/configuration.

This consolidation reviewed saved files and Git history. No application build, tests, live services, or GX10 runtime were run; earlier verification claims belong to the referenced project documents.
