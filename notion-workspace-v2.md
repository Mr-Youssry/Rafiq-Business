# RafiqAI Notion Workspace — Architecture v2

## The Framework

**Four Pillars + Governance Layer**

```
             🔄 HOLACRACY
        (Governance & Structure)
       ┌────────────────────────┐
       │ Circles → Roles →     │
       │ Accountabilities       │
       └──┬──────┬──────┬─────┬┘
          │      │      │     │
       ⚡      📜     👥    📖
     ACTION  LOGBOOK  NET   WIKI
     (Future) (Past) (People) (Knowledge)
```

**URL**: `notion.rafiq.ai`
**Owner (Member)**: Ahmed Youssry
**Guests (Can Edit)**: Shereen, Nermeen, Kelley
**Guests (Can View + Comment)**: Lucy, Jemma, Sherif
**AI Admin**: Claude Agent SDK (via Notion API)

---

## Notion Sidebar

```
📥 Inbox

🔄 Holacracy
   ├── Circles & Roles
   └── Governance Log (view from Logbook)

⚡ Action System
   ├── Projects
   ├── Tasks (Next Actions)
   ├── Calendar
   ├── Waiting For
   └── Someday / Maybe

📜 Logbook
   ├── Timeline (all entries)
   ├── Meetings
   ├── Decisions (ADR)
   ├── Financials
   ├── Metrics
   ├── Milestones
   └── Experiments

👥 Network
   ├── All Contacts
   ├── Partners Pipeline
   ├── Investors Pipeline
   ├── Advisors & Experts
   └── Follow-up Due

📖 Wiki
   ├── About Rafiq
   ├── Product
   ├── Markets
   ├── Validation
   ├── Communications
   ├── Fundraising
   └── Operations
```

---

# 🔄 HOLACRACY — Governance Layer

Holacracy defines WHO does WHAT. It's not just documentation —
it's the operating system that governs how work flows through all four pillars.

In GTD terms, this is **Horizon 2: Areas of Focus & Accountabilities** —
the layer between individual tasks and strategic goals.

## How Holacracy Connects to the Pillars

| Pillar | Holacracy Connection |
|--------|---------------------|
| **Action** | Each project belongs to a circle. Each task has a role owner. |
| **Logbook** | Governance changes are logged. Tactical meetings produce log entries. |
| **Network** | Each contact has a relationship owner (a role). |
| **Wiki** | Each knowledge domain is maintained by a circle. |

## Database: Circles

| Property | Type | Purpose |
|----------|------|---------|
| Circle | Title | Name of the circle |
| Purpose | Text | Why this circle exists |
| Domain | Text | What this circle has authority over |
| Lead Link | Person | Who leads this circle |
| Members | Multi-select | People in this circle |
| Policies | Text | Operating rules |
| Projects | Relation | → Active projects owned by this circle |

### RafiqAI Circles (Current)

| Circle | Purpose | Lead Link | Members |
|--------|---------|-----------|---------|
| **Co-founders** | Strategic direction, vision, and high-level decisions | Shereen | Ahmed, Shereen, Nermeen |
| **Business Development** | Market entry, revenue strategy, partnerships, fundraising | Shereen | Shereen, Nermeen |
| **Marketing & Communications** | Brand presence, content, outreach, storytelling | Nermeen | Shereen, Nermeen |
| **Product Design** | User experience, personas, coaching model, user stories | Shereen | Shereen, Nermeen, Kelley |
| **Build** | Technical architecture, AI engine, mobile app, infrastructure | Ahmed | Ahmed, Sherif |

## Database: Roles

| Property | Type | Purpose |
|----------|------|---------|
| Role | Title | Name of the role |
| Circle | Relation | → Which circle this role belongs to |
| Purpose | Text | What this role exists to do |
| Accountabilities | Text | Specific recurring actions this role is expected to perform |
| Person | Select | Who currently fills this role |

### RafiqAI Roles (Current)

