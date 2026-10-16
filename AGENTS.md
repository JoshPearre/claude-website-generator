# Project instructions


## Shared continuation rules

Read PROJECT_STATUS.md first, then the README and relevant existing project instructions. Treat older handoffs as dated evidence, not proof that an integration or deployment is working today.

GitHub is the shared source of truth. Start with git status -sb, fetch origin, and compare branches before editing. Preserve other contributors' changes; use an isolated branch for conflicting or unfinished work. Never force-push or reset another agent's work.

Use repository-relative Linux paths and committed lockfiles. Keep credentials, private databases, chat/session histories, caches, dependencies, generated logs, and model weights outside Git. Preserve approval gates for paid services, production data changes, messaging, and publishing.

After meaningful work, update PROJECT_STATUS.md briefly with decisions, unresolved problems, next steps, and the exact checks performed. Do not claim runtime success from a source review alone.
