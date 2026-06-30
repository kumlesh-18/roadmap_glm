# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with code in this repository.

## Project Overview

This is the **AI Engineer Roadmap Platform** — a learning platform for AI/ML engineers. The project is in transition from a single-file HTML prototype to a production-grade system. Three source files define the entire domain:

| File | Role |
|------|------|
| `ai_engineer_roadmap_fixed.html` | **Canonical source of truth**. An 11,810-line, 700KB single-file HTML app containing all 39 modules of curriculum content. All module data, learning steps, projects, code exercises, resources, and self-assessment checks live here as static HTML. |
| `ai_roadmap_platform_master_prompt.md` | **Production architecture spec**. Defines the complete rebuild architecture: Next.js + TypeScript frontend, NestJS modular monolith backend, PostgreSQL database, Redis cache, full AI orchestration layer, and deployment pipeline. Includes complete database schema, API contracts, module-by-module system units, and implementation milestones. |
| `AI_Engineer_Roadmap_Technical_Analysis.md` | **Reverse-engineering report**. Documents every function, data structure, event handler, and UI component in the existing HTML app. Essential for understanding what the current prototype does and how to preserve its behavior. |

## Canonical Curriculum Data

The roadmap is immutable in structure. All 13 phases and 39 modules must be preserved exactly.

### 13 Phases (display codes)
1. Phase 01 — Math Foundation (3–4 weeks, ~60 hrs)
2. Phase 02 — Data Structures & Algorithms (3–4 weeks, ~60 hrs)
3. Phase 02 (duplicate display) — NumPy, Pandas & Data Engineering (2–3 weeks, ~40 hrs)
4. Phase 03 — Classical ML (3–4 weeks, ~60 hrs)
5. Phase 04 — PyTorch — Training Neural Networks (4–5 weeks, ~80 hrs)
6. Phase 05 — Deep Learning — CNNs to Transformers (5–6 weeks, ~90 hrs)
7. Phase 06 — LLMs — Fine-Tuning, RLHF & Alignment (5–6 weeks, ~80 hrs)
8. Phase 07 — RAG Pipelines + Model Compression (6–7 weeks, ~90 hrs)
9. Phase 08 — Agentic AI, Autonomous Systems & AGI Frontier (6–8 weeks, ~80 hrs)
10. Phase 10 — Time Series & Recommender Systems (4–5 weeks, 48 hrs)
11. Phase 11 — MLOps, LLMOps & ML System Design (4–5 weeks, 56 hrs)
12. Phase 12 — Big Data Engineering (3–4 weeks, 44 hrs)
13. Phase 13 — Reinforcement Learning (5–6 weeks, 62 hrs)

### Critical Module IDs

```
Phase 01: 1-1, 1-2, 1-3, 1-4
Phase 02: dsa-1, dsa-2, dsa-3
Phase 03: 2-1, 2-2, 2-3, 2-4
Phase 04: 3-1, 3-2, 3-3
Phase 05: 4-1, 4-2, 4-3
Phase 06: 5-1, 5-2, 5-3, 5-4, 5-5
Phase 07: 6-1, 6-2, 6-3, 6-4, 6-5
Phase 08: 7-1, 7-2, 7-3
Phase 09: 8-1, 8-2, 8-3
Phase 10: 9-1, 9-2
Phase 11: 10-1
Phase 12: 11-1
Phase 13: 12-1, 12-2
```

- **Module 5-2 (Transformer from Scratch)** is the **critical path** — highest priority, hardest unlock, 40 hours.
- **Modules dsa-2 and 6-1** have no explicit projects in the source HTML — the system must generate required lab atoms tagged `generated_required`.
- The ALL_MODS array in the HTML (line ~8852) contains only `{ id, name, phase, label }` metadata. Rich content (steps, projects, resources, checks) exists as static HTML embedded in the DOM.

## Key Architecture Decisions

The production spec in `ai_roadmap_platform_master_prompt.md` defines:

- **Frontend**: Next.js App Router, TypeScript strict, TanStack Query, Zustand, Tailwind CSS, shadcn/ui
- **Backend**: NestJS modular monolith, tRPC for internal, REST for external
- **Database**: PostgreSQL with pgvector, Drizzle ORM
- **Cache/Queue**: Redis (Upstash), BullMQ for background jobs
- **Auth**: OAuth 2.0 (Google/GitHub) + JWT sessions + refresh tokens
- **AI Proxy**: Server-side only, NO client-side API keys. BYOK encrypted with AES-256-GCM + KMS.
- **Storage**: S3-compatible (Cloudflare R2) for project artifacts and exports

### Non-Negotiables

