# Self-Hosted AI Admin Assistant — Full Brainstorm

## Source: 113 projects from github.com/stars/Mr-Youssry/lists/self-hosting

---

## KEY PROJECTS FROM YOUR LIST (Grouped by Function)

### Workspace / Project Management
| Project | What it does | Stars | Docker? |
|---------|-------------|-------|---------|
| **Focalboard** | "Alternative to Trello, Notion, Asana" — boards, lists, calendar | ✓ | ✓ |
| **Budibase** | Low-code platform, build custom apps on top of databases | ✓ | ✓ |
| **ToolJet** | Internal tools, dashboards, AI agents builder | ✓ | ✓ |
| **PM** | Vibe-coded project management (Vue-based) | Small | ✓ |
| **Super Productivity** | Todo + time tracking + integrations | 17K | ✓ |
| **Midday** | Invoicing, time tracking, file reconciliation, AI assistant | 14K | ✓ |

### Wiki / Knowledge Base
| Project | What it does | Stars | Docker? |
|---------|-------------|-------|---------|
| **Docmost** | Collaborative wiki (Confluence/Notion alternative), real-time editing, diagrams | ✓ | ✓ |
| **CryptPad** | End-to-end encrypted collaborative office suite | ✓ | ✓ |
| **Memos** | Self-hosted note-taking (quick thoughts, not structured wiki) | 57K | ✓ |
| **Note-gen** | Cross-platform Markdown AI note-taking | ✓ | ✓ |
| **Open Notebook** | Open-source NotebookLM alternative | 20K | ✓ |

### CRM
| Project | What it does | Stars | Docker? |
|---------|-------------|-------|---------|
| **Frappe CRM** | Full CRM — pipelines, deals, contacts, activities | ✓ | ✓ |

### Calendar / Scheduling
| Project | What it does | Stars | Docker? |
|---------|-------------|-------|---------|
| **Cal.com** | Scheduling infrastructure, availability slots, team booking | ✓ | ✓ |
| **Easy!Appointments** | Appointment scheduler | ✓ | ✓ |
| **Fluid Calendar** | Calendar management | Small | ✓ |
| **Flow** | Event, group, and time management | ✓ | ✓ |

### Meeting Assistant
| Project | What it does | Stars | Docker? |
|---------|-------------|-------|---------|
| **meetily** | Privacy-first AI meeting assistant, local transcription | ✓ | ✓ |
| **amurex** | AI meeting copilot, "invisible companion for work" | ✓ | ✓ |
| **Screenpipe** | Records everything on screen (including meetings), all local | ✓ | ✓ |
| **screenpipe-meeting-assistant** | Meeting assistant built on Screenpipe | ✓ | ✓ |

### The Frappe Ecosystem (SAME FRAMEWORK)
| Project | What it does |
|---------|-------------|
| **Frappe CRM** | Contacts, pipelines, deals, activities |
| **Frappe Books** | Accounting, invoicing, financial tracking |
| **Frappe HRMS** | HR, payroll, team management |
| **Frappe Drive** | File storage and sharing |

### AI / Agent Platforms
| Project | What it does | Stars |
|---------|-------------|-------|
| **Dify** | AI workflow builder, agents, RAG, API endpoints | ✓ |
| **AnythingLLM** | Desktop/Docker AI app with RAG, agents, MCP | 55K |
| **Open WebUI** | AI chat interface (Ollama, OpenAI API) | ✓ |
| **LibreChat** | Multi-model ChatGPT alternative | ✓ |
| **Kestra** | Event-driven orchestration & scheduling | ✓ |
| **mcp-agent** | Agent building with MCP | ✓ |
| **cognee** | Memory engine for AI agents | ✓ |
| **mem0** | Universal memory layer for AI agents | ✓ |

### Backend / Database
| Project | What it does | Stars |
|---------|-------------|-------|
| **PocketBase** | "Realtime backend in 1 file" — REST API, auth, files | ✓ |
| **Strapi** | Headless CMS, fully customizable | 71K |
| **Kuzu** | Embedded graph database with vector search | ✓ |

### Other Useful Tools
| Project | Function |
|---------|----------|
| **listmonk** | Newsletter/mailing list (daily digests!) |
| **BillionMail** | Self-hosted email server |
| **postiz-app** | Social media scheduling (content calendar!) |
| **Penpot** | Design tool (Figma alternative) |
| **Excalidraw** | Whiteboard/diagramming |
| **ONLYOFFICE** | Office suite (docs, sheets, slides) |
| **OpenSign** | E-signatures |
| **Stirling-PDF** | PDF editing |
| **solidtime** | Time tracking |
| **CasaOS** | Personal cloud OS |

