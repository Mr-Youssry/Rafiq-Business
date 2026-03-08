# RafiqAI Notion Workspace — Architecture Proposal

## Overview

**URL**: `notion.rafiq.ai` (custom domain pointing to Notion workspace)
**Owner (Member)**: Ahmed Youssry
**Guests (Can Edit)**: Shereen Sayed, Nermeen ElKammar, Kelley (UX)
**Guests (Can View + Comment)**: Lucy (Go-Together Coach), Jemma (Go-Together PM), Sherif (Advisory Coach)
**AI Admin**: Claude Code SDK agent (via Notion API + MCP)

---

## Workspace Sidebar Structure

```
📌 RafiqAI HQ (Homepage/Dashboard)
│
├── 🧭 COMMAND CENTER
│   ├── 📋 Task Board
│   ├── 🗺️ Roadmap & Milestones
│   ├── 📅 Meeting Notes
│   └── ⏱️ Timesheets
│
├── 🧠 KNOWLEDGE BASE
│   ├── 📖 What is Rafiq?
│   ├── 🎯 Product
│   ├── 📊 Market Intelligence
│   └── 💡 Decision Log (ADR + Journal)
│
├── 🔬 VALIDATION LAB
│   ├── 🧪 Hypothesis Board
│   └── 📋 Evidence Library
│
├── 🤝 OUTREACH
│   ├── 🏫 Partners & Pilots CRM
│   └── 💰 Investors CRM
│
├── 🏗️ BUILD
│   ├── 🎨 Design (Figma)
│   ├── ⚙️ Technical Architecture
│   └── 🔧 Sprint Log
│
└── 📂 RESOURCES
    ├── 📄 Templates
    ├── 📚 Reference Materials
    └── 🔗 Useful Links
```

---

## Detailed Section Design

---

### 📌 RafiqAI HQ — Homepage Dashboard

A single overview page that everyone lands on. Contains:

| Widget | Source | Purpose |
|--------|--------|---------|
| **This Week's Focus** | Manual text block | What the team is working on this week |
| **Active Tasks** | Linked view of Task Board (filtered: status = In Progress) | At-a-glance work status |
| **Upcoming Milestones** | Linked view of Roadmap (next 3 milestones) | What's coming |
| **Recent Decisions** | Linked view of Decision Journal (last 5) | Stay aligned |
| **Open Hypotheses** | Linked view of Hypothesis Board (status = Testing) | What we're validating now |
| **Quick Links** | Bookmarks | Figma, Website, GitHub, GlassFrog |

---

### 🧭 COMMAND CENTER

#### 📋 Task Board
**Type**: Database (Board view + Table view)

| Property | Type | Purpose |
|----------|------|---------|
| Task | Title | What needs to be done |
| Status | Select | `Backlog` → `To Do` → `In Progress` → `Review` → `Done` |
| Owner | Person | Who's responsible (Ahmed/Shereen/Nermeen/Kelley) |
| Circle | Select | `Co-founders` / `Business Development` / `Marketing-Comms` / `Product Design` / `Build` |
| Priority | Select | `🔴 Urgent` / `🟡 High` / `🟢 Normal` / `⚪ Low` |
| Due Date | Date | Deadline |
| Sprint/Week | Select | Week number or sprint name |
| Source | Relation | → Links to Meeting Notes (where task was assigned) |
| Hypothesis | Relation | → Links to Hypothesis Board (if task is a validation action) |
| Milestone | Relation | → Links to Roadmap milestone |

**Views**:
- **Board**: Kanban by Status (default)
- **My Tasks**: Filtered by Owner = current user
- **By Circle**: Grouped by GlassFrog circle
- **This Week**: Filtered by Sprint/Week = current week
- **Overdue**: Filtered by Due Date < today AND Status ≠ Done

---

#### 🗺️ Roadmap & Milestones
**Type**: Database (Timeline view + Table view)

