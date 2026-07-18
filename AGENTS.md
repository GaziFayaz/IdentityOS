# AGENTS.md — IdentityOS

## What This Workspace Is

IdentityOS is a structured personal knowledge base and operating system for organizing professional knowledge, projects, experience, skills, learning, achievements, and activity history.

AI assistants, portfolio experiences, search, retrieval, reporting, and automation are downstream consumers of this knowledge base. The AI portfolio assistant is an important use case, but it is not the only purpose of the system.

## How To Use These Instructions

- Read this file first for workspace-wide orientation and rules.
- If working inside a child repo or module, read that directory's `AGENTS.md` before editing.
- Keep detailed rules near the module they govern; load linked topic docs only when they are relevant to the task.
- Prefer small, focused documentation updates that preserve a clear single source of truth.

## Universal Rules

- Do not commit secrets, credentials, API keys, tokens, webhook URLs, or private runtime data.
- Do not stage child module contents from the root repo.
- Run git commands from the repository that owns the changed files.
- Preserve privacy boundaries between public-safe docs and private vault data.
- Do not invent personal facts; knowledge about the person must be explicit in the vault.
- Keep raw captures, generated drafts, and operational automation state outside the searchable vault until approved and summarized.

## Repository Map

- Root repo: project-level coordination, architecture, roadmap, and workspace guidance.
- `vault/`: private personal knowledge layer and canonical source of truth.
- `n8n-workflows/`: public-safe workflow planning docs and n8n workflow JSON exports (secrets stripped).
- `backend-sync/`: future service for parsing vault content, chunking, embeddings, vector upserts, sync metadata, and indexing retries.

## Topic Docs

- Repository ownership and git boundaries: `REPOSITORY_BOUNDARIES.md`
- Security, privacy, and public/private data handling: `SECURITY_AND_PRIVACY.md`
- Stable system architecture and data flow: `SYSTEM_ARCHITECTURE.md`
- Current status, phases, and priorities: `ROADMAP.md`
- Vault-specific agent rules: `vault/AGENTS.md`
- Vault human guide and conventions: `vault/VAULT_GUIDE.md`
- n8n workflow agent rules: `n8n-workflows/AGENTS.md`
- n8n workflow map and recovery model: `n8n-workflows/README.md`

## Completion Checks

- Relevant module-specific `AGENTS.md` was read before editing inside a child module.
- New or changed guidance has one clear home and is linked from related docs when useful.
- Links to moved or split documentation still resolve.
- No secrets, credentials, raw private captures, or invented personal facts were introduced.
