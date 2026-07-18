# Repository Boundaries

## Purpose

This document defines which repository owns each part of IdentityOS and how agents should handle Git operations across root and child modules.

## Root Repository

The root IdentityOS repository owns project-level coordination documents only:

- `README.md`
- `AGENTS.md`
- `REPOSITORY_BOUNDARIES.md`
- `SECURITY_AND_PRIVACY.md`
- `SYSTEM_ARCHITECTURE.md`
- `ROADMAP.md`

The root repo should not directly own private vault data, backend implementation code, or future deployable services unless the repository boundary is intentionally changed.

## Child Modules

Direct child folders are ignored by the root `.gitignore` using `/*/`. This keeps child modules out of the root repository by default.

Current and planned child modules:

| Module | Purpose | Expected Visibility |
|---|---|---|
| `vault/` | Private Markdown knowledge base and source of truth | Private |
| `n8n-workflows/` | Public-safe workflow planning docs and n8n workflow JSON exports | Public-safe |
| `backend-sync/` | Future service for parsing, chunking, embeddings, vector sync, and retry metadata | Public-safe if sanitized |

Each child module should be a separate Git repository when it needs independent privacy, visibility, deployment, or release history.

## Git Rules

- Run Git commands from the repository that owns the files being changed.
- Do not stage child module contents from the root repository.
- Before committing from the root repo, verify that only root-owned files are staged.
- If a child module must be tracked by the root repo later, update root `.gitignore` with an explicit exception before staging it.
- Do not move files across repository boundaries without checking whether privacy, history, or deployment ownership changes.

## Documentation Placement

- Workspace-wide rules belong in root `AGENTS.md` or root topic docs.
- Module-specific agent rules belong in the module's own `AGENTS.md`.
- Human-facing module guides belong beside the module they explain, such as `vault/VAULT_GUIDE.md`.
- Architecture that spans modules belongs in `SYSTEM_ARCHITECTURE.md`.
- Current priorities and phase tracking belong in `ROADMAP.md`.

## Future Modules

When adding a new direct child module:

1. Decide whether it needs its own repository.
2. Add a local `AGENTS.md` if agents will edit files inside it.
3. Document its purpose and visibility in this file.
4. Link the module from root `AGENTS.md` or `README.md` if it becomes part of normal project navigation.
