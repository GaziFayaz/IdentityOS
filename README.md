# IdentityOS

IdentityOS is a structured personal knowledge base and operating system for organizing professional knowledge, projects, experience, skills, learning, achievements, and activity history.

The system is designed to support multiple downstream uses, including AI-assisted retrieval, portfolio experiences, search, reporting, workflow automation, and long-term personal knowledge management.

## Purpose

IdentityOS exists to turn personal professional knowledge into a reliable, structured source of truth that can be maintained by humans and consumed by software systems.

The AI portfolio assistant is an important downstream use case, but it is not the only purpose of the system. The project currently focuses on building a high-quality knowledge foundation before adding the sync, retrieval, API, automation, and user interface layers.

## Core Principles

- **Vault as source of truth**: generated answers must be grounded in retrieved vault documents.
- **Human-readable first**: Markdown notes should be useful to a person before they are useful to automation.
- **Facts over marketing**: the vault stores concrete facts, work history, decisions, outcomes, and lessons learned.
- **Privacy by module**: private personal data stays separate from code and sanitized examples that can be public.
- **Incremental maintenance**: daily notes, weekly summaries, and small updates keep the system current.

## System Modules

| Module | Purpose | Visibility |
|---|---|---|
| `vault/` | Private Markdown knowledge base, Obsidian-compatible vault, and canonical source of truth | Private |
| `n8n-workflows/` | Public workflow planning docs for capture, summaries, consolidation, and sync trigger mechanisms | Public-safe documentation |
| `workflows/` | Future private repo for real n8n workflow exports and runtime automation details | Private |
| Backend sync service | Future service that parses vault content, chunks documents, creates embeddings, and updates the vector index | Public-safe if it contains no private vault data |
| Retrieval/API layer | Future semantic search and question-answering API | Public-safe if configured without secrets or private data |
| Portfolio UI | Future public chatbot or portfolio interface | Public-safe |

## Repository Layout

```text
IdentityOS/
  README.md                    # Project overview
  AGENTS.md                    # Lightweight workspace agent router
  REPOSITORY_BOUNDARIES.md     # Repository ownership and Git boundaries
  SECURITY_AND_PRIVACY.md      # Privacy, public/private data, and secret handling rules
  ROADMAP.md                   # Current status, phases, and next priorities
  SYSTEM_ARCHITECTURE.md       # Stable system architecture and data flow

  n8n-workflows/               # Public workflow planning and mechanism docs
    README.md
    overview.md
    quick-note-capture-telegram.md
    daily-reminder.md
    structured-note-capture-form.md
    automatic-daily-capture.md
    daily-summary-approval.md
    weekly-consolidation.md
    weekly-vector-sync.md
    manual-recovery-and-retry.md

  vault/                       # Private knowledge layer
    README.md                  # Vault-specific overview
    AGENTS.md                  # Vault-specific agent rules
    VAULT_GUIDE.md             # Vault conventions and maintenance process
    _index.md                  # Vault table of contents
    achievements/
    current-focus/
    daily-notes/
    education/
    experience/
    projects/
    skills/
    templates/
    weekly-summaries/

  workflows/                   # Future private n8n workflow implementation repo
  backend-sync/                # Future backend sync service repo
```

Future public modules such as backend sync code can live in separate repositories when their privacy and deployment boundaries are clearer. Real workflow exports and operational automation details should live in the future private `workflows/` repo, while `n8n-workflows/` remains public planning documentation.

## Git Repository Structure

The root IdentityOS repo tracks project-level documentation and coordination files only.

Root `.gitignore` ignores all direct child folders with `/*/`. This keeps child modules out of the root repo by default, including `vault/`, `n8n-workflows/`, future `workflows/`, and future `backend-sync/`.

Each child module should own its own Git repository when it needs independent privacy, visibility, deployment, or release history. Run Git commands from the repo that owns the files being changed.

See `REPOSITORY_BOUNDARIES.md` for the full ownership model.

If the root repo ever needs to track a child folder directly, add an explicit exception to root `.gitignore` before staging it.

## Current Status

IdentityOS is in Phase 1: Knowledge Foundation.

Completed foundation work includes the vault structure, note templates, Maps of Content, and initial knowledge entries. Current work focuses on improving the vault content and preparing the automation and sync architecture.

See `ROADMAP.md` for the current phase, active focus, and next priorities.

## High-Level Architecture

```text
Vault Markdown
  -> Daily and weekly consolidation
  -> Backend sync service
  -> Chunks and embeddings
  -> Vector database and metadata index
  -> Retrieval API
  -> LLM answer generation
  -> Portfolio assistant response
```

The central rule for AI consumers is that answers should use retrieved context from the vault, not memorized or inferred facts.

See `SYSTEM_ARCHITECTURE.md` for the full system design.

## Key Documents

| Document | Purpose |
|---|---|
| `SYSTEM_ARCHITECTURE.md` | Stable architecture, layers, and data flow |
| `ROADMAP.md` | Current status, phases, and upcoming work |
| `AGENTS.md` | Lightweight workspace rules and links to module-specific agent guidance |
| `REPOSITORY_BOUNDARIES.md` | Repository ownership, child module boundaries, and Git rules |
| `SECURITY_AND_PRIVACY.md` | Privacy model, secret handling, and public/private data boundaries |
| `vault/README.md` | Vault-specific overview |
| `vault/AGENTS.md` | Vault-specific agent rules |
| `vault/VAULT_GUIDE.md` | Vault writing conventions, naming rules, and maintenance process |
| `vault/_index.md` | Vault table of contents |
| `n8n-workflows/AGENTS.md` | n8n workflow documentation rules for agents |

## Privacy Model

The vault contains personal knowledge and should remain private. Public repositories or examples should use sanitized sample data only.

Secrets, credentials, API keys, webhook URLs, and private tokens should never be committed to any repository. Workflow exports should be reviewed before publication because they can reveal implementation details even without credentials.

See `SECURITY_AND_PRIVACY.md` for the full privacy model.

## Development Notes

- Keep system-level documentation at the project root.
- Keep vault-specific guidance inside `vault/`.
- Keep real personal data in the private vault only.
- Keep `n8n-workflows/` limited to public workflow planning and mechanism documentation.
- Keep real n8n exports and operational workflow details in the future private `workflows/` repo.
- Use `.env.example` files and sanitized samples for public backend or workflow repositories.
- Do not stage child module contents from the root repo.
- Prefer small, frequent documentation updates over large infrequent rewrites.