1. **Content is immutable**. No module is merged or renamed in canonical storage. Versioned as `roadmap_versions`.
2. **Graph edges use FK references**, not text nodes. `graph_edges.from_module_id` → `modules(id)`.
3. **Mastery score is never computed live**. Dashboard reads pre-computed `module_progress.mastery_score`.
4. **Question bank is backbone of assessment**. All LLM-generated questions are persisted.
5. **Token budget is enforced**. Context assembler allocates: system 500 + module atoms 1500 + weakTags 300 + history 2000 + userMessage 500 + reserve 500 = ~5600 tokens max.
6. **AI dead-ends always degrade gracefully**. No blank quiz. No broken chat. Every failure path has a deterministic fallback.
7. **Import dry-run is strictly read-only**. Schema version adapters handle all known export versions. Confirm step is the only write.
8. **API keys NEVER stored client-side**. The existing HTML stores API keys in localStorage — this is a critical security flaw the production system eliminates.

## Working with Source Files

### `ai_engineer_roadmap_fixed.html`
- **Line ranges for key sections**:
  - CSS styles: line 24–2618
  - Phase 01 (ph0) HTML: line 2618–3245
  - Phase modules in HTML: ph0–ph12, each is a `<section class="phase" id="phN">`
  - ALL_MODS array: line 8852–8892
  - JavaScript: line ~8900–end
- All module content is pre-rendered static HTML. To extract data, read the phase sections (ph0 through ph12) and parse learning steps, projects, resources, common mistakes, and self-assessment checks.
- The HTML uses inline `onclick` handlers extensively. No event delegation or reactive framework.
- State persists to localStorage with `airv3_*` prefix for core data and `aim_*` for AI data.

### `ai_roadmap_platform_master_prompt.md`
- **Section 1**: Canonical learning graph (phases, modules, dependency graph)
- **Section 2**: Full architecture (frontend, backend, directories)
- **Section 3**: Complete database schema (PostgreSQL with pgvector)
- **Section 4**: API contracts and endpoints
- **Section 5**: AI layer specification (provider strategy, token budgets, prompt contracts, dead-end handlers)
- **Section 6**: Adaptive mastery engine (weighted scoring formula, state machine)
- **Section 7**: Offline-first sync (IndexedDB with Dexie)
- **Section 8**: Import/export with schema versioning
- **Section 10**: Implementation milestones (M0–M7)
- **Section 13**: Non-negotiable peer review refinements
- **Section 15**: Module-by-module complete reference (learning steps, projects, measurable objectives, system units)

### `AI_Engineer_Roadmap_Technical_Analysis.md`
- Section mapping: Executive Summary → File Analysis → Features → Functions (120+) → Events → Data Architecture → UI Components → Rendering → Security Audit → Performance → Scalability → Rebuild Blueprint → Module-by-Module Breakdown → Missing Features
- Key for understanding current prototype behavior and identifying what to preserve vs. redesign

## Data Models

### Module Content Atom Types
```
time_budget    why            learning_step
code_exercise  project        common_mistake
resource       self_assessment
```

### Progress State Machine
```
locked → unlocked → in_progress → mastery_pending → complete → review_recommended
```

### Module State Values
- `locked`: prerequisites not met
- `unlocked`: prerequisites met
- `in_progress`: first view / start / check / chat / quiz
- `mastery_pending`: completion clicked without full evidence
- `complete`: `mastery_score >= 0.75`
- `review_recommended`: later weak tag matches module core tag

## API Patterns

### Key Endpoints (production spec)
- `GET /api/learning-graph/current` — cached by version
- `GET /api/modules/{moduleId}` — module content atoms + state
- `POST /api/modules/{moduleId}/completion` — idempotent with idempotency key
- `PATCH /api/checks/{checkId}` — optimistic UI safe, includes context
- `POST /api/assessments/generate` — AI generation with deterministic fallback chain
- `POST /api/assessments/{assessmentId}/attempts` — writes mastery events
- `POST /api/ai/tutor/stream` — SSE streaming, free provider only
- `POST /api/import/progress` — dry-run merge report with migration preview
- `POST /api/import/progress/confirm` — only write endpoint for imports

## Development Context

This project currently has **no existing build system, package.json, or backend code.** It is three files: a single HTML prototype and two markdown specification documents. The codebase is in the planning/metadata stage of the rebuild described in the architecture document.

When implementing, the canonical algorithm is:
1. Extract all content from `ai_engineer_roadmap_fixed.html` (read phase sections, not the ALL_MODS array which only has metadata)
2. Structure as normalized data per the schema in `ai_roadmap_platform_master_prompt.md`
3. Build the production system following the architecture specification
4. Preserve all 39 modules, 13 phases, learning steps, projects, code exercises, resources, and self-assessment checks exactly
