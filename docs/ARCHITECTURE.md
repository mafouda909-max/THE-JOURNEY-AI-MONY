# LENS Project Architecture

**Last Updated:** 2026-09-04  
**Phase:** 1 (Manual + AI Assisted)

## Overview

LENS is a content production operating system that automates research, fact-checking, script generation, and publishing for cinematic business storytelling videos in Arabic.

The system operates on a **Human-in-the-Loop (HITL)** model with 4 mandatory approval gates and 13 autonomous AI agents orchestrated by a central workflow engine.

## Core Architecture

### 1. Production Pipeline (§J)

The content flows through 20 sequential stages, divided into 4 phases:

```
DISCOVERY PHASE (3 stages)
  └─ Idea Registration → Discovery → Research

CURATION PHASE (2 stages)
  └─ Fact Checking → Story Selection [GATE-1: Editorial approval]

PRODUCTION PHASE (9 stages)
  └─ Outline → Script → QA [GATE-2: Content approval]
              → Visual Plan → Assets → Audio
              → Editing → Thumbnail → Title → SEO

PUBLISHING PHASE (6 stages)
  └─ Final QA [GATE-4: Go/No-Go]
           → Publish [GATE-3: Publishing approval]
           → Distribution → Analytics → Feedback Loop → Learning
```

### 2. Agent System (§K)

| Agent | Role | Input | Output | Autonomy |
|-------|------|-------|--------|----------|
| AGT-SCOUT | Discovery | Trends/Sources | `discovery_note[]` | L1 (assisted) |
| AGT-RES | Research | `discovery_note` | `research_package` | L2 (semi-auto) |
| AGT-FACT | Fact Check | `research_package` | `fact_check_report` | L2 (escalates on fail) |
| AGT-STRAT | Scoring | Research + fact-check | `score_card` + decision | L1 (GATE-1 approval) |
| AGT-SCRIPT | Script Writer | Outline | `script_v1` | L1 (assisted) |
| AGT-EDIT | Story Editor | `script_v1` | `script_final` | L2 (with QA checks) |
| AGT-VIS | Visual Director | Script | `visual_plan` | L2 (semi-auto) |
| AGT-ASSET | Asset Manager | Visual plan | `asset_pack` | L2 (with human gate) |
| AGT-SEO | SEO | Script + keywords | Titles, desc, tags | L2 (semi-auto) |
| AGT-PUB | Publishing | QA certificate | `publish_receipt` | L2 (requires approval) |
| AGT-DIST | Distribution | Master video | Distribution logs | L2 (scheduled) |
| AGT-ANLY | Analytics | Platform data | `analytics_snapshot` | L3 (fully auto) |
| AGT-LRN | Learning | Insights | `rule_proposals` | L1 (proposed for review) |
| AGT-ORCH | Orchestrator | All states | Task transitions | L2 (enforces gates) |

### 3. Data Model (§M)

**Core Tables:**
- `ideas` — Seed ideas with metadata
- `research_packages` — Complete research with sources and claims
- `sources` — Tiered source tracking (Tier 1–4 hierarchy)
- `fact_checks` — Claim-by-claim verification status
- `story_scores` — Weighted scoring results
- `scripts` — Versioned script content (JSON scenes)
- `videos` — Final video metadata
- `publications` — Multi-platform publishing records
- `analytics_snapshots` — Performance metrics per publication
- `experiments` — A/B test definitions and results
- `learnings` — Extracted rules from data
- `agent_runs` — Audit log of all agent executions

**Relationships:**
```
idea 1→1 research_package
     1→N scripts (versions)
     1→N videos
     
research_package 1→N sources
                1→N fact_checks
                1→N claims
                
video 1→N publications
    1→N analytics_snapshots
    1→N experiments
```

### 4. Human Gates (§J.3)

**GATE-1: Story Selection** (after scoring)
- Decision: "Is this story worth our credibility?"
- Approval needed from: Chief Strategy
- Can reject, request research extension, or approve for production

**GATE-2: Script QA** (after final edit)
- Decision: "Does every claim stand up? Is narrative sound?"
- Approval needed from: Editor + Fact-Checker
- Can request rewrites or escalate for external review

**GATE-3: Assets & Thumbnail** (before publishing)
- Decision: "No misleading visuals? All rights clear?"
- Approval needed from: Visual Director + Legal
- Can reject assets or request regeneration

**GATE-4: Final QA** (before publication)
- Decision: "Ship this?"
- Approval needed from: Project Lead
- Last checkpoint—covers all 15 QA criteria (§T)

### 5. Safety & Editorial Rules (§S)

**Blocking Rules (R-01 through R-06):**
- No fabrication of facts or sources
- Every number has a verified source
- Clear distinction: fact vs. estimate vs. opinion
- No clickbait promises
- Copyright/licensing verified
- No misleading headlines or thumbnails

**Enforcement:**
- FactChecker validates at source-entry time
- Script QA re-validates at narrative time
- Final QA spot-checks before shipping
- Any violation triggers `FAIL` in §T checklist

### 6. Quality Metrics (§U)

**Primary Metric:** Average Story Completion % (retention to end + return viewers)

**Supporting Metrics:**
- CTR (title + thumbnail strength)
- Retention curve (narrative pacing)
- Avg view % (overall quality signal)
- Watch time (content value)
- Engagement (emotional/intellectual response)
- Subs gained + returning viewers (trust building)
- Conversion rates (Short → Long video, viewer → subscriber)

**Learning Loop:**
- Weekly: Top/bottom 3 performing videos analyzed
- Monthly: Learning Digest published with rule proposals
- Quarterly: §I scoring weights re-calibrated
- Every experiment documented in `experiments` table

