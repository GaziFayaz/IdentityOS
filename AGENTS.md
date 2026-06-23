# AGENTS.md — IdentityOS

## What This Project Is

IdentityOS is a **Personal Knowledge System + AI Portfolio Assistant** built on a Retrieval-Augmented Generation (RAG) architecture. The vault at `vault/` is the source of truth about **Gazi Fayaz Ahmed** — every answer the system generates must be grounded in documents retrieved from this vault. The system is currently in Phase 1 (Knowledge Foundation), focused on populating high-quality structured data before building the AI pipeline.

## Repository Boundaries

- The root IdentityOS Git repo tracks only project-level documentation and coordination files.
- Root `.gitignore` uses `/*/`, so all direct child folders are ignored by default.
- Child folders such as `vault/`, `n8n-workflows/`, future `workflows/`, and future `backend-sync/` should be separate Git repositories when they need independent privacy, visibility, deployment, or release history.
- `vault/` is the private knowledge repo.
- `n8n-workflows/` is public workflow planning and mechanism documentation.
- `workflows/` is reserved for the future private repo containing real n8n workflow exports and operational automation details.
- Do not stage child module contents from the root repo.
- Commit changes from inside the repo that owns the changed files.

## Vault Structure

```
vault/
  _index.md                      # Root table of contents
  achievements/                  # Certifications, publications, career milestones
    _achievements-moc.md         # MOC for achievements
    Certifications/
    Publications/
    Career/
  current-focus/                 # What the user is currently working on/learning
  daily-notes/                   # Finalized daily work summaries
  education/                     # Formal education
  experience/                    # Work experience entries
    _experience-moc.md
  projects/                      # Software project documentation
    _projects-moc.md
  skills/                        # Technical and professional skills
    _skills-moc.md
    ai/  backend/  database/  devops/  frontend/
  weekly-summaries/              # Weekly consolidated knowledge for retrieval
  templates/                     # Note templates for each entity type
```

## Key Rules for All Agents

### 1. Follow the Templates
Every new entry in the vault **must** follow its corresponding template in `vault/templates/`. Available templates:

| Template | Type Field | When to Use |
|---|---|---|
| `skill-template.md` | `skill` | Documenting a technology/tool/skill |
| `project-template.md` | `project` | Documenting a software project |
| `experience-template.md` | `experience` | Documenting a work experience/role |
| `education-template.md` | `education` | Documenting formal education or coursework |
| `publication-template.md` | `publication` | Documenting a paper, talk, or article |
| `certification-template.md` | `certification` | Documenting a certification/exam |
| `career-achievement-template.md` | `career-achievement` | Documenting a promotion, award, or milestone |

### 2. Always Update the MOC
Every time you create a new entry file, **add a wiki-link** to it in the corresponding MOC (Map of Content) file. MOC files are prefixed with `_` and end in `-moc.md` (e.g., `_achievements-moc.md`).

### 3. Naming Conventions
- **Filenames**: lowercase with hyphens (`complete-web-development-course.md`)
- **MOC files**: `_section-moc.md` prefix
- **Use wiki-links** for cross-references: `[[Certifications/entry-name]]`
- Subfolders under categories use **PascalCase** (e.g., `Certifications/`, `Publications/`)

### 4. Frontmatter Standards
Every file must include YAML frontmatter delimited by `---`:
```yaml
---
title: "Entry Title"
type: certification  # Must match one of the valid types
tags: [achievement, certification]
date: 2024-06-01
# Additional fields per template
---
```
Valid `type` values: `skill`, `project`, `experience`, `education`, `publication`, `certification`, `career-achievement`, `daily-note`, `weekly-summary`, `moc`, `now`, `index`

### RAG Metadata Standards
Searchable content notes should include this common metadata core:
```yaml
summary: ""
importance: medium
visibility: public
```

- `summary` is a short factual description for previews, ranking, and chunk context.
- `importance` must be `low`, `medium`, or `high`.
- `visibility` must be `public`, `internal`, or `private`.

Use type-specific relationship fields only where they are useful. Empty arrays are intentional and mean no relationship is currently known. Relationship targets in frontmatter use stable vault-relative note references without `vault/`, `.md`, or wiki-link brackets:
```yaml
related_projects:
  - projects/fleetblox
related_skills:
  - skills/backend/nodejs
related_experiences:
  - experience/llamamind-backend-developer
related_achievements: []
```

Use human-readable fields such as `technologies`, `topics`, `company`, `role`, and `title` for semantic retrieval. Use relationship fields for exact filtering and graph traversal.