---

## THE THREE BEST ARCHITECTURES

### ═══════════════════════════════════════════════
### OPTION 1: "The Frappe Kingdom" (Most Integrated)
### ═══════════════════════════════════════════════

The Frappe ecosystem is a hidden powerhouse. All apps share ONE framework,
ONE database (MariaDB), ONE API, ONE auth system.

```
┌─────────────────────────────────────────────────────────┐
│                  DigitalOcean Droplet (8GB RAM)          │
│                                                         │
│  ┌───────────────── Frappe Framework ─────────────────┐ │
│  │                                                    │ │
│  │  Frappe CRM        Frappe Books    Frappe HRMS     │ │
│  │  ┌────────────┐   ┌────────────┐  ┌────────────┐  │ │
│  │  │ Contacts   │   │ Ledger     │  │ Team       │  │ │
│  │  │ Pipelines  │   │ Invoices   │  │ Leave mgmt │  │ │
│  │  │ Deals      │   │ Reports    │  │ Payroll    │  │ │
│  │  │ Activities │   │ Budgets    │  │ Attendance │  │ │
│  │  └────────────┘   └────────────┘  └────────────┘  │ │
│  │                                                    │ │
│  │  Frappe Drive      Custom Doctypes (built-in!)     │ │
│  │  ┌────────────┐   ┌────────────────────────────┐   │ │
│  │  │ Files      │   │ Tasks & Projects           │   │ │
│  │  │ Sharing    │   │ OKRs / Circles / Roles     │   │ │
│  │  │ Versions   │   │ Logbook entries            │   │ │
│  │  └────────────┘   │ Meeting records            │   │ │
│  │                    │ Hypotheses & Experiments   │   │ │
│  │                    │ Content Calendar           │   │ │
│  │                    └────────────────────────────┘   │ │
│  │                                                    │ │
│  │            ┌── Frappe REST API ──┐                  │ │
│  └────────────┴────────────────────┴──────────────────┘ │
│                         ↕                               │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Claude Agent SDK + meetily                      │   │
│  │  • Meeting → transcribe → populate Frappe        │   │
│  │  • Daily digest → query Frappe → send email      │   │
│  │  • Chatbot → answer team questions from Frappe   │   │
│  │  • Automation → follow-up alerts, staleness      │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  Docmost (wiki/knowledge base alongside Frappe)         │
└─────────────────────────────────────────────────────────┘
```

**What Frappe gives you natively:**
- ✅ CRM with pipeline views (investor pipeline, partner pipeline)
- ✅ Financial tracking (ledger, invoices, reports)
- ✅ HR (team management, attendance)
- ✅ File storage (Drive)
- ✅ Custom Doctypes = you can build Tasks, Projects, OKRs, Logbook entries
     as structured database entities with forms, views, and API access
- ✅ Single API for everything
- ✅ Built-in permissions and roles
- ✅ Webhooks (for real-time Claude SDK triggers)
- ✅ Email integration
- ✅ Print formats, reports, dashboards

