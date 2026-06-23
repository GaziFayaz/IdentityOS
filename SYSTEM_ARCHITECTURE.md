# System Architecture

Last Updated: 2026-05-31

---

# Overview

This system is a Personal Knowledge System + AI Portfolio Assistant.

It maintains a structured, queryable representation of:

- Professional experience
    
- Skills and technologies
    
- Projects
    
- Education
    
- Achievements
    
- Ongoing work (current focus)
    
- Historical activity (daily/weekly logs)
    

It enables an AI system to answer questions about the user using **retrieved factual knowledge**, not memorization.

---

# Core Principle

This system follows a **Retrieval-Augmented Generation (RAG)** architecture.

The AI must:

1. Retrieve relevant information from the knowledge base
    
2. Then generate an answer based only on retrieved context
    

The AI must NOT rely on internal memory or assumptions.

---

# High-Level Architecture

User Query  
↓  
API Layer  
↓  
Retrieval System  
↓  
Knowledge Index (Vector + Metadata)  
↓  
Relevant Documents  
↓  
LLM (Answer Generator)  
↓  
Response

---

# System Layers

## 1. Knowledge Layer (Vault)

Source of truth for all data.

Contains structured Markdown notes:

- projects/
    
- skills/
    
- experience/
    
- education/
    
- achievements/
    
- current-focus/
    
- daily-notes/
    
- weekly-summaries/
    

Rules:

- Human-readable format
    
- Markdown-based
    
- Consistent structure
    
- No duplication of facts across multiple files
    

---

## 1.5 Automation Layer (n8n)

Purpose: Capture daily activity signals and convert them into vault-ready knowledge.

Responsibilities:

- Receive raw manual notes from Telegram, n8n forms, or future personal input tools
- Fetch automatic activity signals from GitHub, calendar, and relevant local/vault changes
- Store raw capture data outside the vault in n8n-controlled staging storage
- Combine raw manual notes and automatic captures into a daily summary draft
- Send daily summary drafts for approval or correction
- Write approved daily summaries to `daily-notes/`
- Consolidate daily summaries into approved weekly summaries
- Trigger the backend sync service after approved weekly summaries are written

The automation layer must not write raw captures directly into the searchable vault. It produces human-readable vault notes that follow vault conventions.

The automation layer should not own markdown parsing, chunking, embedding generation, vector database writes, or sync metadata. Those responsibilities belong to the synchronization layer.

---

## 2. Synchronization Layer

Purpose: Convert vault into machine-readable knowledge.

Responsibilities:

- Detect file changes
    
- Parse Markdown content
    
- Extract metadata
    
- Chunk documents
    
- Generate embeddings
    
- Store in vector database
    

Only changed files should be reprocessed (incremental sync). For recent activity, approved weekly summaries are the preferred sync boundary. Daily notes remain available as historical detail and may be indexed selectively later if retrieval quality requires it.

---

## 3. Search Layer (Retrieval Engine)

Purpose: Find relevant knowledge for a query.

Responsibilities:

- Semantic search (vector similarity)
    
- Metadata filtering
    
- Ranking / re-ranking results
    
- Returning top-k relevant chunks
    

Output: contextual documents for LLM

---

## 4. Intelligence Layer (LLM)

Purpose: Generate answers using retrieved context.

Responsibilities:

- Interpret user question
    
- Use retrieved documents
    
- Generate grounded response
    
- Avoid hallucination
    

Rule:

If context is insufficient → respond with uncertainty.

---

## 5. Presentation Layer

Purpose: User-facing interfaces.

Examples:

- Portfolio chatbot UI
    
- Admin dashboard (future)
    
- Internal tooling
    

This layer must remain thin.

All logic belongs in backend services.

---

# Data Model Philosophy

All knowledge is stored as:

- Structured Markdown
    
- Optional metadata (frontmatter)
    
- Clear, human-readable sections
    

Example:

---

type: project  
status: active  
technologies:

- NestJS
    
- PostgreSQL
    

---

# Project Name

Content...

---

# Retrieval Strategy

Priority order:

1. Current Focus
    
2. Projects
    
3. Experience
    
4. Skills
    
5. Weekly Summaries
    
6. Daily Notes
    

Reason:

More recent and structured data is more relevant.

---

# Metadata Standards

Searchable notes SHOULD include a lightweight common frontmatter core:

```yaml
summary: ""
importance: medium
visibility: public
```

Allowed values:

- `importance`: `low`, `medium`, `high`
- `visibility`: `public`, `internal`, `private`

Relationship fields use stable vault-relative note references without `vault/`, `.md`, or wiki-link brackets:

```yaml
related_projects:
  - projects/fleetblox
related_skills:
  - skills/backend/nodejs
related_experiences:
  - experience/llamamind-backend-developer
```

Human-readable fields such as `technologies`, `topics`, `company`, `role`, and `title` remain important for semantic search. Relationship fields are primarily for exact filtering, graph traversal, and future validation.

During the future indexing pipeline, frontmatter can be copied into chunk metadata directly. A first implementation does not need to resolve relationship IDs into richer objects.

Example chunk metadata shape:

```ts
metadata: {
  source_file: "vault/projects/fleetblox.md",
  note_type: "project",
  title: "Fleetblox",
  summary: "SaaS fleet management platform with real-time vehicle telemetry...",
  importance: "high",
  visibility: "public",
  status: "completed",
  company: "Llamamind",
  technologies: ["Node.js", "Express.js", "PostgreSQL"],
  related_skills: ["skills/backend/nodejs", "skills/backend/express"],
  related_experiences: ["experience/llamamind-backend-developer"]
}
```

Metadata improves:

- Filtering
- Ranking
- Context precision
- Graph-aware retrieval
    

---

# Security Rules

The system must NOT index or expose:

- API keys
    
- Secrets
    
- Credentials
    
- Private sensitive data not intended for public exposure
    

Only vault-approved data is searchable.

---

# System Responsibilities

Vault  
→ Stores canonical human-readable knowledge

n8n Automation Layer  
→ Captures raw signals outside the vault, writes approved summaries into the vault, and triggers backend sync

Sync Service  
→ Converts vault → embeddings and owns vector database writes

Vector DB  
→ Stores searchable representations

API Service  
→ Handles question answering

Frontend  
→ Displays chat interface

---

# Design Principles

1. Simplicity over complexity
    
2. Retrieval before generation
    
3. Human-readable source of truth
    
4. Modular services
    
5. No tight coupling between components
    
6. Knowledge must exist before it can be queried
    

---

# Monitoring (Future)

Planned metrics:

- Index freshness
    
- Sync success rate
    
- Query latency
    
- Retrieval accuracy
    
- Response quality feedback
    

---

# Architectural Stability Rule

This document defines system structure.

It should only change when:

- A new service is added
    
- A layer is modified
    
- Data flow changes
    
- Storage or retrieval strategy changes
    

It should NOT include planning or future roadmap.

---

# Decision Log

Append-only log of architectural decisions.

Format:

Date:  
Decision:  
Reason:  
Impact:

---

# Success Criteria

The system is successful when:

- All knowledge is correctly retrievable
    
- Answers are grounded in vault data
    
- Updates require minimal effort
    
- System remains understandable without external context