| Role | Circle | Purpose | Accountabilities | Person |
|------|--------|---------|------------------|--------|
| AI Advocate | Co-founders | Define the technological art of the possible | Pressure-test concepts against AI capabilities; ensure technical feasibility and scalability | Ahmed |
| Coaching Advocate | Co-founders | Champion the needs of coaches and teachers | Guarantee product is genuinely useful; represent end-user workflows and adoption barriers | Nermeen |
| Leadership Advocate | Co-founders | Represent the customer and beneficiary | Ensure measurable ROI for buyers (schools, districts, ministries); lead strategy | Shereen |
| Market Researcher | Business Dev | Understand target markets deeply | Conduct interviews; compile country profiles; validate assumptions | Nermeen, Shereen |
| Investor Relations | Business Dev | Build relationships with funders | Maintain investor CRM; prepare pitch materials; track applications | Shereen |
| Content Creator | Marketing & Comms | Produce content that tells Rafiq's story | Write LinkedIn posts; manage content calendar; develop campaigns | Nermeen, Shereen |
| UX Designer | Product Design | Design intuitive, accessible user experiences | Create Figma prototypes; conduct user testing; iterate on feedback | Kelley |
| Product Architect | Product Design | Define what the product does and why | Write user stories; define coaching model; design reports | Shereen, Nermeen |
| Tech Lead | Build | Build and maintain the technical platform | Develop MVP; manage infrastructure; integrate AI models | Ahmed |
| Advisory Coach | Build | Guide technical decisions and accelerate development | Review architecture; provide expertise; support sprint planning | Sherif |

## Governance Evolution

When roles or circles change (new role created, accountability shifted, circle split),
this is logged in the **Logbook** as a `Type = Governance` entry. This creates an
auditable trail of how your organizational structure evolved over time.

