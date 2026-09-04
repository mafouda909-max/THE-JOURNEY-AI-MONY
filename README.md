# THE-JOURNEY-AI-MONY

**Project:** LENS — Cinematic Business Storytelling for Arabic Audiences  
**Status:** Phase 0 → Phase 1 (Infrastructure Setup)  
**Source of Truth:** `CHANNEL_BRAIN.md` (see `docs/` for extracted version)

## Quick Links

- **Strategic Guide:** [CHANNEL_BRAIN.md](./docs/CHANNEL_BRAIN.md)
- **Architecture:** [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md)
- **Database Schema:** [database/schema.sql](./database/schema.sql)
- **Agent System:** [agents/README.md](./agents/README.md)
- **API Docs:** [backend/README.md](./backend/README.md)

## Project Overview

LENS is an AI-assisted content production system for creating cinematic business storytelling videos in Arabic. The platform orchestrates:

- **Research & Fact-Checking**: Automated discovery, verification, and source tracking
- **Content Production**: Script generation, visual planning, asset management
- **Publishing**: Multi-platform distribution (YouTube, TikTok, Instagram, etc.)
- **Analytics & Learning**: Performance tracking and systematic improvement

### Key Principles

1. **Accuracy First** — No content ships without verified sources
2. **Story-Driven** — All content serves narrative architecture (§D)
3. **Human Gates** — Critical decisions (GATE-1 through GATE-4) remain human-approved
4. **Systematic Learning** — Every video teaches us something for the next

## Architecture at a Glance

```
LENS Production Pipeline
┌─────────────────────────────────────────────────────────┐
│  Discovery → Research → Fact-Check → Story Selection    │
│     (AGT-SCOUT)  (AGT-RES)  (AGT-FACT)  (AGT-STRAT)     │
└──────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│  Outline → Script → QA → Visual → Assets → Audio        │
│ (SCRIPT)  (SCRIPT) (EDIT) (VIS) (ASSET) (VOICE)        │
└──────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│  Editing → Thumbnail → Title → SEO → Final QA           │
│   (HUMAN)   (ASSET)   (SEO)   (SEO)  (QA/ORCH)         │
└──────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│  Publish → Distribute → Analytics → Learning            │
│  (AGT-PUB) (AGT-DIST)  (AGT-ANLY)  (AGT-LRN)           │
└──────────────────────────────────────────────────────────┘
```

Orchestrated by **AGT-ORCH** with 4 mandatory human approval gates.

## Project Structure

```
THE-JOURNEY-AI-MONY/
├── docs/                          # Documentation
│   ├── CHANNEL_BRAIN.md           # Full strategic document (extracted from ZIP)
│   ├── ARCHITECTURE.md            # Technical architecture decisions
│   ├── DATABASE.md                # Database design notes
│   └── AGENTS.md                  # Agent system reference
├── backend/                       # Node.js/Express API
│   ├── src/
│   │   ├── api/                   # API routes
│   │   ├── services/              # Business logic
│   │   ├── agents/                # Agent implementations
│   │   ├── database/              # DB access layer
│   │   └── middleware/            # Auth, validation, etc.
│   ├── package.json
│   ├── tsconfig.json
│   └── .env.example
├── database/                      # Supabase/Postgres
│   ├── schema.sql                 # Main schema
│   ├── migrations/
│   └── seed.sql
├── agents/                        # Standalone agent implementations
│   ├── research-agent/
│   ├── fact-checker-agent/
│   ├── orchestrator-agent/
│   └── README.md
├── prompts/                       # Prompt templates (§N)
│   ├── P-RES-01.yaml
│   ├── P-FCT-01.yaml
│   └── ...
├── config/                        # Configuration & constants
│   ├── constants.ts
│   ├── types.ts
│   └── env.ts
├── tests/                         # Test suite
├── .github/workflows/             # CI/CD
├── .gitignore
└── package.json (root)
```

## Getting Started

### Prerequisites

- Node.js 18+ and npm/yarn
- Supabase account (or local Postgres)
- Environment variables (see `.env.example`)

### Installation

```bash
# Clone and install dependencies
git clone https://github.com/mafouda909-max/THE-JOURNEY-AI-MONY.git
cd THE-JOURNEY-AI-MONY
npm install

# Set up environment
cp backend/.env.example backend/.env.local
# Edit backend/.env.local with your Supabase keys

# Run database migrations
npm run db:migrate

# Start backend
npm run dev
```

### Running Agents

```bash
# Orchestrator (manages the workflow)
npm run agent:orchestrator

# Individual agents can be tested independently
npm run agent:research
npm run agent:factcheck
```

## Development Workflow

1. **Understand the Document** — Read CHANNEL_BRAIN.md to grasp the full vision
2. **Check the Schema** — database/schema.sql is the contract between agents
3. **Add a Feature** — Implement agents incrementally, testing each stage
4. **Run Tests** — `npm test` validates behavior
5. **Deploy** — Vercel (frontend), Supabase (backend/DB)

## Current Phase

**Phase 1: Manual + AI Assisted** (4–6 months)

- Set up foundational infrastructure ✓ (in progress)
- Implement core agents (Research, Fact-Check, Story Selection)
- Build manual approval gates (GATE-1 through GATE-4)
- Produce first 20–30 test videos
- Measure and calibrate metrics

**Success Criteria:**
- Story-Market Fit: videos watched to completion, audience returns
- Baseline metrics established
- Quality gates operational

## Key Contacts & Roles

| Role | Responsibility |
|------|-----------------|
| Chief Strategy + Content | Editorial decisions, content approval, quality gates |
| Engineering Lead | Architecture, backend, agent orchestration |
| Content Producers | Script writing, sourcing, fact-checking |
| Community/Analytics | Metrics, feedback integration |

## Security & Privacy

- All source code is private (repo is private)
- Secrets managed via environment variables (never in code)
- Database RLS policies enforce access control
- API authentication via Supabase Auth or custom JWT
- External APIs rate-limited and monitored

## Q&A / Support

For architecture questions, refer to CHANNEL_BRAIN.md §K (Agent Architecture) and §J (Production Pipeline).

For implementation questions, open an issue or reach out to the maintainer.

---

**Last Updated:** 2026-09-04  
**Version:** 0.1.0-alpha