### 7. Automation Levels

```
L0: Manual (no automation)
    - GATE decisions
    - Final editing/montage
    - Brand voice validation

L1: Assisted (AI suggests, human approves)
    - Title/thumbnail generation (3 options, 1 chosen)
    - Script drafting (human-reviewed completely)
    - Story scoring
    - Learning proposals

L2: Semi-Automated (Workflow runs with HITL gates)
    - Research, fact-check, outline
    - Visual planning, asset generation
    - SEO optimization
    - Publishing workflow

L3: Fully Automated (no human-in-loop)
    - Analytics collection (only)
    - Archival (only)
    - Metadata logging (only)
```

**Key Rule:** Nothing touching *facts, narrative, or publishing* can run at L3. L2 is max for editorial.

---

## Development Priorities

### Phase 1: Foundation (4–6 months)

**Objectives:**
1. Prove Story-Market Fit (videos are watched, audience returns)
2. Establish baseline metrics for calibration
3. Operationalize 4 HITL gates
4. Document all learnings

**Scope:**
- Build agents in dependency order (Research → Fact-Check → Script → Publish)
- Produce 20–30 test videos manually (agents are assistants)
- Run §V experiments on: hooks, titles, length, posting time
- Extract first 10–15 learning rules

**Success Criteria:**
- Avg Story Completion % trending upward across last 5 videos
- 3+ videos in top tier (85+ score on §I metrics)
- All 4 gates operational (zero bypasses)
- Learning loop producing actionable insights

### Phase 2: Semi-Automation (3–6 months after Phase 1)

**Objectives:**
1. 2x productivity without quality loss
2. Retire manual bottlenecks (research, outline, SEO)
3. Implement automated approval queues

**Scope:**
- Deploy AGT-RES, AGT-FACT, AGT-SCRIPT (Tier 1)
- Build basic Orchestrator + approval dashboard
- Increase production to 2 videos/week
- Calibrate agent prompts (§N) on real output

**Success Criteria:**
- Time-to-publish reduced 40%
- §T checklist pass rate 95%+
- Zero quality regressions in key metrics
- Agents make 80%+ of decisions without human escalation

### Phase 3: Content Operating System (6+ months after Phase 2)

**Objectives:**
1. Fully autonomous discovery-to-publish pipeline
2. Discover, produce, publish without manual intervention
3. Scale to 4–8 videos/week
4. Multi-language/platform capability

**Scope:**
- Deploy all agents (AGT-LRN, multi-platform distribution)
- Implement advanced multi-agent reasoning (agents talk to each other)
- Build feedback loops into content strategy

**Success Criteria:**
- Production rate: 4–8 videos/week, all quality-gated
- Manual overhead: <10% (gates only)
- Revenue positive (§Q)

---

## Technical Stack

### Backend

- **Runtime:** Node.js 18+
- **Framework:** Express.js or Fastify
- **Language:** TypeScript
- **Package Manager:** npm/yarn

### Database

- **Primary:** Supabase (PostgreSQL)
- **ORM:** Drizzle or Raw SQL
- **Migrations:** Flyway or custom
- **RLS:** Enabled for multi-agent safety

### AI / Agents

- **LLM Provider:** OpenAI, Anthropic, or Ollama (provider-agnostic)
- **Orchestration:** Custom Node.js + Bull queues or Temporal
- **Memory:** Supabase (acts as vector store + fact db)
- **Web Search:** Bing Search API (fallback: local RSS)

### External Services

- **YouTube Data API** — Publishing, analytics
- **Image Generation:** DALL-E, Midjourney, or Stable Diffusion
- **TTS:** ElevenLabs or similar (high-quality Arabic)
- **Auth:** Supabase Auth or custom JWT
- **Analytics:** Plausible or Mixpanel

### Deployment

- **Frontend:** Vercel (Next.js or SvelteKit)
- **Backend:** Vercel Functions / Railway / Render
- **Database:** Supabase (hosted)
- **CI/CD:** GitHub Actions

---

## File Structure

```
THE-JOURNEY-AI-MONY/
├── backend/
│   ├── src/
│   │   ├── api/
│   │   │   ├── routes.ts
│   │   │   ├── auth.ts
│   │   │   └── ...
│   │   ├── agents/
│   │   │   ├── orchestrator.ts
│   │   │   ├── research.ts
│   │   │   ├── factcheck.ts
│   │   │   ├── script.ts
│   │   │   └── ...
│   │   ├── database/
│   │   │   ├── schema.ts
│   │   │   ├── queries.ts
│   │   │   └── migrations/
│   │   ├── services/
│   │   │   ├── scoreCard.ts
│   │   │   ├── publishing.ts
│   │   │   └── ...
│   │   ├── middleware/
│   │   └── index.ts
│   ├── package.json
│   └── tsconfig.json
├── database/
│   ├── schema.sql
│   ├── migrations/
│   └── functions/
├── agents/
│   ├── orchestrator-agent/
│   ├── research-agent/
│   └── ...
├── prompts/
│   ├── P-RES-01.yaml
│   └── ...
├── docs/
│   ├── CHANNEL_BRAIN.md (strategic)
│   ├── ARCHITECTURE.md (this file)
│   └── ...
└── .env.example
```

---

## Next Steps

1. **Database:** Implement §M schema in Supabase
2. **API:** Build CRUD routes for ideas, research, scoring
3. **Agents:** Implement AGT-RES (research) and AGT-FACT (fact-check) first
4. **Gates:** Build GATE-1 (story selection) UI and approval workflow
5. **Testing:** Test pipeline end-to-end with manual input

See ROADMAP section in README.md for detailed timeline.