Holacracy governance questions to revisit monthly:
- Is each circle's purpose still accurate?
- Are accountabilities assigned to the right roles?
- Do we need a new role or circle?
- Are there tensions (gaps between current reality and what's needed)?

---

# 📥 INBOX

**Type**: Simple Notion page with a database view

The Inbox is a zero-friction capture point. Anything can go here:
- An idea from a conversation
- A contact someone mentioned
- A task from an email
- A research finding
- A financial receipt

**Rules**:
1. **Capture immediately** — don't try to organize in the moment
2. **Process weekly** — every Sunday, move each item to the right place:
   - If it's actionable → **Action System** (project or task)
   - If it's a person/org → **Network**
   - If it's knowledge → **Wiki**
   - If it's a record of something that happened → **Logbook**
   - If it takes < 2 minutes → do it now, then log it
3. **Inbox Zero** — the inbox should be empty after weekly review

### Inbox Database

| Property | Type | Purpose |
|----------|------|---------|
| Item | Title | What was captured |
| Captured By | Select | Ahmed / Shereen / Nermeen / Kelley / Claude |
| Date | Date | When it was captured |
| Source | Select | `Meeting` / `Email` / `Conversation` / `Research` / `Idea` / `AI Generated` |
| Processed | Checkbox | Has this been sorted? |
| Moved To | Select | Where it went: `Action` / `Logbook` / `Network` / `Wiki` / `Done` |
| Notes | Text | Additional context |

---

# ⚡ ACTION SYSTEM — The Future

Everything that needs to happen. GTD-powered.

## Database: Projects

A project is any outcome that requires more than one action step and has a clear "done" state.

| Property | Type | Purpose |
|----------|------|---------|
| Project | Title | Name of the project |
| Status | Select | `Active` / `On Hold` / `Completed` / `Dropped` |
| Circle | Relation | → Holacracy circle that owns this |
| Owner | Person | Primary responsible person |
| Goal | Text | What "done" looks like — one sentence |
| Start Date | Date | When work began |
| Target Date | Date | When it should be done |
| GT Objective | Select | `2.1` / `2.3` / `2.4` / `2.7` / `3.1` / `3.2` / `1.11` / `None` |
| Priority | Select | `P1 Must Do` / `P2 Should Do` / `P3 Nice to Have` |
| Tasks | Relation | → Tasks in this project |
| Log Entries | Relation | → Logbook entries related to this project |
| Hypotheses | Relation | → Wiki: Hypothesis Board entries this project validates |
| Notes | Text | Context, background, links |

### Views
- **Active Projects**: Status = Active, sorted by Priority
- **By Circle**: Grouped by Circle
- **Go-Together**: Filtered by GT Objective ≠ None
- **Completed**: Status = Completed (archive)

### Starter Projects (from existing content)

| Project | Circle | GT Objective | Priority |
|---------|--------|-------------|----------|
| Validate customer problem (15+ interviews) | Business Dev | 2.1 | P1 |
| Define market size & beachhead segment | Business Dev | 2.4 | P1 |
| Define & test value proposition | Business Dev | 2.3 | P2 |
| Test willingness to pay signals | Business Dev | 2.7 | P2 |
| Complete Figma prototype v2 | Product Design | 3.1 | P1 |
| Conduct prototype user testing (10 users) | Product Design | 3.1 | P1 |
| Set up basic product systems | Build | 3.2 | P2 |
| Establish legal foundations (jurisdiction) | Co-founders | 1.11 | P2 |
| Apply to WISE Testbed | Business Dev | None | P2 |
| Launch Ramadan Fazoura campaign | Marketing & Comms | None | P1 |
| Build investor pipeline (seed from 5000 VC list) | Business Dev | None | P2 |
| Validate Qatar as pilot market | Business Dev | 2.1 | P1 |
| Prepare pre-seed fundraising documents | Co-founders | None | P2 |
| Develop LinkedIn content strategy | Marketing & Comms | None | P2 |

## Database: Tasks (Next Actions)

Single, concrete actions. No task should be vague — it should start with a verb.

| Property | Type | Purpose |
|----------|------|---------|
| Task | Title | What to do (starts with a verb) |
| Status | Select | `Next Action` / `In Progress` / `Done` / `Waiting For` |
| Project | Relation | → Parent project |
| Owner | Person | Who's doing it |
| Context | Multi-select | `@call` / `@write` / `@research` / `@build` / `@review` / `@design` / `@meeting` |
| Priority | Select | `P1` / `P2` / `P3` |
| Due Date | Date | Deadline (if any) |
| Circle | Relation | → Holacracy circle |
| Waiting On | Text | If status = Waiting For, who/what are we waiting on? |
| Source Log | Relation | → Logbook entry that generated this task (e.g., a meeting) |
| Notes | Text | Context |

### Views
- **Next Actions**: Status = Next Action, grouped by Context (GTD-style)
- **My Tasks**: Filtered by Owner = me
- **By Project**: Grouped by Project
- **Waiting For**: Status = Waiting For
- **This Week**: Due Date within 7 days
- **Overdue**: Due Date < today AND Status ≠ Done

## Calendar

Not a separate database — it's a **Calendar view** on the Tasks database (filtered by Due Date ≠ empty)
plus links to external calendars:
- Team meeting schedule (Tactical: Thursdays)
- Go-Together sessions (Lucy, Jemma)
- Kelley design syncs
- Content posting schedule (from Wiki > Communications > Content Calendar)
- Application deadlines
- Conference dates

## Someday / Maybe

A simple database or page for ideas parked for later:
- Explore Australia market
- Build teacher community forum
- Podcast series on AI in education
- Y Combinator application
- Academic paper on Rafiq methodology
- Apply to Sky Deck Accelerator
- Partnership with Teach For All network

---

# 📜 LOGBOOK — The Past

A single unified database that records everything significant that happened.
Every entry is timestamped. The entire history of RafiqAI lives here.

## Database: Log

| Property | Type | Purpose |
|----------|------|---------|
| Entry | Title | What happened |
| Date | Date | When |
| Type | Select | `Meeting` / `Decision` / `Financial` / `Metric` / `Milestone` / `Experiment` / `Governance` / `Communication` |
| Tags | Multi-select | `market` / `product` / `legal` / `fundraising` / `coaching` / `design` / `comms` / `team` |
| Circle | Relation | → Holacracy circle |
| People | Multi-select | Who was involved |
| Project | Relation | → Action System project |
| Tasks Generated | Relation | → Tasks created from this entry |
| Body | Rich text | The full content |
| Attachments | Files | Supporting documents |

## Log Entry Types — Detail

### Type: Meeting

Standard fields plus:
- **Meeting Type**: `Tactical` / `Go-Together (Lucy)` / `Go-Together (Jemma)` / `Design (Kelley)` / `Advisory (Sherif)` / `Strategy` / `External` / `Governance`
- **Attendees**: from People field
- **Agenda**: in Body
- **Notes**: in Body
- **Action Items**: auto-linked to Tasks Generated

**Template for Meeting entries:**
```
## Agenda
-

## Notes
-

## Decisions Made
- [Link to Decision entry]

## Action Items
- [ ] Task → Owner → Due Date

## Key Takeaways
-
```

### Type: Decision (ADR)

Standard fields plus structured body:

```
## ADR-[Number]: [Decision Title]

### Status
Proposed / Accepted / Superseded / Deprecated

### Context
What situation or problem prompted this decision?

### Decision
What we decided.

### Rationale
Why we chose this over alternatives.

### Alternatives Considered
What else we considered and why we rejected them.

### Consequences
What this decision means going forward.
Impact on other decisions, projects, or plans.

### Supersedes
[Link to previous ADR if this replaces one]
```

### Type: Financial

Standard fields plus:
- **Amount**: Number (positive = income, negative = expense)
- **Currency**: Select (USD / EGP / GBP / QAR)
- **Category**: Select
  - `Team Compensation`
  - `UX Design`
  - `Development`
  - `Infrastructure (hosting, tools)`
  - `Travel & Events`
  - `Marketing`
  - `Legal & Registration`
  - `Pilot Costs`
  - `Grant Income`
  - `Investment Income`
- **Paid By**: Select (Ahmed / Shereen / Nermeen / Company)
- **Receipt**: Files

**Views for Financial entries:**
- **Ledger**: All financial entries sorted by date
- **By Category**: Grouped with sum of Amount
- **By Person**: Who paid what (for co-founder reimbursement)
- **Monthly Summary**: Grouped by month
- **Runway Tracker**: Running balance

### Type: Metric

A point-in-time measurement of something you're tracking.

Standard fields plus:
- **Metric Name**: Select
  - `Interviews Completed`
  - `Prototype Test Sessions`
  - `LinkedIn Followers`
  - `Website Visitors`
  - `Content Posts Published`
  - `Investor Contacts Made`
  - `Partner Pipeline Count`
  - `Tasks Completed`
  - `Hours Logged (Team)`
  - `GT Objective Progress %`
  - `Hypothesis Validated Count`
  - `NPS / Feedback Score`
- **Value**: Number
- **Previous Value**: Number (for tracking change)
- **Target**: Number
- **Period**: Select (`Weekly` / `Monthly` / `Quarterly`)

**Views for Metric entries:**
- **Dashboard**: Latest value per Metric Name
- **Trends**: Metric Name grouped, sorted by date (shows progression)
- **vs Target**: Value vs Target comparison

### Type: Milestone

Standard fields plus:
- **Milestone Name**: e.g., "First 15 customer interviews completed"
- **Achievement Date**: Date
- **Evidence**: Text (how we know this was achieved)
- **Project**: Relation (which project this milestone belongs to)
- **GT Objective**: Select (if applicable)

Milestones are celebrations. They mark meaningful progress.

### Type: Experiment

Standard fields plus:
- **Hypothesis**: Relation → Wiki: Hypothesis Board
- **Method**: Text (what we actually did)
- **Result**: Select (`Validated` / `Invalidated` / `Inconclusive`)
- **Evidence**: Text
- **Next Step**: Text (what we do with this learning)

### Type: Governance

Standard fields plus:
- **Change**: Text (what changed in the organizational structure)
- **Affected Circle**: Relation → Circles
- **Affected Role**: Text
- **Reason**: Text (why this change was needed — the tension)

### Type: Communication

Standard fields plus:
- **Channel**: Select (`LinkedIn` / `Email` / `Newsletter` / `Blog` / `Podcast` / `Event` / `Website`)
- **Content Title**: Text
- **Link**: URL
- **Engagement**: Text (likes, comments, shares — if tracked)

## Logbook Views Summary

| View Name | Filter | Use Case |
|-----------|--------|----------|
| **Full Timeline** | All, sorted by Date desc | See everything chronologically |
| **Meetings** | Type = Meeting | Review past meetings |
| **Decisions** | Type = Decision | Decision audit trail |
| **Financial Ledger** | Type = Financial | Money in/out tracking |
| **Metrics Dashboard** | Type = Metric | KPI tracking |
| **Milestones** | Type = Milestone | Progress celebrations |
| **Experiments** | Type = Experiment | What we tested and learned |
| **Governance** | Type = Governance | Org structure changes |
| **Communications** | Type = Communication | Content tracking |
| **This Month** | Date = this month | Monthly review |
| **By Project** | Grouped by Project | Project history |
| **By Circle** | Grouped by Circle | Circle activity |

---

# 👥 NETWORK — The People

One database for every person and organization RafiqAI interacts with.
Different views separate the different relationship types.

## Database: Contacts

| Property | Type | Purpose |
|----------|------|---------|
| Name | Title | Person or organization name |
| Type | Select | `Partner` / `Investor` / `Advisor` / `Pilot School` / `Expert` / `Team` / `Accelerator` / `Grant` / `Conference` |
| Sub-type | Select | Partners: `NGO` / `Private School` / `Public School` / `International School` / `University` / `District` / `Ministry`. Investors: `VC` / `Angel` / `Family Office` / `Government Fund` |
| Organization | Text | Where they work |
| Role/Title | Text | Their position |
| Country | Select | Location |
| Stage | Select | See pipeline stages below |
| Email | Email | |
| Phone | Phone | |
| LinkedIn | URL | |
| Introduced By | Text | How we connected |
| Owner | Select | Ahmed / Shereen / Nermeen (relationship owner — mapped to Holacracy role) |
| Last Contact | Date | When we last interacted |
| Next Action | Text | What to do next |
| Next Action Date | Date | When |
| Potential Value | Text | Why this relationship matters |
| Log Entries | Relation | → Logbook entries involving this contact |
| Tags | Multi-select | `Early Adopter` / `Warm Intro` / `Cold` / `Go-Together Network` / `Conference` / `Harvard Network` / `UGA Network` / `AUC Network` |
| Notes | Rich text | Relationship history and context |

### Pipeline Stages

**Partners & Pilots:**
`Identified` → `Contacted` → `Intro Meeting` → `Interested` → `Pilot Agreement` → `Active Pilot` → `Paying Customer`

**Investors:**
`Researched` → `To Contact` → `Contacted` → `Meeting Scheduled` → `Pitched` → `Due Diligence` → `Term Sheet` → `Closed` → `Passed`

**Grants & Accelerators:**
`Identified` → `Preparing Application` → `Submitted` → `Interview` → `Accepted` → `Active` → `Completed` → `Rejected`

### Views

| View | Filter | Purpose |
|------|--------|---------|
| **All Contacts** | None | Full directory, searchable |
| **Partners Pipeline** | Type = Partner/Pilot School | Kanban by Stage |
| **Investors Pipeline** | Type = Investor | Kanban by Stage |
| **Grants & Accelerators** | Type = Accelerator/Grant | Kanban by Stage |
| **Advisors** | Type = Advisor/Expert | List view |
| **Team** | Type = Team | Current team |
| **Follow-up Due** | Next Action Date ≤ this week | What needs attention NOW |
| **By Country** | Grouped by Country | For market-specific networking |
| **Warm Intros** | Tags contains "Warm Intro" | Prioritized outreach |
| **Conferences** | Type = Conference | Events to attend |

### Seed Data Sources

| Source | Records | Destination |
|--------|---------|-------------|
| 5000 VC spreadsheet | ~5000 | Investors (Type = VC/Angel/etc.) |
| Accelerators & VCs xlsx | ~100 | Investors + Accelerators |
| UK Venture Capital xlsx | ~50 | Investors |
| Nermeen's KSA/Qatar contacts | ~10 | Partners + Experts |
| Shereen's Georgia contacts | ~5 | Partners + Experts |
| Go-Together network | ~20 | Partners + Advisors |

---

# 📖 WIKI — The Knowledge

Static knowledge with temporal marks. Every page carries:
- **Last Updated**: date
- **Updated By**: person
- **Version**: number (for major documents)

## Wiki Structure

```
📖 Wiki
│
├── About Rafiq
│   ├── Vision & Mission
│   ├── The Problem We Solve
│   ├── Our Solution (How RafiqAI Works)
│   ├── Brand Identity & Naming
│   └── Elevator Pitches (short/medium/long)
│
├── Product
│   ├── User Personas
│   │   ├── Overwhelmed New Teacher (Private)
│   │   ├── Overwhelmed New Teacher (Public)
│   │   ├── Overwhelmed New Teacher (Community)
│   │   ├── Burnout Veteran (Private)
│   │   ├── NGO Facilitator
│   │   └── School Librarian
│   ├── Buyer Personas
│   │   ├── PD Director (District)
│   │   ├── NGO Program Manager
│   │   ├── School Principal (Private)
│   │   └── Ministry Official
│   ├── User Stories (by role: Teacher / Coach / Leader)
│   ├── User Journey (Layla's Story)
│   ├── Feature Inventory (40+ features)
│   ├── Coaching Framework
│   │   ├── Coaching Model (Journey → Community → Reports)
│   │   ├── Coach-Teacher Agreement
│   │   ├── Coaching Guidelines
│   │   ├── Lesson Plan Rubric
│   │   ├── Lesson Plan Validation (AI Training Criteria)
│   │   └── Observation Tools
│   ├── Technical Architecture
│   │   ├── System Overview
│   │   ├── RAG + Graphiti Memory Bank
│   │   ├── Data Privacy Matrix (Private/Shared/School/District)
│   │   ├── Performance Report Design
│   │   └── API & Integration Design
│   ├── Design Hub
│   │   ├── Figma Prototype (embedded link)
│   │   ├── Screen Inventory
│   │   └── Design Decisions (links to ADR in Logbook)
│   └── Competitor Analysis
│       ├── Edthena
│       ├── Schoolmint Grow
│       ├── Tevera
│       ├── SupervisionAssist
│       ├── Lumivero
│       └── Comparison Matrix
│
├── Markets
│   ├── Research Guide (framework & questions)
│   ├── Market Comparison Matrix (scored)
│   ├── Country Profiles
│   │   ├── 🇪🇬 Egypt
│   │   │   ├── Legal & Regulatory
│   │   │   ├── Desirability
│   │   │   ├── Feasibility
│   │   │   ├── Viability
│   │   │   ├── Validation
│   │   │   ├── Ecosystem
│   │   │   └── Sources
│   │   ├── 🇶🇦 Qatar (same structure)
│   │   ├── 🇸🇦 Saudi Arabia
│   │   ├── 🇺🇸 USA / Georgia
│   │   ├── 🇦🇪 UAE
│   │   ├── 🇬🇧 UK
│   │   └── 🇹🇷 Turkey
│   ├── Interview Protocols & Scripts
│   │   ├── Coach Interview Script
│   │   ├── Principal Interview Script
│   │   ├── PD Director Interview Script
│   │   ├── Buyer Validation Script
│   │   └── Founder/Market Expert Script
│   └── MENA EdTech TestBed Organizations
│
├── Validation
│   ├── Hypothesis Board (database)
│   ├── Evidence Library (database)
│   ├── Go-Together Objectives
│   │   ├── 2.1 Validate Customer Problem (10 actions)
│   │   ├── 2.3 Define Value Proposition (10 actions)
│   │   ├── 2.4 Market Size & Segment (10 actions)
│   │   ├── 2.7 Willingness to Pay (10 actions)
│   │   ├── 3.1 First Users / Prototype (10 actions)
│   │   ├── 3.2 Basic Product Systems (10 actions)
│   │   └── 1.11 Legal Foundations (10 actions)
│   ├── Logical Framework
│   └── 5-Month Execution Plan
│
├── Communications
│   ├── Communication Strategy & Plan
│   ├── Content Calendar (database)
│   │   ├── Properties: Title, Channel, Date, Author, Status, Theme, Link
│   │   ├── Channels: LinkedIn / Blog / Newsletter / Podcast / Event
│   │   ├── View: Calendar by Date
│   │   ├── View: By Channel
│   │   └── View: Content Pipeline (Kanban: Idea → Drafting → Review → Published)
│   ├── LinkedIn Strategy
│   │   ├── Posting frequency & timing
│   │   ├── Personal vs Brand posts
│   │   ├── Theme rotation
│   │   └── Hashtag strategy
│   ├── Ramadan Fazoura Campaign (full content)
│   ├── Podcast Planning (Post-Eid)
│   └── Testimonials & Success Stories
│       ├── Jamila's Story
│       ├── Rim's Story
│       ├── Hana's Story
│       └── UGA Prototype Feedback
│
├── Fundraising
│   ├── Pitch Deck Versions (V1, V2, ...)
│   ├── Financial Model & Budget Breakdown
│   ├── Applications & Grants Tracker
│   │   ├── WISE Testbed
│   │   ├── Women Founders Grant
│   │   ├── Sky Deck Accelerator
│   │   ├── Concept Ventures
│   │   └── [Future applications]
│   ├── Investor FAQ / Objection Handling
│   └── Go-Together Application (complete)
│
└── Operations
    ├── Meeting Templates
    │   ├── Tactical Meeting Template
    │   ├── Go-Together Session Template
    │   ├── Design Review Template
    │   └── Weekly Progress Report Template
    ├── Onboarding Guide
    ├── Tools & Links
    │   ├── Figma
    │   ├── GitHub
    │   ├── GlassFrog
    │   ├── Website (rafiq-ai.com)
    │   └── Notion API
    └── Risk Register
        ├── Technical: Graphiti maturity, Arabic AI, data collection
        ├── Market: regulatory changes, competition
        ├── Team: part-time availability, key-person risk
        └── Financial: runway, funding timeline
```

### Hypothesis Board (Database in Wiki > Validation)

| Property | Type | Purpose |
|----------|------|---------|
| Hypothesis | Title | e.g., "H-012: NGOs in Egypt will pay $15/teacher/month" |
| ID | Text | Sequential: H-001, H-002... |
| Category | Select | `Desirability` / `Feasibility` / `Viability` |
| Sub-category | Select | `Problem-fit` / `Solution-fit` / `Market-fit` / `Channel` / `Revenue` / `Pricing` / `Technical` |
| Status | Select | `Untested` → `Designing` → `Testing` → `Validated` → `Invalidated` → `Pivoted` |
| Statement | Text | "We believe that [customer] will [action] because [reason]" |
| Riskiness | Select | `Critical` / `Important` / `Nice to know` |
| Method | Multi-select | `Interview` / `Survey` / `Prototype Test` / `Landing Page` / `Pilot` / `Desk Research` |
| Success Criteria | Text | "We'll know this is true if [measurable outcome]" |
| Evidence | Text | What we found |
| Result | Text | Conclusion |
| Market | Select | Country this applies to |
| Project | Relation | → Action System project |
| Log Entry | Relation | → Logbook experiment entries |
| Owner | Person | Who runs this experiment |

**Views**:
- **Experiment Board**: Kanban by Status
- **By Risk**: Critical first
- **By Category**: Grouped
- **Scorecard**: Validated vs Invalidated counts

### Content Calendar (Database in Wiki > Communications)

| Property | Type | Purpose |
|----------|------|---------|
| Title | Title | Content piece name |
| Channel | Select | `LinkedIn (Personal)` / `LinkedIn (Brand)` / `Blog` / `Newsletter` / `Podcast` / `Event` |
| Publish Date | Date | When to publish |
| Author | Select | `Shereen` / `Nermeen` / `Ahmed` / `Rafiq Brand` |
| Status | Select | `Idea` → `Drafting` → `Review` → `Scheduled` → `Published` |
| Theme | Select | `Coaching` / `AI in Education` / `Our Journey` / `EdTech` / `Teachers` / `Product Update` / `Research Insight` |
| Content | Text | Draft or final content |
| Link | URL | Published URL |
| Engagement | Text | Metrics after publishing |
| Campaign | Select | `Ramadan Fazoura` / `Thought Leadership` / `Product Launch` / `General` |

**Views**:
- **Calendar**: By Publish Date
- **Pipeline**: Kanban by Status
- **By Author**: Who's writing what
- **By Channel**: What goes where

---

# Database Relationships Map

```
                    ┌─────────────┐
                    │ 🔄 CIRCLES  │
                    │ & ROLES     │
                    └──────┬──────┘
                           │ governs
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
   ┌─────────────┐  ┌───────────┐   ┌────────────┐
   │ ⚡ PROJECTS  │  │ 📜 LOG     │   │ 👥 CONTACTS│
   │             │  │           │   │            │
   │  has many   │  │ records   │   │  involved  │
   │     ▼       │  │           │   │  in logs   │
   │ ⚡ TASKS    │◄─┤ generates │   │            │
   │             │  │ tasks     │   │            │
   └──────┬──────┘  └─────┬─────┘   └──────┬─────┘
          │               │                │
          │     ┌─────────┼────────┐       │
          │     ▼         ▼        ▼       │
          │  Meetings  Decisions  Metrics   │
          │  Financial Milestones Experiments│
          │  Governance Communications     │
          │               │                │
          └───────────────┼────────────────┘
                          │
                   ┌──────▼──────┐
                   │ 📖 WIKI     │
                   │             │
                   │ Hypotheses  │
                   │ Evidence    │
                   │ Content Cal │
                   │ (databases) │
                   │             │
                   │ Everything  │
                   │ else (pages)│
                   └─────────────┘
```

**Key flows**:
1. **Inbox → Everywhere**: Raw capture gets sorted into the right pillar
2. **Log (Meeting) → Tasks**: Action items from meetings become tasks
3. **Log (Decision) → Wiki**: Decisions update knowledge pages
4. **Log (Experiment) → Wiki (Hypothesis)**: Results update hypothesis status
5. **Log (Financial) → Metrics**: Spending feeds into runway tracking
6. **Tasks → Log (Milestone)**: Completing key tasks triggers milestone entries
7. **Network → Log**: Every significant interaction with a contact gets logged
8. **Circles → Projects → Tasks**: Organizational structure governs work assignment
9. **Wiki (Content Calendar) → Log (Communication)**: Published content gets logged

---

# AI Admin Assistant

## What It Does

### Chatbot Mode (Team queries)
- "What did we decide about the Qatar market?" → Searches Logbook (Type = Decision)
- "Show me all open tasks for Nermeen" → Queries Action System
- "What hypotheses are we currently testing?" → Queries Wiki > Hypothesis Board
- "How much have we spent this month?" → Queries Logbook (Type = Financial)
- "What's our next meeting with Lucy?" → Queries Calendar
- "Summarize our progress on Objective 2.1" → Cross-references GT Objectives + Logbook

### Backend Agent Mode (Automated)
- **Meeting Processor**: Raw meeting notes → extracts action items → creates Tasks + Decisions + Log entry
- **Inbox Sorter**: Suggests where inbox items should go
- **Follow-up Monitor**: Alerts when Network contacts are overdue for follow-up
- **Hypothesis Staleness**: Flags hypotheses stuck in "Testing" for > 2 weeks
- **Weekly Digest**: Generates a summary: tasks completed, decisions made, metrics changed, milestones hit
- **Content Reminder**: Alerts when content calendar items are due for drafting

### Technical Stack
```
┌─────────────────────┐
│  notion.rafiq.ai    │
│  (Custom Domain)    │
└──────────┬──────────┘
           │
    ┌──────┴──────┐
    │ Chat Widget │ ← Team asks questions
    └──────┬──────┘
           │
    ┌──────┴──────────────┐
    │ Claude Agent SDK    │
    │ + Notion MCP Server │
    │                     │
    │ Hosted on:          │
    │ DigitalOcean /      │
    │ Vercel              │
    └─────────────────────┘
```

---

# Access Control

| Person | Role | Access | Sees |
|--------|------|--------|------|
| Ahmed | Owner/Member | Full Access | Everything |
| Shereen | Guest | Can Edit | All shared pages |
| Nermeen | Guest | Can Edit | All shared pages |
| Kelley | Guest | Can Edit | Product, Design, Tasks, Meeting Notes |
| Lucy | Guest | Can Comment | Logbook, Projects, Validation, Wiki |
| Jemma | Guest | Can Comment | Logbook, Projects, Validation |
| Sherif | Guest | Can Comment | Build, Tasks, Logbook |
| Claude | API | Read/Write | Everything (via integration token) |

---

# Migration Plan

| Source | Destination |
|--------|------------|
| **Google Drive** | |
| Communications/*.docx | Wiki > Communications |
| Market Research/*.docx | Wiki > Markets > Interview Protocols + Logbook (interviews as entries) |
| Pitch-decks/* | Wiki > Fundraising > Pitch Deck Versions |
| Investment/*.xlsx | Network (seed investors) + Wiki > Fundraising |
| Go-Together Accelerator/* | Wiki > Validation > GT Objectives + Logbook (sessions) |
| Product/*.docx | Wiki > Product (personas, coaching framework, user stories) |
| Management & Planning/Meetings Notes.docx | Logbook (one entry per meeting) |
| Management & Planning/Applications.docx | Wiki > Fundraising > Applications + Wiki > About Rafiq |
| Management & Planning/First Year Plan.docx | Action System > Projects + Wiki > Validation > 5-Month Plan |
| Time Sheets/*.xlsx | Logbook (Type = Metric, summarized) |
| Resources/* | Wiki > Operations > Reference Materials |
| **Repo (Markdown)** | |
| Objectives/*.md | Wiki > Validation > GT Objectives + Logical Framework |
| Market-Disk-Research/**/*.md | Wiki > Markets > Country Profiles (7 countries x 7 dimensions) |
| Meetings/**/*.md | Logbook (Type = Meeting) |
| WISE-APPLICATION.md | Wiki > Fundraising > Applications |

---

# Implementation Sequence

| Step | What | Effort |
|------|------|--------|
| 1 | Create Notion workspace + custom domain | 30 min |
| 2 | Build database schemas (Inbox, Projects, Tasks, Log, Contacts) | 2 hours |
| 3 | Build Holacracy databases (Circles, Roles) | 30 min |
| 4 | Create Wiki page tree structure | 1 hour |
| 5 | Build Hypothesis Board + Content Calendar databases | 30 min |
| 6 | Set up database relations between all databases | 30 min |
| 7 | Configure views (Kanban boards, calendars, filters) | 1 hour |
| 8 | Migrate Google Drive content → Wiki | 2 hours |
| 9 | Migrate repo markdown → Wiki + Logbook | 1 hour |
| 10 | Seed Network database from spreadsheets | 1 hour |
| 11 | Create meeting & entry templates | 30 min |
| 12 | Set up Notion API integration | 30 min |
| 13 | Build Claude Agent SDK backend | 4-8 hours |
| 14 | Invite team members as guests | 15 min |
| 15 | Team walkthrough / onboarding | 1 hour |

**Total estimated effort: ~12-16 hours**