### Career vs Experience
`vault/experience/` stores full job, role, internship, and professional experience notes. `vault/achievements/Career/` stores only discrete career milestones such as promotions, awards, recognitions, or notable delivery achievements. Do not duplicate full role histories in career achievement notes.

### 5. Markdown Body Structure
- H1 (`#`) mirrors the `title` field
- H2 (`##`) sections follow the template structure
- Unordered lists for enumerations
- No duplication of facts across files

### 6. No Hallucination
The RAG system will use this vault as its only knowledge source. Every fact must be explicit in the text. Do not add speculative content. When a field is unknown, leave it blank (`""`) rather than inventing values.

### 7. Never Expose Secrets
Do not write API keys, passwords, tokens, or private credentials into any vault file.

## How Entries Should Be Written

- **Clean, professional writing** — polish raw notes into well-structured prose
- **Wiki-links** to related skills, projects, and experiences
- **Date format**: `YYYY-MM-DD` or `YYYY-MM`
- **External links**: Markdown links with descriptive text
- **Empty fields**: Use `""` for unknown values in frontmatter; omit fields that don't apply rather than filling with placeholder text

## Writing Principles from VAULT_GUIDE.md

- **Facts Over Marketing** — The vault is a knowledge repository, not a resume or portfolio. Store facts, details, lessons learned, challenges, decisions, and outcomes. Future AI systems can transform facts into resumes or summaries.
- **Human Readable First** — All notes must be understandable by a human. AI systems consume the same information humans read. Avoid storing information in formats that require tooling to understand.
- **Incremental Updates** — Small frequent updates are preferred over large infrequent ones. A two-minute update daily is better than a large update monthly.
- **Single Source of Truth** — Information exists in one canonical location. Other notes reference it via wiki-links rather than duplicating content.

## Content Expectations Per Note Type

### Project Notes
Should answer: what the project is, why it was built, technologies used, challenges solved, what was learned, and outcome.

### Skill Notes
Should answer: what the skill is, how it has been used, which projects use it, current proficiency level, and future learning goals.

### Daily Notes
Daily notes are finalized daily summaries generated from combined raw capture sources. They should capture work completed, problems solved, technologies explored, interesting discoveries, and ideas worth revisiting. Consistency matters more than detail.

### Experience Notes
One file per company, role, or significant position. Track responsibilities, achievements, technologies used, and lessons learned.

## Maintenance Cadence

- **Daily**: Update daily note and current focus if needed
- **Weekly**: Create weekly summary, review recent activity, update active projects
- **Monthly**: Review skills, experience, project status; archive outdated info

## Daily Capture And Weekly Indexing Convention

- Raw manual notes and raw automatic captures are staging data, not canonical vault knowledge.
- Raw staging data should be stored outside the searchable vault in n8n-controlled storage.
- Manual daily input and automatic captures must be merged before creating the final daily note.
- Final daily notes belong in `vault/daily-notes/YYYY-MM-DD.md`.
- Weekly summaries belong in `vault/weekly-summaries/YYYY-Www.md`.
- Telegram quick note capture is the first-priority manual input channel.
- n8n form capture is the second-priority structured input channel.
- A custom personal web app is a later enhancement, not the first implementation.
- Vector database updates should be triggered after weekly consolidation, not after every raw daily capture.
- n8n should trigger the backend sync service for vector updates; the backend owns parsing, chunking, embeddings, vector upserts, sync metadata, and indexing retries.
- Daily notes may be indexed later only if retrieval quality needs more recent detail.
- Workflow schedules must remain configurable until actual operating times are chosen.

## MOC Pattern

Each top-level section has a `_section-moc.md` that serves as its index. When adding an entry:
1. Create the entry file in the appropriate directory
2. Open the corresponding `_*-moc.md`
3. Add a wiki-link under the relevant category heading
4. Do not change the MOC's frontmatter or structure

## Relationship Between Project Docs

- `AGENTS.md` (this file) → rules for AI agents working on the vault
- `SYSTEM_ARCHITECTURE.md` → how the RAG system works (stable, rarely changes)
- `ROADMAP.md` → current status, phases, and next priorities (changes frequently)
- `vault/VAULT_GUIDE.md` → conventions, naming, maintenance process for the vault
- `vault/_index.md` → user-facing table of contents

## Verification Checklist

Before claiming work is complete:
- [ ] Entry follows the correct template
- [ ] Frontmatter includes `type`, `tags`, and applicable date fields
- [ ] H1 matches the `title` field
- [ ] MOC updated with wiki-link
- [ ] Filename uses lowercase-hyphen convention
- [ ] No duplicated content across files
- [ ] No placeholder text remains (or placeholders are intentional)