**What you'd add:**
- Docmost for the Wiki (Frappe's document system is functional but not a great wiki)
- meetily/amurex for meeting transcription
- Claude Agent SDK as the AI brain

**Why this is powerful:**
Frappe's "Custom Doctype" system means you can create EXACTLY the databases
from your notion-workspace-v2.md design. Every entity (Circle, Role, Project,
Task, Log Entry, Contact, Hypothesis) becomes a Doctype with typed fields,
relations, and automatic REST API endpoints. Claude SDK can then CRUD all of them
through one consistent API.

**Resource needs:** ~6-8GB RAM (Frappe is Python/MariaDB, moderate footprint)
**Effort:** 20-30 hours (learning Frappe + building doctypes + AI layer)
**Risk:** Frappe has a learning curve. It's enterprise-grade, which means powerful but complex.

---

### ═══════════════════════════════════════════════
### OPTION 2: "Docmost + PocketBase + Claude SDK" (Lightest, Fastest)
### ═══════════════════════════════════════════════

The minimalist architecture. Two lightweight tools + Claude as the brain.

```
┌─────────────────────────────────────────────────────────┐
│              DigitalOcean Droplet (4GB RAM is enough!)   │
│                                                         │
│  ┌────────────────┐    ┌─────────────────────────────┐  │
│  │   Docmost      │    │   PocketBase                │  │
│  │   (Wiki)       │    │   (Structured Data Backend)  │  │
│  │                │    │                              │  │
│  │ • Wiki pages   │    │ Collections:                 │  │
│  │ • Meeting      │    │ ├── circles                  │  │
│  │   summaries    │    │ ├── roles                    │  │
│  │ • Knowledge    │    │ ├── projects                 │  │
│  │   base         │    │ ├── tasks                    │  │
│  │ • Logbook      │    │ ├── log_entries              │  │
│  │   narratives   │    │ ├── contacts                 │  │
│  │ • Decision     │    │ ├── hypotheses               │  │
│  │   records      │    │ ├── metrics                  │  │
│  │                │    │ ├── financial                 │  │
│  │ Spaces:        │    │ └── content_calendar         │  │
│  │ • Product      │    │                              │  │
│  │ • Markets      │    │ Features:                    │  │
│  │ • Validation   │    │ • REST API (auto-generated)  │  │
│  │ • Operations   │    │ • Real-time subscriptions    │  │
│  │ • Fundraising  │    │ • Auth & permissions         │  │
│  │                │    │ • File uploads               │  │
│  └───────┬────────┘    │ • Admin dashboard            │  │
│          │             │ • 1 SINGLE FILE (!)          │  │
│          │             └──────────┬────────────────────┘  │
│          │                       │                       │
│          └───────────┬───────────┘                       │
│                      │                                   │
│          ┌───────────▼──────────────┐                    │
│          │   Claude Agent SDK      │                    │
│          │                         │                    │
│          │  Agents:                │                    │
│          │  ├── Meeting Processor  │ ← meetily input    │
│          │  │   (transcript →      │                    │
│          │  │    summary → tasks   │                    │
│          │  │    → Docmost page    │                    │
│          │  │    → PocketBase      │                    │
│          │  │    records)          │                    │
│          │  │                      │                    │
│          │  ├── Daily Digest       │ → email/Telegram   │
│          │  │   (query PocketBase  │                    │
│          │  │    → format digest   │                    │
│          │  │    → send)           │                    │
│          │  │                      │                    │
│          │  ├── Team Chatbot       │ ← LibreChat or    │
│          │  │   (natural language  │   Telegram bot     │
│          │  │    → query both DBs  │                    │
│          │  │    → answer)         │                    │
│          │  │                      │                    │
│          │  ├── Follow-up Monitor  │ → alerts           │
│          │  │   (check contacts    │                    │
│          │  │    overdue dates)    │                    │
│          │  │                      │                    │
│          │  └── Inbox Processor    │                    │
│          │      (classify items →  │                    │
│          │       route to right    │                    │
│          │       system)           │                    │
│          └─────────────────────────┘                    │
│                                                         │
│  meetily (meeting transcription, runs alongside)        │
│  listmonk (for sending daily digest emails)             │
└─────────────────────────────────────────────────────────┘
```

**Why PocketBase is the secret weapon:**
- It's literally ONE FILE. One binary. No dependencies.
- Auto-generates REST API for every collection you create
- Real-time subscriptions (Claude agent can react instantly)
- Admin UI for your team to browse/edit data manually
- Built-in auth, file storage
- Runs on ~50MB of RAM
- You define your schema visually or via API → instant CRUD endpoints
- Claude SDK can read/write everything via simple HTTP calls

**The split:**
- **PocketBase** = structured data (tasks, contacts, metrics, log entries) — the "database"
- **Docmost** = unstructured content (wiki pages, narratives, knowledge articles) — the "wiki"
- **Claude SDK** = intelligence layer (processing, routing, querying, alerting)
- **meetily** = sensory input (meeting transcription)
- **listmonk** = output channel (daily emails, newsletters)

**Resource needs:** ~2-3GB RAM total. Will run comfortably on a $24/mo droplet.
**Effort:** 15-20 hours (PocketBase schema + Docmost setup + Claude agents)
**Risk:** Low. Each component is simple. PocketBase admin UI is basic but functional.

---

### ═══════════════════════════════════════════════
### OPTION 3: "Budibase + Docmost + Claude SDK" (Most Customizable UI)
### ═══════════════════════════════════════════════

Budibase lets you build custom apps (Kanban boards, dashboards, forms, tables)
on top of a database. You get a visual UI that looks professional.

```
┌─────────────────────────────────────────────────────────┐
│              DigitalOcean Droplet (8GB RAM)              │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Budibase                                        │   │
│  │                                                  │   │
│  │  Custom Apps (built visually, no-code):           │   │
│  │  ┌─────────────┐  ┌─────────────┐               │   │
│  │  │ Action      │  │ Network     │               │   │
│  │  │ System      │  │ (CRM)       │               │   │
│  │  │ • Projects  │  │ • Contacts  │               │   │
│  │  │   (Kanban)  │  │   (table)   │               │   │
│  │  │ • Tasks     │  │ • Pipeline  │               │   │
│  │  │   (board +  │  │   (Kanban)  │               │   │
│  │  │    list)    │  │ • Follow-up │               │   │
│  │  │ • Calendar  │  │   (filtered)│               │   │
│  │  │   (view)    │  │             │               │   │
│  │  └─────────────┘  └─────────────┘               │   │
│  │  ┌─────────────┐  ┌─────────────┐               │   │
│  │  │ Logbook     │  │ Holacracy   │               │   │
│  │  │ • Timeline  │  │ • Circles   │               │   │
│  │  │ • Meetings  │  │ • Roles     │               │   │
│  │  │ • Decisions │  │ • Governance│               │   │
│  │  │ • Financial │  │   log       │               │   │
│  │  │ • Metrics   │  │             │               │   │
│  │  │   dashboard │  │             │               │   │
│  │  └─────────────┘  └─────────────┘               │   │
│  │                                                  │   │
│  │  Backed by: PostgreSQL (shared with Claude SDK)  │   │
│  │  API: Auto-generated REST endpoints              │   │
│  └──────────────────────────────────────────────────┘   │
│                         ↕                               │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Docmost (Wiki) + meetily + Claude Agent SDK     │   │
│  │  (same as Option 2)                              │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

**Why Budibase is interesting:**
- You can replicate EVERY VIEW from your notion-workspace-v2.md
  (Kanban boards, filtered tables, calendar views, dashboards)
- Visual app builder = fast iteration
- Backed by PostgreSQL = Claude SDK can also query directly
- Role-based access control
- Built-in automation workflows
- REST API for everything

**Resource needs:** ~6-8GB RAM
**Effort:** 25-35 hours (building all the Budibase apps + wiki + AI)
**Risk:** Medium. Budibase is powerful but can feel clunky for complex apps.

---

## COMPARISON MATRIX

| Factor | Frappe Kingdom | Docmost+PocketBase | Budibase+Docmost |
|--------|---------------|-------------------|------------------|
| **RAM needed** | 6-8 GB | 2-3 GB | 6-8 GB |
| **Monthly cost** | $48 droplet | $24 droplet | $48 droplet |
| **Setup effort** | 25-35 hrs | 15-20 hrs | 25-35 hrs |
| **CRM quality** | ★★★★★ (native) | ★★★ (custom) | ★★★★ (custom) |
| **Wiki quality** | ★★★ (+Docmost) | ★★★★★ (Docmost) | ★★★★★ (Docmost) |
| **Task mgmt** | ★★★★ (custom) | ★★★ (basic UI) | ★★★★★ (Kanban+) |
| **Financial** | ★★★★★ (native) | ★★★ (custom) | ★★★ (custom) |
| **API quality** | ★★★★★ | ★★★★★ | ★★★★ |
| **Claude SDK fit** | ★★★★ | ★★★★★ | ★★★★ |
| **Team usability** | ★★★ (learning curve) | ★★★★ (simple) | ★★★★ (visual) |
| **Scalability** | ★★★★★ | ★★★★ | ★★★★ |
| **Maintenance** | ★★★ (complex) | ★★★★★ (minimal) | ★★★ (updates) |

---

## THE MEETING HEARTBEAT — How It Works (Any Architecture)

```
┌──────────────────────────────────────────────────────┐
│                  THE HEARTBEAT CYCLE                 │
│                                                      │
│  MEETING INPUT                                       │
│  ════════════                                        │
│  Team joins call → meetily/Screenpipe records         │
│  → Audio captured locally on droplet                 │
│  → Whisper transcribes to text                       │
│                                                      │
│  CLAUDE AGENT: Meeting Processor                     │
│  ════════════════════════════════                     │
│  Input: Raw transcript                               │
│  │                                                   │
│  ├─→ SUMMARY (structured)                            │
│  │   "Meeting: Tactical #14 — March 6, 2026          │
│  │    Attendees: Ahmed, Shereen, Nermeen              │
│  │    Duration: 47 minutes"                           │
│  │                                                   │
│  ├─→ DECISIONS (extracted)                            │
│  │   "ADR-023: Focus Qatar pilot on private           │
│  │    schools first, not government"                  │
│  │   Status: Accepted                                │
│  │   Rationale: "Government procurement cycle         │
│  │   is 18 months, too slow for validation"           │
│  │                                                   │
│  ├─→ ACTION ITEMS (with assignments)                  │
│  │   "☐ Shereen: Contact Al Jazeera Academy by       │
│  │    March 10 — Priority P1"                        │
│  │   "☐ Ahmed: Set up staging environment by          │
│  │    March 8 — Priority P2"                         │
│  │   "☐ Nermeen: Draft Qatar market one-pager by      │
│  │    March 12 — Priority P1"                        │
│  │                                                   │
│  ├─→ FLAGS (uncertainties)                            │
│  │   "⚠️ Not sure if budget discussed was $5K or     │
│  │    $15K for Qatar pilot. Please confirm."          │
│  │   "⚠️ Shereen mentioned a contact name that       │
│  │    sounded like 'Dr. Al-Mansour' or               │
│  │    'Dr. Al-Mansoori'. Please verify."              │
│  │                                                   │
│  ├─→ METRICS MENTIONED                               │
│  │   "Interviews completed: 8 (target: 15)"          │
│  │   "Prototype test sessions: 3 (target: 10)"       │
│  │                                                   │
│  ├─→ WIKI UPDATES NEEDED                             │
│  │   "Qatar country profile needs update:             │
│  │    private school segment prioritized"             │
│  │                                                   │
│  └─→ GOVERNANCE CHANGES                              │
│      "Nermeen taking over Qatar market research       │
│       from Shereen (role change in Business Dev)"     │
│                                                      │
│  POPULATION (automatic writes)                       │
│  ═══════════                                         │
│  → Logbook: Meeting entry with full summary           │
│  → Logbook: Decision ADR entry                        │
│  → Tasks: 3 new tasks created, assigned, dated        │
│  → Metrics: Updated interview count to 8              │
│  → Contacts: Log interaction with mentioned contacts  │
│  → Inbox: 2 flagged items for human review            │
│  → Wiki: Draft update queued for Qatar page           │
│  → Governance: Role change logged                     │
│                                                      │
│  NOTIFICATION                                        │
│  ════════════                                         │
│  → Team chat/email: "Meeting #14 processed.           │
│     3 tasks created. 2 items need your review. ⚠️"    │
│                                                      │
│  DAILY DIGEST (next morning, 7am)                    │
│  ═════════════                                        │
│  → Personalized per team member                      │
│  → "Good morning Shereen, today you have:             │
│     P1: Contact Al Jazeera Academy (due today)        │
│     P1: Review Qatar market one-pager (due Mar 12)    │
│     ⚠️ Please confirm: was the budget $5K or $15K?   │
│     📊 This week: 2/5 tasks completed                │
│     📅 Thursday: Tactical meeting at 4pm              │
│     💡 Hypothesis H-012 has been in 'Testing'         │
│        for 3 weeks — update needed"                   │
└──────────────────────────────────────────────────────┘
```

---

## MY UPDATED RECOMMENDATION

### 🏆 Go with Option 2: Docmost + PocketBase + Claude SDK

**Why:**

1. **Lightest footprint** — runs on your existing $24/mo droplet with room to spare
2. **Fastest to build** — 15-20 hours to a working system
3. **PocketBase is perfect for Claude SDK** — simple REST API, every collection
   auto-generates CRUD endpoints, Claude can read/write everything trivially
4. **Docmost gives you a beautiful wiki** — your team gets real-time collaboration,
   diagrams, spaces, exactly what you need for knowledge management
5. **Lowest risk** — if PocketBase doesn't scale, migrate to PostgreSQL later;
   if Docmost doesn't fit, swap for another wiki. The Claude SDK agents are portable.
6. **Your team can use it immediately** — Docmost is intuitive (it looks like Notion),
   PocketBase admin is simple, and the AI does the heavy lifting

### Enhancement path (add later if needed):
- **Cal.com** → when you need public scheduling/availability slots
- **listmonk** → when you want beautiful digest emails instead of plain text
- **Frappe CRM** → if CRM needs outgrow PocketBase collections
- **postiz-app** → when content calendar needs social media scheduling
- **Screenpipe** → when you want always-on meeting capture (not just manual)

### The Stack:
```
Docmost          — Wiki, knowledge base, meeting summaries, logbook narratives
PocketBase       — Tasks, projects, contacts, metrics, OKRs, financial records
meetily          — Meeting transcription
Claude Agent SDK — The brain: processing, routing, querying, alerting
listmonk         — Daily digest delivery (optional, can start with Telegram)
```

### Total monthly cost:
- DigitalOcean droplet (4GB): ~$24/mo
- Anthropic API (Claude): ~$20-50/mo (depending on meeting frequency)
- Domain (notion.rafiq.ai → workspace.rafiq.ai): already owned
- **Total: ~$44-74/mo** for a system that would cost $500+/mo commercially