| Property | Type | Purpose |
|----------|------|---------|
| Milestone | Title | Name of the milestone |
| Quarter | Select | `Q1 2025` through `Q4 2026` |
| Category | Multi-select | `Product` / `Business` / `Market` / `Fundraising` / `Go-Together` / `Legal` |
| Status | Select | `Not Started` → `In Progress` → `Completed` → `Deferred` |
| Start Date | Date | When work begins |
| Target Date | Date | When it should be done |
| Owner | Person | Primary responsible |
| Go-Together Objective | Select | Links to specific GT objectives (2.1, 2.3, 2.4, 2.7, 3.1, 3.2, 1.11) |
| Key Results | Text | What "done" looks like |
| Tasks | Relation | → Links to Task Board |
| Decisions | Relation | → Links to Decision Log |

**Views**:
- **Timeline**: Gantt-style view by date range
- **By Quarter**: Grouped by Quarter
- **Go-Together**: Filtered by Go-Together Objective ≠ empty
- **By Category**: Grouped by Category

---

#### 📅 Meeting Notes
**Type**: Database (List view)

| Property | Type | Purpose |
|----------|------|---------|
| Title | Title | e.g., "Mar 1, 2026 - RafiqAI Tactical" |
| Date | Date | Meeting date |
| Type | Select | `Tactical` / `Go-Together (Lucy)` / `Go-Together (Jemma)` / `Kelley (Design)` / `Sherif (Advisory)` / `Strategy` / `Ad-hoc` |
| Attendees | Multi-select | Who was present |
| Agenda | Text | What was planned to discuss |
| Notes | Text (body) | Full meeting notes |
| Decisions Made | Relation | → Links to Decision Log |
| Action Items | Relation | → Links to Task Board |
| Tags | Multi-select | Topics covered |

**Template**: Each new meeting note auto-includes:
```
## Agenda
-

## Notes
-

## Decisions
-

## Action Items
- [ ] Task — Owner — Due Date
```

---

#### ⏱️ Timesheets
**Type**: Database (Table view)

| Property | Type | Purpose |
|----------|------|---------|
| Entry | Title | Brief description of work |
| Person | Select | `Ahmed` / `Shereen` / `Nermeen` / `Kelley` / `Sherif` |
| Date | Date | When the work was done |
| Hours | Number | Time spent |
| Circle | Select | `Co-founders` / `Business Development` / `Marketing-Comms` / `Product Design` / `Build` |
| Category | Select | `Research` / `Design` / `Development` / `Meeting` / `Admin` / `Content` |
| Notes | Text | Additional context |

**Views**:
- **This Week**: Filtered by current week
- **By Person**: Grouped by Person with sum of Hours
- **By Circle**: Grouped by Circle with sum of Hours
- **Monthly Summary**: Grouped by month

---

### 🧠 KNOWLEDGE BASE

#### 📖 What is Rafiq?
**Type**: Page tree (not a database — these are reference documents)

```
What is Rafiq?
├── Vision & Mission
├── The Problem We Solve
├── Our Solution (How RafiqAI Works)
│   ├── User Journey (Layla's Story)
│   ├── Core Features List
│   ├── Technical Architecture (RAG + Graphiti Memory Bank)
│   └── Human-AI Hybrid Model
├── The Team
│   ├── Ahmed — AI Advocate
│   ├── Shereen — Leadership Advocate
│   ├── Nermeen — Coaching Advocate
│   ├── Kelley — UX Designer
│   └── Sherif — Advisory Coach
├── Coaching Framework
│   ├── Coach-Teacher Agreement
│   ├── Coaching Guidelines
│   ├── Lesson Plan Rubric
│   └── Lesson Plan Validation (AI Training Criteria)
├── User Personas
│   ├── Overwhelmed New Teacher (Private)
│   ├── Overwhelmed New Teacher (Public)
│   ├── Overwhelmed New Teacher (Community)
│   ├── Burnout Veteran
│   ├── NGO Facilitator
│   └── School Librarian
├── User Stories (by role)
│   ├── Teacher Stories
│   ├── Coach Stories
│   └── Education Leader Stories
└── Brand & Communications
    ├── Communication Strategy
    ├── Ramadan Fazoura Campaign
    └── Naming & Branding Notes
```

