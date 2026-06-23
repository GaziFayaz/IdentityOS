# IdentityOS

IdentityOS is a Personal Knowledge System and AI Portfolio Assistant built around a private Markdown knowledge vault and a Retrieval-Augmented Generation (RAG) architecture.

The system is designed to help an AI assistant answer questions about Gazi Fayaz Ahmed using retrieved, factual knowledge rather than model memory or assumptions.

## Purpose

IdentityOS exists to turn personal professional knowledge into a reliable source of truth for future AI-powered portfolio, search, and question-answering experiences.

The project currently focuses on building a high-quality knowledge foundation before adding the sync, retrieval, API, and user interface layers.

## Core Principles

- **Vault as source of truth**: generated answers must be grounded in retrieved vault documents.
- **Human-readable first**: Markdown notes should be useful to a person before they are useful to automation.
- **Facts over marketing**: the vault stores concrete facts, work history, decisions, outcomes, and lessons learned.
- **Privacy by module**: private personal data stays separate from code and sanitized examples that can be public.
- **Incremental maintenance**: daily notes, weekly summaries, and small updates keep the system current.

## System Modules

| Module | Purpose | Visibility |
|---|---|---|
| `vault/` | Private Markdown knowledge base and Obsidian-compatible vault | Private |
| Workflows | Automation for capture, summaries, consolidation, and sync triggers | Private or sanitized public examples |
| Backend sync service | Parses vault content, chunks documents, creates embeddings, and updates the vector index | Public-safe if it contains no private vault data |
| Retrieval/API layer | Future semantic search and question-answering API | Public-safe if configured without secrets or private data |
| Portfolio UI | Future public chatbot or portfolio interface | Public-safe |

## Repository Layout

```text
IdentityOS/
  README.md                    # Project overview
  AGENTS.md                    # Project-wide agent instructions
  ROADMAP.md                   # Current status, phases, and next priorities
  SYSTEM_ARCHITECTURE.md       # Stable system architecture and data flow

  vault/                       # Private knowledge layer
    README.md                  # Vault-specific overview
    AGENTS.md                  # Pointer to project-wide agent instructions
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
```

Future public modules such as backend sync code or sanitized workflow examples can live in separate repositories when their privacy and deployment boundaries are clearer.

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

The central rule is that the assistant should answer using retrieved context from the vault, not memorized or inferred facts.

See `SYSTEM_ARCHITECTURE.md` for the full system design.

## Key Documents

| Document | Purpose |
|---|---|
| `SYSTEM_ARCHITECTURE.md` | Stable architecture, layers, and data flow |
| `ROADMAP.md` | Current status, phases, and upcoming work |
| `AGENTS.md` | Project-wide rules for AI agents working in this workspace |
| `vault/README.md` | Vault-specific overview |
| `vault/VAULT_GUIDE.md` | Vault writing conventions, naming rules, and maintenance process |
| `vault/_index.md` | Vault table of contents |

## Privacy Model

The vault contains personal knowledge and should remain private. Public repositories or examples should use sanitized sample data only.

Secrets, credentials, API keys, webhook URLs, and private tokens should never be committed to any repository. Workflow exports should be reviewed before publication because they can reveal implementation details even without credentials.

## Development Notes

- Keep system-level documentation at the project root.
- Keep vault-specific guidance inside `vault/`.
- Keep real personal data in the private vault only.
- Use `.env.example` files and sanitized samples for public backend or workflow repositories.
- Prefer small, frequent documentation updates over large infrequent rewrites.
