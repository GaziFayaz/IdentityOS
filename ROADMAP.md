# Roadmap

Last Updated: 2026-06-20

---

# Purpose

This document tracks:

- Current system status
    
- Completed components
    
- Work in progress
    
- Planned features
    
- Known issues
    
- Next development priorities
    

It acts as the **operational memory** of the system.

Unlike SYSTEM_ARCHITECTURE.md, this file changes frequently.

---

# Current Status (Snapshot)

## Completed

- Vault structure created
    
- Knowledge categories defined
    
- Templates created
    
- MOC (Map of Content) structure implemented
    

---

## In Progress

- Initial knowledge population (manual)
    
- Daily notes workflow setup
- n8n workflow documentation for manual capture, automatic capture, daily summaries, weekly consolidation, manual recovery, and backend vector sync triggering


---

## Not Started

- Sync service (vault → embeddings)
    
- Vector database integration
    
- Retrieval API
    
- Portfolio chatbot UI
    
- Automated ingestion pipelines
    

---

# Active Focus

Current primary focus:

- Populate high-quality real data into vault
    
- Establish consistent daily logging habit
    

Secondary focus:

- Prepare system for indexing layer (Phase 3)
    

---

# Development Phases

## Phase 1 — Knowledge Foundation (Current)

Goal:  
Build a complete and high-quality personal knowledge vault.

Includes:

- Projects documentation
    
- Skills documentation
    
- Experience entries
    
- Daily notes habit
    
- Current focus tracking
    

Status: ACTIVE

---

## Phase 2 — Sync Infrastructure

Goal:  
Convert vault into structured machine-readable data.

Includes:

- File watcher / change detection
    
- Markdown parsing
    
- Chunking strategy
    
- Embedding generation
    
- Vector database setup
    

Status: NOT STARTED

---

## Phase 3 — Retrieval System

Goal:  
Enable semantic search over personal knowledge.

Includes:

- Query embedding
    
- Vector search
    
- Context ranking
    
- Filtering by metadata
    

Status: NOT STARTED

---

## Phase 4 — Question Answering API

Goal:  
Create backend service for chat interface.

Includes:

- `/ask` endpoint
    
- Prompt construction
    
- Context injection
    
- LLM response generation
    

Status: NOT STARTED

---

## Phase 5 — Portfolio Chat UI

Goal:  
Public-facing AI assistant.

Includes:

- Chat interface on portfolio
    
- Streaming responses
    
- Conversation handling
    

Status: NOT STARTED

---

## Phase 6 — Automation Layer

Goal:  
Reduce manual updates.

Includes:

- GitHub sync
- Calendar ingestion
- Activity summarization
- Daily AI notes generation
- Weekly summarization pipeline
- Telegram quick note capture
- n8n form-based structured note capture
- Human approval or correction of daily summaries
- Backend vector sync trigger after approved weekly consolidation
    

Status: NOT STARTED

---

## Phase 7 — Personal Intelligence Layer

Goal:  
Make system understand trends over time.

Includes:

- Skill evolution tracking
    
- Project progression analysis
    
- Learning trajectory insights
    

Status: FUTURE

---

# Known Issues

- Vault currently has limited real data
    
- No automated sync exists yet
    
- No retrieval layer implemented
    
- Daily notes habit not yet consistent
    

---

# Key Decisions Log

Append-only history of roadmap decisions.

Format:

Date:  
Decision:  
Reason:  
Impact:

Example:

Date:  
2026-05-31

Decision:  
Start with manual vault population before building AI pipeline.

Reason:  
High-quality data is required before automation.

Impact:  
Delays technical build but improves final system quality.

---

Date:  
2026-06-20

Decision:  
Use weekly consolidation as the primary vector update boundary for recent activity.

Reason:  
Weekly summaries reduce retrieval noise while finalized daily notes preserve detailed historical context.

Impact:  
n8n workflows should capture raw signals outside the vault, create approved daily summaries in `vault/daily-notes/`, create approved weekly summaries in `vault/weekly-summaries/`, and trigger the backend sync service after weekly consolidation.

---

# Weekly Update Rule

This document MUST be updated:

- Every time a new feature is started
    
- When a phase is completed
    
- When priorities change
    
- When blockers appear
    
- At minimum: once per week
    

---

# Definition of “Done”

The system is considered complete when:

- Vault is fully populated
    
- Sync is automated and reliable
    
- Retrieval is accurate
    
- Chatbot is deployed on portfolio
    
- System is maintainable without confusion
    
- Future changes can be made safely using these documents
    

---

# Relationship to Other Documents

- `vault/VAULT_GUIDE.md` -> how knowledge is structured
    
- `SYSTEM_ARCHITECTURE.md` -> how system works
    
- `ROADMAP.md` -> what is currently happening and what is next
    

These documents form the operating context of this project.