---

#### 📊 Market Intelligence
**Type**: Page tree + embedded databases

```
Market Intelligence
├── Market Overview Dashboard
│   ├── TAM/SAM/SOM Summary
│   └── Key Market Data Points
├── Country Profiles (sub-pages)
│   ├── 🇪🇬 Egypt
│   ├── 🇶🇦 Qatar
│   ├── 🇸🇦 Saudi Arabia (KSA)
│   ├── 🇺🇸 USA (Georgia)
│   ├── 🇦🇪 UAE (future)
│   ├── 🇬🇧 UK (future)
│   └── 🇹🇷 Turkey (future)
├── Competitive Landscape
│   ├── Edthena
│   ├── Lumivero / Experiential Learning Cloud
│   ├── Schoolmint Grow
│   ├── SupervisionAssist
│   └── Tevera
├── Interview Transcripts & Notes
│   ├── Karen (Coach, Georgia)
│   ├── Melissa (Coach, Georgia)
│   ├── Ahmed JadAllah (Founder, Qatar)
│   ├── Kholoud (CSO Talents, KSA)
│   ├── Amany (Admin, KSA Private School)
│   ├── Gawish (Market Entry Expert)
│   └── [Future interviews]
└── Strategy Questions Bank
    ├── Desirability Questions
    ├── Feasibility Questions
    └── Viability Questions
```

Each **Country Profile** follows a template:
```
## Country: [Name]
### Market Size & Growth
### Education Structure (Public/Private/NGO)
### Teacher Workforce Stats
### Professional Development Landscape
### Coaching Adoption Status
### Regulatory Environment
### Digital Readiness
### Entry Strategy
### Key Contacts & Networks
### Researcher: [Name]
### Status: [Exploring / Validating / Active / Parked]
```

---

#### 💡 Decision Log
**Two linked databases**:

**Database 1: ADR (Architecture Decision Records)**

| Property | Type | Purpose |
|----------|------|---------|
| ADR Title | Title | e.g., "ADR-007: Start in Qatar before KSA" |
| ADR Number | Number | Sequential ID |
| Status | Select | `Proposed` → `Accepted` → `Superseded` → `Deprecated` |
| Date Decided | Date | When the decision was made |
| Context | Text | What situation or problem prompted this decision |
| Decision | Text | What we decided |
| Rationale | Text | Why we chose this over alternatives |
| Alternatives Considered | Text | What else we considered and why we rejected them |
| Consequences | Text | What this decision means going forward |
| Category | Select | `Market` / `Product` / `Technical` / `Business Model` / `Legal` / `Team` / `Fundraising` |
| Decided By | Multi-select | Who made the decision |
| Related Meeting | Relation | → Meeting Notes |
| Related Hypotheses | Relation | → Hypothesis Board |

**Database 2: Decision Journal (lightweight log)**

| Property | Type | Purpose |
|----------|------|---------|
| Decision | Title | Short title |
| Date | Date | When it was made |
| Context | Select | `Tactical Meeting` / `Go-Together Session` / `Strategy Session` / `Ad-hoc` |
| Meeting Link | Relation | → Meeting Notes |
| ADR Link | Relation | → ADR record (for significant decisions) |
| Summary | Text | One-line summary |

---

### 🔬 VALIDATION LAB

#### 🧪 Hypothesis Board
**Type**: Database (Board view + Table view)

