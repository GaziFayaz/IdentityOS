# Security And Privacy

## Purpose

This document defines cross-cutting security and privacy rules for IdentityOS. Module-specific files may add stricter rules, but they should not weaken these rules.

## Universal Rules

- Do not commit API keys, passwords, tokens, webhook URLs, private credentials, or secrets.
- Do not store raw private captures, temporary drafts, or operational runtime state in public-safe repositories.
- Do not invent personal facts. Personal knowledge must come from explicit vault content.
- Keep public examples sanitized and free of private personal data.
- Review workflow exports before committing them. Strip `pinData`, personal test data, and any actual secrets. n8n exports reference credentials by name — never replace those references with actual secret values.

## Data Boundaries

| Data | Correct Location | Notes |
|---|---|---|
| Canonical personal knowledge | `vault/` | Private source of truth |
| Public-safe workflow planning and n8n exports | `n8n-workflows/` | Docs and secrets-stripped workflow JSON |
| Raw manual captures | Outside searchable vault | n8n-controlled staging |
| Raw automatic captures | Outside searchable vault | n8n-controlled staging |
| Daily summary drafts | Outside searchable vault until approved | Draft staging |
| Approved daily notes | `vault/daily-notes/YYYY-MM-DD.md` | Final human-readable notes |
| Approved weekly summaries | `vault/weekly-summaries/YYYY-Www.md` | Preferred recent-activity indexing boundary |
| Vector index and sync metadata | Backend-owned stores | Not stored in the vault |

## Vault Privacy

The vault is private and may contain personal professional history, private context, and non-public details. Public repositories should not duplicate vault content unless it has been intentionally sanitized.

Searchable vault notes may include `visibility` metadata with one of these values:

- `public`
- `internal`
- `private`

Future indexing, retrieval, and publishing systems must respect this metadata.

## Workflow Privacy

Workflow planning documents may describe mechanisms, schedules, recovery models, and data flow. They must not contain real credentials, live webhook URLs, raw captures, generated private drafts, or private operational state.

Raw capture data should remain outside the searchable vault until it is summarized, reviewed, and approved.

## Agent Safety Checklist

Before completing work, check that:

- No secrets or credentials were added.
- No private raw captures or drafts were committed.
- Public-safe files do not reveal private vault details unnecessarily.
- New personal facts are grounded in explicit vault notes or user-provided facts.
- Any new module-specific privacy rules are linked from the relevant module `AGENTS.md`.