| Property | Type | Purpose |
|----------|------|---------|
| Hypothesis | Title | e.g., "H-012: NGOs in Egypt will pay $15/teacher/month" |
| ID | Formula/Number | Sequential: H-001, H-002... |
| Category | Select | `Desirability` / `Feasibility` / `Viability` |
| Sub-category | Select | `Problem-fit` / `Solution-fit` / `Market-fit` / `Channel` / `Revenue` / `Pricing` / `Technical` |
| Status | Select | `Untested` → `Designing` → `Testing` → `Validated ✅` → `Invalidated ❌` → `Pivoted 🔄` |
| Assumption Statement | Text | "We believe that [target customer] will [action] because [reason]" |
| Riskiness | Select | `🔴 Critical` / `🟡 Important` / `🟢 Nice to know` |
| Method | Multi-select | `Interview` / `Survey` / `Prototype Test` / `Landing Page` / `Pilot` / `Desk Research` / `A/B Test` |
| Success Criteria | Text | "We'll know this is true if [measurable outcome]" |
| Evidence | Text | What we actually found |
| Result | Text | Summary of conclusion |
| Related Market | Relation | → Country in Market Intelligence |
| Tasks | Relation | → Task Board (actions needed to run this experiment) |
| Decision | Relation | → ADR (if the result led to a decision) |
| Owner | Person | Who is running this experiment |

**Views**:
- **Experiment Board**: Kanban by Status
- **By Risk**: Sorted by Riskiness (Critical first)
- **By Category**: Grouped by Category
- **Validated**: Filtered to show validated/invalidated only
- **Active Experiments**: Status = Designing or Testing

**Starter hypotheses to seed** (from existing content):
1. H-001: NGOs in Egypt will partner with us for pilot testing (Desirability, Critical)
2. H-002: Teachers will record lessons via mobile in low-bandwidth environments (Feasibility, Critical)
3. H-003: Coaches support 50+ teachers; AI can reduce this ratio meaningfully (Desirability, Critical)
4. H-004: Schools in Qatar will pay for AI coaching tools (Viability, Critical)
5. H-005: Our RAG engine delivers culturally relevant feedback in Arabic (Feasibility, Important)
6. H-006: The buyer is the PD Director, not the individual teacher (Desirability, Critical)
7. H-007: NGOs are the right entry point before schools (Viability, Important)
8. H-008: $15-20/teacher/month is an acceptable price point (Viability, Critical)
9. H-009: Teachers prefer app over platform for coaching tools (Desirability, Important)
10. H-010: Figma prototype achieves >80% task completion in user testing (Feasibility, Important)

---

#### 📋 Evidence Library
**Type**: Database

| Property | Type | Purpose |
|----------|------|---------|
| Evidence | Title | Name/description |
| Type | Select | `Interview Transcript` / `Survey Result` / `Usage Data` / `Research Paper` / `Pilot Report` / `Testimonial` |
| Date Collected | Date | When |
| Source | Text | Who/where |
| Hypothesis | Relation | → Hypothesis Board |
| Summary | Text | Key takeaways |
| File | Files & media | Attachments |

---

### 🤝 OUTREACH

#### 🏫 Partners & Pilots CRM
**Type**: Database

| Property | Type | Purpose |
|----------|------|---------|
| Organization | Title | Name |
| Type | Select | `NGO` / `Private School` / `Public School` / `International School` / `University` / `District` / `Ministry` |
| Country | Select | Country list |
| Contact Person | Text | Primary contact name |
| Contact Email | Email | |
| Contact Phone | Phone | |
| Stage | Select | `Identified` → `Contacted` → `Intro Meeting` → `Interested` → `Pilot Agreement` → `Active Pilot` → `Paying Customer` |
| Relationship Owner | Person | Ahmed/Shereen/Nermeen |
| Introduced By | Text | Connection source |
| Notes | Text | Conversation history |
| Next Action | Text | What to do next |
| Next Action Date | Date | When |
| Last Contact | Date | Last interaction |
| Potential Teachers | Number | Estimated teacher count |
| Tags | Multi-select | `Early Adopter` / `Warm Intro` / `Cold Outreach` / `Go-Together Network` / `Conference` |

**Views**:
- **Pipeline**: Kanban by Stage
- **By Country**: Grouped by Country
- **Follow-up Due**: Filtered by Next Action Date ≤ this week
- **Active Pilots**: Filtered by Stage = Active Pilot

---

#### 💰 Investors CRM
**Type**: Database

| Property | Type | Purpose |
|----------|------|---------|
| Investor | Title | Name of fund/person |
| Type | Select | `VC` / `Angel` / `Accelerator` / `Grant` / `Family Office` / `Government Fund` |
| Stage | Select | `Researched` → `To Contact` → `Contacted` → `Meeting Scheduled` → `Pitched` → `Due Diligence` → `Term Sheet` → `Closed` → `Passed` |
| Focus | Multi-select | `EdTech` / `AI` / `MENA` / `Africa` / `Impact` / `Pre-seed` / `Seed` |
| Geography | Select | Country/region |
| Check Size | Text | Typical investment range |
| Contact Person | Text | |
| Contact Email | Email | |
| Relationship Owner | Person | |
| Introduced By | Text | |
| Notes | Text | |
| Next Action | Text | |
| Next Action Date | Date | |
| Last Contact | Date | |
| Application Deadline | Date | For grants/accelerators |
| Status Notes | Text | |

**Views**:
- **Pipeline**: Kanban by Stage
- **By Type**: Grouped by Type
- **Upcoming Deadlines**: Filtered/sorted by Application Deadline
- **EdTech Focused**: Filtered by Focus contains EdTech

**Seed data**: Import from the 5000 VC spreadsheet + Accelerators & VCs list

---

### 🏗️ BUILD

#### 🎨 Design (Figma)
**Type**: Page (not database)

```
Design Hub
├── 🔗 Figma Prototype Link (embedded)
├── Current Design Status
├── Kelley's Feedback Queue
├── Screen Inventory
│   ├── Dashboard / Homepage
│   ├── Lesson Recording
│   ├── Feedback View
│   ├── Coach Interface
│   ├── Reports
│   └── Settings
├── Design Decisions (links to ADR)
└── Branding Assets
```

---

#### ⚙️ Technical Architecture
**Type**: Page tree

```
Technical Architecture
├── System Overview
├── AI Engine (RAG + Graphiti)
├── Backend (Infrastructure, Auth, Database)
├── Mobile App (Flutter)
├── API Design
└── Data Privacy & Compliance
```

---

#### 🔧 Sprint Log
**Type**: Database

| Property | Type | Purpose |
|----------|------|---------|
| Sprint | Title | e.g., "Sprint 3: Lesson Recording Flow" |
| Start Date | Date | |
| End Date | Date | |
| Goals | Text | What we aim to accomplish |
| Deliverables | Text | What was actually delivered |
| Tasks | Relation | → Task Board |
| Status | Select | `Planning` → `Active` → `Complete` → `Retrospective Done` |
| Retro Notes | Text | What went well / what to improve |

---

## Database Relationships Map

```
Meeting Notes ──→ Task Board ──→ Roadmap & Milestones
     │                │                    │
     ▼                ▼                    │
Decision Journal ──→ ADR ◄────────────────┘
                      │
                      ▼
              Hypothesis Board ──→ Evidence Library
                      │
                      ▼
                  Task Board (validation actions)
                      │
              ┌───────┴───────┐
              ▼               ▼
    Partners CRM      Investors CRM
```

**Key flows**:
1. **Meeting → Task**: Action items from meetings become tasks
2. **Meeting → Decision**: Decisions made in meetings are logged
3. **Hypothesis → Task**: Validation experiments generate tasks
4. **Hypothesis → Evidence**: Test results are stored as evidence
5. **Hypothesis → ADR**: Validated/invalidated hypotheses inform decisions
6. **Decision → Milestone**: Big decisions affect the roadmap
7. **Task → Milestone**: Tasks roll up to milestones

---

## AI Admin Assistant Architecture

### What It Does

**Chatbot Mode** (for team queries):
- "What did we decide about the Qatar market?"
- "Show me all open tasks for Nermeen"
- "What hypotheses are we currently testing?"
- "Summarize last week's meeting notes"
- "What's the status of the Figma prototype?"

**Backend Agent Mode** (automated organization):
- Processes raw meeting notes → extracts action items → creates tasks
- Receives market research inputs → categorizes into country profiles
- Monitors hypothesis board → flags stale experiments
- Generates weekly summary for the team
- Updates CRM follow-up reminders

### Technical Implementation

```
                  ┌─────────────────┐
                  │  notion.rafiq.ai │
                  │  (Custom Domain) │
                  └────────┬────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
    ┌─────────────┐ ┌───────────┐ ┌──────────┐
    │ Notion Pages│ │ Chat Widget│ │ Notion API│
    │ (Content)   │ │ (Frontend) │ │ (Backend) │
    └─────────────┘ └─────┬─────┘ └─────┬────┘
                          │             │
                          ▼             ▼
                   ┌──────────────────────┐
                   │ Claude Agent SDK     │
                   │ (Hosted on DO/Vercel)│
                   │                      │
                   │ - Notion MCP Server  │
                   │ - Query handler      │
                   │ - Content organizer  │
                   │ - Meeting processor  │
                   └──────────────────────┘
```

**Components**:
1. **Notion Workspace** — The source of truth for all content
2. **Notion API Integration** — Read/write access via internal integration token
3. **Claude Agent SDK** — Backend agent that processes queries and automates organization
4. **Chat Widget** — Embedded on notion.rafiq.ai for team interaction
5. **Hosting** — Can run on your existing DigitalOcean droplet or Vercel

---

## Migration Plan (from Google Drive)

| Google Drive Folder | → Notion Destination |
|---------------------|---------------------|
| Communications/ | Knowledge Base → Brand & Communications |
| Market Research/ | Knowledge Base → Market Intelligence → Interview Notes |
| Pitch-decks/ | Resources → Reference Materials (+ links in Investor CRM) |
| Investment/ | Investors CRM (seed data) + Resources |
| Go-Together Accelerator/ | Roadmap (objectives), Meeting Notes (coach sessions), Knowledge Base (questionnaires) |
| Product/ | Knowledge Base → Product (personas, user stories, agreements) |
| Management & Planning/ | Meeting Notes + Roadmap + Task Board (action items) |
| Time Sheets/ | Timesheets database |
| Resources/ | Resources → Reference Materials |

---

## Access Control Summary

| Person | Role | Access Level | Can See |
|--------|------|-------------|---------|
| Ahmed | Owner/Member | Full Access | Everything |
| Shereen | Guest | Can Edit | All shared pages |
| Nermeen | Guest | Can Edit | All shared pages |
| Kelley | Guest | Can Edit | Design, Task Board, Meeting Notes |
| Lucy | Guest | Can View + Comment | Roadmap, Meeting Notes, Hypothesis Board |
| Jemma | Guest | Can View + Comment | Roadmap, Meeting Notes |
| Sherif | Guest | Can View + Comment | Build section, Task Board, Meeting Notes |
| Claude Agent | API | Full Read/Write | Everything (via integration token) |

---

## Estimated Block Usage

| Section | Estimated Blocks | Notes |
|---------|-----------------|-------|
| Homepage | ~20 | Dashboard widgets |
| Task Board | ~200 | 100 initial tasks × 2 blocks each |
| Roadmap | ~60 | 30 milestones |
| Meeting Notes | ~500 | 50 meetings × 10 blocks avg |
| Knowledge Base | ~400 | Static content pages |
| Hypothesis Board | ~100 | 50 hypotheses |
| Partner CRM | ~200 | 100 contacts |
| Investor CRM | ~300 | 150 contacts from spreadsheets |
| Decision Log | ~100 | 50 decisions |
| Build section | ~50 | Mostly links |
| **Total estimate** | **~1,930** | **Solo plan = unlimited** |

Since you're the sole member, this is **unlimited blocks** — no concerns.

---

## Next Steps

1. **Create the Notion workspace** and set up the custom domain
2. **Build the database schemas** (Task Board, Hypothesis Board, CRM, etc.)
3. **Migrate Google Drive content** into the Knowledge Base
4. **Seed initial data** (hypotheses, milestones from First Year Plan, CRM contacts from spreadsheets)
5. **Set up the Notion API integration** for Claude access
6. **Build the Claude Agent SDK** backend for the AI admin assistant
7. **Invite team members** as guests with appropriate permissions
8. **Train the team** on the workspace structure
