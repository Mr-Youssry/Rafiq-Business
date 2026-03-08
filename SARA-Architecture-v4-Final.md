# SARA — Synthetic Administration and Research Assistant

## Architecture & Build Blueprint v4.0 (Final — Ready for Claude Code)

**Project:** Internal AI-powered operating system for the RafiqAI founding team
**Authors:** Mr Be + Claude (brainstorm session, March 2026)
**Status:** Design complete — ready to build with Claude Code
**Domain:** `*.office.rafiq-ai.com` (nested subdomain, isolated from customer-facing domains)

---

## 1. What is SARA?

SARA is an AI Chief of Staff for the RafiqAI founding team. She is a self-hosted system that combines a knowledge base, accounting, calendar scheduling, meeting intelligence, holacracy integration, proactive research, CRM, and operational logging into a single AI-powered platform — unified behind one SSO login and accessible via a polished chat UI, Slack, and voice in meetings.

SARA is not a product for RafiqAI's customers. She is an internal tool that makes a 4-person founding team (plus 3 advisors) operate like a 15-person one.

### What SARA does

- **Knows everything:** Stores and reasons over all team docs, meeting notes, OKRs, decisions, financial data, and project context
- **Attends meetings:** Joins Google Meet/Zoom as a bot, transcribes (Arabic + English code-switching), structures notes, extracts action items
- **Speaks in meetings:** Answers questions live with voice ("Hey Sara, are we on track for the pilot?")
- **Manages the org:** Reads and writes to GlassFrog (holacracy roles, circles, projects, metrics, tensions)
- **Tracks finances:** Pulls real data from Bigcapital (P&L, balance sheet, expenses) and models future scenarios with Claude
- **Manages scheduling:** Owns the team calendar via Cal.com — booking pages for investors, meeting coordination, automated reminders
- **Tracks the pipeline:** Maintains investor, school, and partner contacts as structured docs in Outline
- **Keeps the books:** Logs timesheets (auto-generated from meetings), validation records, and team decisions
- **Researches proactively:** During idle time, scans the web for competitors, funding news, edtech trends, AI developments
- **Reminds and nudges:** Posts daily briefings, weekly digests, LinkedIn posting reminders, overdue action item alerts
- **Converses naturally:** Via Open WebUI (polished ChatGPT-like interface), Slack bot, or voice — grounded in the full knowledge base
- **Unified login:** All services behind Keycloak SSO — one account, one login, access everything

---

## 2. Domain Strategy

All SARA services live under `office.rafiq-ai.com` as nested subdomains. This namespace is completely isolated from customer-facing domains.

```
Internal (SARA ecosystem — team only)
┌─────────────────────────────────────────────────┐
│  *.office.rafiq-ai.com                          │
│                                                  │
│  wiki.office.rafiq-ai.com   → Outline           │
│  chat.office.rafiq-ai.com   → Open WebUI (SARA) │
│  books.office.rafiq-ai.com  → Bigcapital        │
│  cal.office.rafiq-ai.com    → Cal.com           │
│  auth.office.rafiq-ai.com   → Keycloak (SSO)    │
│  meet.office.rafiq-ai.com   → Vexa (future)     │
└─────────────────────────────────────────────────┘

Public (customer-facing — completely separate)
┌─────────────────────────────────────────────────┐
│  rafiq-ai.com               → marketing site    │
│  app.rafiq-ai.com           → RafiqAI product   │
│  api.rafiq-ai.com           → product API       │
└─────────────────────────────────────────────────┘
```

### DNS Configuration (Cloudflare)

One wildcard CNAME or individual records per service:

```
*.office.rafiq-ai.com  →  CNAME  →  <tunnel-id>.cfargotunnel.com
```

Or individually:

```
wiki.office.rafiq-ai.com    →  CNAME  →  <tunnel-id>.cfargotunnel.com
chat.office.rafiq-ai.com    →  CNAME  →  <tunnel-id>.cfargotunnel.com
books.office.rafiq-ai.com   →  CNAME  →  <tunnel-id>.cfargotunnel.com
cal.office.rafiq-ai.com     →  CNAME  →  <tunnel-id>.cfargotunnel.com
auth.office.rafiq-ai.com    →  CNAME  →  <tunnel-id>.cfargotunnel.com
```

### Cloudflare Tunnel Config (`config.yml`)

```yaml
tunnel: <tunnel-id>
credentials-file: /etc/cloudflared/<tunnel-id>.json

ingress:
  - hostname: wiki.office.rafiq-ai.com
    service: http://localhost:3000
  - hostname: chat.office.rafiq-ai.com
    service: http://localhost:3080
  - hostname: books.office.rafiq-ai.com
    service: http://bigcapital-nginx:80     # on DO droplet, separate tunnel
  - hostname: cal.office.rafiq-ai.com
    service: http://localhost:3100
  - hostname: auth.office.rafiq-ai.com
    service: http://localhost:9090
  - service: http_status:404               # catch-all
```

### Security

- **Keycloak SSO** gates every service — no anonymous access to any `*.office.rafiq-ai.com` URL
- **No public listing** — these subdomains are not linked from any public page, sitemap, or DNS listing
- **Cloudflare proxy** — real server IP is hidden behind Cloudflare's network
- **All HTTPS** — Cloudflare handles SSL termination automatically

---

## 3. Architecture Overview

```
SARA Ecosystem
══════════════

Surface Pro (home server)                    DO Droplets
i7 / 8GB RAM / 250GB SSD                    
Ubuntu Server 24.04 LTS                     Vexa Droplet ($24/mo)
│                                            4GB RAM / 2 vCPU
│                                            ├── Vexa meeting bot API
├── Keycloak                                 │   ├── Joins Google Meet / Zoom
│   SSO identity provider                    │   ├── Headless Chrome
│   auth.office.rafiq-ai.com                 │   ├── Whisper STT (GPU-free mode)
│   OIDC for all services                    │   ├── Interactive bot (speak/chat)
│   Port 9090                                │   └── WebSocket transcript stream
│                                            │
├── Open WebUI                               Bigcapital Droplet ($6/mo)
│   SARA chat interface                      1GB RAM / 1 vCPU
│   chat.office.rafiq-ai.com                 ├── Bigcapital server + webapp
│   PWA (installable on phone)               ├── MariaDB + MongoDB
│   Conversation history, voice, files       ├── Redis
│   Connected to sara-api via Pipeline       └── books.office.rafiq-ai.com
│   SSO via Keycloak                             (via separate Cloudflare Tunnel)
│   Port 3080                               
│                                            
├── Outline                                  
│   Knowledge base, wiki, CRM, logs         
│   wiki.office.rafiq-ai.com                
│   SSO via Keycloak                        
│   Port 3000                               
│                                            
├── Cal.com                                  
│   Scheduling & calendar                    
│   cal.office.rafiq-ai.com                 
│   Syncs with Google Calendar              
│   Booking pages for investors/schools     
│   SSO via Keycloak                        
│   Port 3100                               
│                                            
├── PostgreSQL 15                            
│   Database for Outline + Cal.com          
│   + sara-api                              
│   Port 5432                                
│                                            
├── Redis                                    
│   Shared cache: Outline + APScheduler     
│   Port 6379                                
│                                            
├── sara-api (FastAPI + Python)              
│   Core AI service — SARA's brain          
│   Claude SDK (Anthropic API)               
│   Port 8080                                
│   │                                        
│   ├── Endpoints                            
│   │   ├── POST /ask                        
│   │   ├── POST /meeting                    
│   │   ├── POST /digest                     
│   │   ├── POST /realtime                   
│   │   ├── POST /glassfrog                  
│   │   ├── POST /finance                    
│   │   ├── POST /calendar                   
│   │   ├── POST /export                     
│   │   │     ↑ Generate PDF/PPTX from       
│   │   │       Outline docs                 
│   │   ├── POST /parse                      
│   │   │     ↑ Parse uploaded docs          
│   │   │       (PDF, DOCX, PPTX) via Docling
│   │   ├── POST /fill-form                  
│   │   │     ↑ TheAgenticBrowser fills      
│   │   │       web forms (applications)     
│   │   ├── POST /webhooks/outline           
│   │   ├── POST /webhooks/vexa             
│   │   ├── POST /webhooks/cal              
│   │   └── GET  /v1/chat/completions        
│   │         ↑ OpenAI-compatible endpoint   
│   │           for Open WebUI Pipeline      
│   │                                        
│   ├── Cron Scheduler (APScheduler)         
│   │   ├── daily_briefing      8:00 AM EET  
│   │   ├── weekly_digest       Sun 8:00 PM  
│   │   ├── okr_check           Mon 9:00 AM  
│   │   ├── linkedin_remind     Tue/Thu 10AM 
│   │   ├── reindex_docs        Every 6hrs   
│   │   ├── pipeline_check      Fri 3:00 PM  
│   │   ├── monthly_finance     1st of month 
│   │   └── timesheet_gen       Fri 5:00 PM  
│   │                                        
│   ├── Heartbeat Loop                       
│   │   ├── Every 30min, 9AM-10PM EET        
│   │   ├── Reads HEARTBEAT.md from Outline  
│   │   ├── Triage on Qwen 3.5 2B (free)    
│   │   ├── Work on Claude Sonnet (smart)    
│   │   └── Reports to Slack                 
│   │                                        
│   └── Research Processor                   
│       ├── Picks from Research Queue doc     
│       ├── Scrapling for stealth web fetching
│       ├── Qwen 3.5 2B for extraction (free)
│       ├── Claude Sonnet for synthesis (paid)
│       ├── Writes findings to Outline        
│       └── Posts summary to Slack            
│                                            
├── sara-slack-bot                           
│   Slack integration (Bolt for Python)      
│   Commands: /sara ask, /sara join,         
│   /sara research, /sara digest, /sara cal  
│                                            
├── Ollama + Qwen 3.5 2B                    
│   Local LLM for free background tasks      
│   Heartbeat triage, web scraping,          
│   data extraction, classification          
│   OpenAI-compatible API at :11434          
│   Port 11434                               
│                                            
└── cloudflared                              
    Cloudflare Tunnel daemon                 
    Routes *.office.rafiq-ai.com             
    to internal services                     


External SaaS (source of truth)
├── GlassFrog        Holacracy governance, roles, circles
├── Slack            Team async communication
├── Anthropic API    Claude Haiku + Sonnet
└── Vexa hosted API  Meeting bot (free tier to start, then self-host)
```

---

## 4. Technology Stack

| Component | Technology | Domain | Where hosted |
|-----------|-----------|--------|-------------|
| SSO / Auth | Keycloak | auth.office.rafiq-ai.com | Surface |
| Chat UI | Open WebUI | chat.office.rafiq-ai.com | Surface |
| Knowledge base | Outline | wiki.office.rafiq-ai.com | Surface |
| Calendar | Cal.com | cal.office.rafiq-ai.com | Surface |
| Accounting | Bigcapital | books.office.rafiq-ai.com | DO droplet ($6/mo) |
| Meeting bot | Vexa | meet.office.rafiq-ai.com (future) | DO droplet ($24/mo) |
| AI service | FastAPI + Claude SDK | internal (port 8080) | Surface |
| Database (Outline, Cal) | PostgreSQL 15 | internal | Surface |
| Database (Bigcapital) | MariaDB + MongoDB | internal | DO droplet |
| Cache / Queue | Redis | internal | Surface + DO droplet |
| LLM (cloud) | Claude Sonnet + Haiku | Anthropic API (cloud) | — |
| LLM (local) | Ollama + Qwen 3.5 2B | internal (port 11434) | Surface |
| Web scraping | Scrapling | internal (pip library) | Surface |
| Document parsing | Docling (IBM) | internal (pip library) | Surface |
| Form filling | TheAgenticBrowser | internal (pip + Playwright) | Surface |
| PDF generation | WeasyPrint | internal (pip library) | Surface |
| Slide generation | python-pptx | internal (pip library) | Surface |
| STT | Whisper (via Vexa) | — | DO droplet |
| TTS | Edge TTS (Python) | — | Surface |
| Holacracy | GlassFrog API v3 | glassfrog.com (SaaS) | — |
| Tunnel | Cloudflare Tunnel | *.office.rafiq-ai.com | Surface + DO |
| Scheduling | APScheduler + Redis | internal | Surface |
| Slack | Slack Bolt (Python) | — | Surface |
| Containerization | Docker Compose | — | All |

### Python Utility Libraries (pip install, zero infrastructure)

All of these live inside sara-api as pip dependencies. No new containers, no RAM impact beyond negligible.

| Library | Purpose | When to add |
|---------|---------|-------------|
| **litellm** | Unified LLM API — one interface for Ollama, Claude Sonnet, Haiku. Simplifies all model routing. | Phase 1 |
| **instructor** | Structured output from LLMs via Pydantic models. Guarantees clean JSON for action items, contact extraction, etc. | Phase 1 |
| **newspaper4k** | Clean article extraction from news URLs — title, text, authors, date. For research pipeline. | Phase 3 |
| **markdownify** | HTML → clean markdown conversion. Glue between Scrapling web fetches and Outline storage. | Phase 3 |
| **jinja2** | Template engine for PDF/PPTX generation. Powers `{{ company_name }}` in export templates. | Phase 5 |
| **pandas** | Data analysis and financial modeling. Burn rate, runway projections, unit economics. | Phase 5 |
| **plotly** | Interactive charts — burn rate graphs, OKR progress, pipeline funnels. Embeddable in Outline. | Phase 5 |
| **arabic-reshaper + python-bidi** | Proper Arabic text rendering in exported PDFs. Essential for bilingual MENA materials. | Phase 5 |
| **langdetect** | Detect Arabic vs English in code-switched text. Helps segment bilingual meeting transcripts. | Phase 3 |
| **tiktoken** | Token counting before LLM calls. Prevents context window overflows, helps decide summarize-first strategy. | Phase 1 |
| **icalendar** | Parse/generate iCal files for Cal.com webhook processing and calendar sync. | Phase 2 |
| **slack-sdk** | Extended Slack features — file uploads (share PDFs in Slack), Block Kit (interactive buttons), threading. | Phase 1 |
| **difflib** (stdlib) | Text diff comparison. Meaningful change notifications when Outline docs are updated. | Phase 3 |

---

## 5. Unified Authentication (Keycloak)

All SARA services authenticate through a single Keycloak instance at `auth.office.rafiq-ai.com`.

### How it works

```
User opens any *.office.rafiq-ai.com URL
        │
        ▼
Service checks: authenticated?
        │
    No ─┤── Redirect to auth.office.rafiq-ai.com
        │         │
        │         ▼
        │   Keycloak login page
        │   (email + password, or future: 2FA)
        │         │
        │         ▼
        │   Keycloak issues OIDC token
        │         │
        │   Yes ──┘
        │
        ▼
Access granted — user is in
```

### Keycloak Realm: `rafiq-office`

**Users (7 total):**

| Person | Keycloak Role | Access Level |
|--------|--------------|--------------|
| Mr Be | `team-admin` | Full access — all services |
| Nermeen | `team-admin` | Full access — all services |
| Shereen | `team-member` | Full access — all services |
| Kelly | `team-member` | Full access — all services |
| Advisor 1 | `advisor` | Read-only — selected Outline collections |
| Advisor 2 | `advisor` | Read-only — selected Outline collections |
| Advisor 3 | `advisor` | Read-only — selected Outline collections |

**Permission Matrix:**

```
                         Team          Advisors
                         (Mr Be,       (3 advisors)
                         Nermeen,
                         Shereen,
                         Kelly)

Keycloak SSO             ✅ Full        ✅ Limited
Open WebUI (SARA chat)   ✅ Full        ❌ No access
Outline (wiki)           ✅ All         ✅ Read-only, selected collections
Bigcapital (books)       ✅ Full        ❌ No access (see briefs in Outline)
Cal.com (calendar)       ✅ Full        📎 Public booking links only
GlassFrog               ✅ Full        👁️ View-only (free on GlassFrog)
Slack                    ✅ Full        Optional guest channels
```

**Outline Collections — Advisor Visibility:**

| Collection | Team | Advisors |
|-----------|------|----------|
| 📋 OKRs | Read/Write | ✅ Read-only |
| 📝 Meetings | Read/Write | ✅ Read-only |
| 🚀 Projects | Read/Write | ✅ Read-only |
| 🔬 Research | Read/Write | ✅ Read-only |
| 💼 Pipeline (CRM) | Read/Write | ❌ Hidden |
| 📣 Content & Marketing | Read/Write | ❌ Hidden |
| 📊 Logs | Read/Write | ❌ Hidden |
| 🔮 Financial Intelligence | Read/Write | ✅ Read-only (narrative briefs only) |
| ⚙️ SARA Config | Read/Write | ❌ Hidden |
| 📚 Knowledge Base | Read/Write | ✅ Read-only |
| 📦 Exports | Read/Write | ✅ Read-only (shared pitch decks, one-pagers) |

**Clients (one per service):**

| Client ID | Service | Protocol |
|-----------|---------|----------|
| `outline` | wiki.office.rafiq-ai.com | OIDC |
| `open-webui` | chat.office.rafiq-ai.com | OIDC |
| `bigcapital` | books.office.rafiq-ai.com | SAML / OIDC |
| `calcom` | cal.office.rafiq-ai.com | OIDC |
| `sara-api` | Internal API auth | JWT validation |

### Authentication Exception: GlassFrog

GlassFrog is the one service that lives outside Keycloak SSO. It's a SaaS product at `glassfrog.com` that doesn't support external OIDC on the free plan.

- **Team (humans):** Log into GlassFrog separately with GlassFrog credentials — the one separate login
- **SARA (AI):** Queries GlassFrog via API key — no browser login needed
- **Advisors:** Get free view-only accounts on GlassFrog if they need to see the org structure

This exception goes away in Phase 6 when governance moves into Outline.

### Adding a new team member

1. Create one account in Keycloak
2. Assign role (admin or user)
3. They can immediately access all services

No per-service account creation needed.

---

## 6. Cal.com — Scheduling & Calendar

Cal.com replaces Google Calendar as the team's scheduling hub. Self-hosted at `cal.office.rafiq-ai.com`.

### What it provides

- **Team calendar** — shared view of all team availability
- **Booking pages** — shareable links for external meetings:
  - `cal.office.rafiq-ai.com/mrbe/investor-call` — 30-min investor call
  - `cal.office.rafiq-ai.com/nermeen/school-demo` — 45-min school demo
  - `cal.office.rafiq-ai.com/team/sync` — team sync scheduling
- **Calendar sync** — two-way sync with Google Calendar (personal calendars stay in sync)
- **Automated reminders** — SMS and email reminders before meetings
- **Workflows** — auto-send prep materials, follow-up emails after booking
- **Video integration** — built-in Cal Video, or Google Meet / Zoom links

### How SARA uses Cal.com

- **Daily briefing:** Pulls today's schedule from Cal.com API → includes in morning Slack briefing
- **Meeting prep:** Before each booked meeting, SARA generates a prep brief (who, context from Outline, last interaction, open items)
- **Follow-up tracking:** After meetings, SARA creates follow-up tasks and checks if they were completed
- **Pipeline integration:** When an investor books via Cal.com, SARA auto-creates or updates their CRM doc in Outline
- **Heartbeat check:** "Any meetings in the next 2 hours without a prep brief? Generate one."

---

## 7. Open WebUI — SARA's Chat Interface

Open WebUI is the primary way the team interacts with SARA for longer conversations. Lives at `chat.office.rafiq-ai.com`, installable as a PWA on mobile.

### Why Open WebUI

- Polished, battle-tested UI with conversation history, markdown rendering, file uploads
- PWA — installable on phone, works like a native app
- Voice input (Whisper) and voice output (TTS) built in
- Keycloak SSO integration — documented and proven
- Custom theming — branded as SARA
- Responsive across desktop, laptop, and mobile

### Connection to SARA

```
User types message in Open WebUI
        │
        ▼
Open WebUI sends to SARA Pipeline (Python plugin)
        │
        ▼
Pipeline calls sara-api → enriches with full context:
├── Outline: docs, meeting notes, OKRs, CRM, logs
├── GlassFrog: roles, projects, metrics
├── Bigcapital: financial data, reports
├── Cal.com: upcoming schedule, booking history
├── Vexa: meeting history, transcripts
└── Web search: if research needed
        │
        ▼
Claude generates response → Open WebUI renders it
```

### Slack vs Open WebUI

| Slack | Open WebUI |
|-------|-----------|
| Quick queries, meeting commands, notifications | Deep conversations, scenario modeling, document drafting |
| Team-visible interactions | Private 1-on-1 with SARA |
| Mobile-quick (already in Slack) | Mobile-rich (full markdown, voice, files) |

---

## 8. Outline Collections Structure

```
Outline Collections (wiki.office.rafiq-ai.com)
├── 📋 OKRs
│   ├── Q2 2026 Objectives
│   ├── Key Results Tracker
│   └── OKR Archive
│
├── 📝 Meetings
│   ├── [Auto-generated meeting docs by SARA]
│   ├── Meeting Template
│   └── Decisions Log (append-only)
│
├── 🚀 Projects
│   ├── EdVenture
│   ├── RafiqAI
│   ├── SARA (this project)
│   └── Project Template
│
├── 🔬 Research
│   ├── Research Queue (SARA picks from here)
│   ├── [Auto-generated research briefs]
│   ├── Competitive Landscape
│   └── Investor Landscape
│
├── 💼 Pipeline (CRM)
│   ├── Investors
│   │   ├── [One doc per investor contact]
│   │   └── Investor Tracker Summary
│   ├── Schools / Partners
│   │   ├── [One doc per school/org]
│   │   └── Partner Tracker Summary
│   └── Pipeline Template
│
├── 📣 Content & Marketing
│   ├── LinkedIn Content Calendar
│   ├── Post Drafts
│   └── Brand Voice Guide
│
├── 📊 Logs
│   ├── Timesheets
│   │   ├── [Mr Be — Week of {date}]
│   │   ├── [Nermeen — Week of {date}]
│   │   ├── [Shereen — Week of {date}]
│   │   ├── [Kelly — Week of {date}]
│   │   └── Timesheet Template
│   ├── Validation Log
│   │   ├── [Hypothesis]: Teachers will use AI coaching weekly
│   │   ├── [Hypothesis]: Schools will pay $X per teacher
│   │   └── Validation Template
│   ├── Decision Log
│   │   └── [Auto-populated from meeting transcripts]
│   └── Changelog
│       ├── SARA Changelog
│       └── Product Changelog
│
├── 🔮 Financial Intelligence
│   ├── Monthly Finance Brief (SARA-generated from Bigcapital)
│   ├── Runway Model (updated monthly)
│   ├── Revenue Scenarios (what-if analyses)
│   ├── Hiring Impact Models
│   └── Unit Economics Tracker
│
├── ⚙️ SARA Config
│   ├── SOUL.md (SARA's identity, personality, boundaries)
│   ├── HEARTBEAT.md (autonomous action checklist)
│   ├── SKILLS.md (skill registry — all capabilities)
│   ├── Research Queue
│   ├── Users
│   │   ├── Mr Be.md (preferences, context, interaction patterns)
│   │   ├── Nermeen.md
│   │   ├── Shereen.md
│   │   └── Kelly.md
│   └── SARA Changelog
│
├── 📚 Knowledge Base
│   ├── Architecture Decisions (ADRs)
│   ├── Processes & Playbooks
│   ├── Onboarding
│   └── Reference
│
└── 📦 Exports (SARA-generated PDFs & slides)
    ├── Pitch Decks
    │   ├── [RafiqAI Pitch Deck v1.2 (March 2026).pptx]
    │   └── Pitch Deck Template
    ├── Investor Materials
    │   ├── [One-Pager (Latest).pdf]
    │   ├── [Financial Summary Q1 2026.pdf]
    │   └── [Investor Update March 2026.pdf]
    ├── School Materials
    │   ├── [School Partnership Proposal.pdf]
    │   ├── [Pilot Program Overview.pdf]
    │   └── Case Study Template
    └── Branding
        ├── RafiqAI Logo (various formats)
        ├── PDF Template (WeasyPrint HTML/CSS)
        └── Slide Template (python-pptx)
```

---

## 9. Integrations Map

### Keycloak → All Services (SSO)
- **Protocol:** OIDC (OpenID Connect)
- **Services:** Outline, Open WebUI, Cal.com, Bigcapital
- **API Auth:** JWT token validation for sara-api

### Open WebUI → sara-api (via Pipeline)
- **Protocol:** OpenAI Chat Completions
- **Pipeline:** Custom Python plugin enriching every message with SARA's full context

### Outline API (bi-directional)
- **Read:** Search docs, get content, list collections
- **Write:** Create meeting notes, research briefs, update OKRs, append logs
- **Events:** Webhooks on doc create/update/delete

### Cal.com API (bi-directional)
- **Read:** Today's schedule, upcoming bookings, availability
- **Write:** Create event types, update bookings
- **Webhooks:** New booking → trigger meeting prep brief
- **Sync:** Two-way sync with Google Calendar for personal calendars

### Bigcapital API (primarily read)
- **Read:** P&L, balance sheet, cash flow, invoices, expenses, account balances
- **Write:** Potentially create expense entries (Phase 5+)

### GlassFrog API v3 (bi-directional)
- **Read:** Circles, roles, people, projects, metrics, checklist items, goals & targets
- **Write:** Create projects, update actions, log tensions

### Vexa API (bi-directional)
- **Send:** Bot join meeting, speak command (TTS audio)
- **Receive:** Real-time transcript stream via WebSocket, meeting end webhook
- **MCP:** Vexa MCP server for agent integration

### Slack (bi-directional)
- **Send:** Briefings, digests, summaries, alerts, nudges
- **Receive:** Slash commands, mentions, DMs

### Anthropic API (outbound)
- **Claude Sonnet:** Complex reasoning, meeting processing, research synthesis, scenario modeling, document writing
- **Claude Haiku:** Fallback for when local model is unavailable or task needs slightly more intelligence than Qwen 2B

### Ollama — Local LLM (on Surface)
- **Model:** Qwen 3.5 2B quantized (~1.5GB)
- **API:** OpenAI-compatible at `localhost:11434`
- **Used for:** Heartbeat triage, web scraping summaries, data extraction, document classification, simple Q&A, tagging
- **Cost:** $0 — runs entirely on Surface CPU
- **Routing:** sara-api automatically routes tasks to local Qwen or cloud Claude based on complexity

**Model routing logic in sara-api:**

```
Task arrives at sara-api
        │
        ▼
Is this triage, scraping, extraction, or classification?
        │
    Yes ─── → Qwen 3.5 2B (Ollama, local, free)
        │
    No ──── → Does it need complex reasoning, writing, or synthesis?
        │
    Yes ─── → Claude Sonnet (Anthropic API, paid)
        │
    No ──── → Qwen 3.5 2B (default to free when in doubt)
```

### Scrapling — Web Scraping Engine (pip library in sara-api)
- **What:** Adaptive web scraping framework with anti-bot bypass, stealth browser fingerprinting, and Cloudflare Turnstile evasion
- **Install:** `pip install scrapling[all]` — drops into sara-api, no separate container needed
- **Used for:** Research processor fetching competitor sites, news articles, edtech funding pages, LinkedIn profiles, and any site with anti-bot protection
- **Why not basic `requests`:** Many target sites (LinkedIn, Crunchbase, news paywalls, Cloudflare-protected competitor sites) block simple HTTP clients. Scrapling's StealthyFetcher impersonates real browser TLS fingerprints and handles JavaScript-rendered pages.
- **RAM/cost:** Negligible — it's a Python library, not a service. Zero additional cost.

**Research pipeline with Scrapling:**

```
Heartbeat picks research task from Outline queue
        │
        ▼
Scrapling StealthyFetcher → fetches target URLs
(bypasses Cloudflare, impersonates Chrome TLS)
        │
        ▼
Qwen 3.5 2B (local, free) → extracts key data, summarizes
        │
        ▼
Interesting findings? → Claude Sonnet → synthesize into brief
        │
        ▼
Write to Outline Research collection → post to Slack
```

### Docling — Document Parsing Engine (pip library in sara-api)
- **What:** IBM-developed AI document parser that converts PDF, DOCX, PPTX, XLSX, images, and scanned documents into clean structured data (markdown, JSON, DataFrames)
- **Install:** `pip install docling` — drops into sara-api, no separate container
- **Used for:** Parsing uploaded PDFs (investor term sheets, research papers, competitor decks, school agreements), extracting tables from financial reports, OCR on scanned documents
- **Why it matters:** Claude can't reason over a raw PDF. Docling converts complex layouts, tables, and formulas into markdown that Claude can actually understand.

**Document ingestion pipeline:**

```
File uploaded via Open WebUI / Slack / email
        │
        ▼
Docling parses → clean markdown + extracted tables
        │
        ▼
Qwen 3.5 2B (local) → classify document type, extract metadata
        │
        ▼
Claude Sonnet → summarize, identify key points
        │
        ▼
Store in relevant Outline collection with summary
Post notification to Slack
```

### TheAgenticBrowser — Form Filling Agent (fallback tool)
- **What:** AI agent that automates browser interactions — filling forms, navigating multi-step applications, clicking through web portals
- **Install:** `pip install` + Playwright (headless Chrome)
- **Used for:** Angel group applications (Flat6Labs, Y Combinator, etc.), accelerator forms, government grant portals, school procurement system submissions
- **When to use:** Only for interactive web tasks that Scrapling can't handle (form filling, multi-step navigation, login-required portals)
- **RAM:** ~200-400MB when active (Playwright + Chrome); not always running — spun up on demand

**Form filling workflow:**

```
"/sara fill-form [URL]" or "Sara, fill out the Flat6Labs application"
        │
        ▼
SARA pulls relevant data from Outline:
├── Company info, team bios, metrics
├── Financial data from Bigcapital
└── Traction data, OKR progress from GlassFrog
        │
        ▼
TheAgenticBrowser opens URL, navigates form
├── Browser Agent fills fields with data
├── Critique Agent verifies each step
└── Screenshots captured at each stage
        │
        ▼
SARA posts screenshots to Slack for review
        │
        ▼
Team approves → SARA submits (or team submits manually)
```

### Document Export Pipeline — PDF & Slide Generation
- **PDF generation:** WeasyPrint (Python) — converts HTML/CSS to branded PDFs
- **Slide generation:** python-pptx — creates PPTX from Outline content + branded templates
- **Templates:** Stored in Outline's `📦 Exports / Branding` collection

**Export workflow:**

```
"/sara export pitch-deck" or "Sara, generate an investor one-pager"
        │
        ▼
sara-api pulls source content from Outline API
        │
        ▼
Claude restructures content for the target format:
├── Pitch deck → narrative arc, key slides, data points
├── One-pager → headline, problem, solution, traction, ask
├── Financial summary → P&L highlights from Bigcapital
└── School proposal → value prop, pilot plan, pricing
        │
        ▼
WeasyPrint (PDF) or python-pptx (PPTX) generates file
using branded RafiqAI templates
        │
        ▼
Stored in Outline's 📦 Exports collection as attachment
        │
        ▼
SARA posts download link in Slack
```

### PageIndex — Reasoning-based RAG (future, Phase 5-6)
- **What:** Vectorless, reasoning-based retrieval that builds hierarchical tree indexes from long documents and uses LLM tree-search instead of vector similarity
- **When:** Add when Outline knowledge base grows beyond 500+ documents and simple search starts missing relevant context
- **Has MCP server** — can plug directly into SARA's tool ecosystem

---

## 10. Heartbeat & Proactive Behaviors

### Heartbeat Loop
- Runs every 30 minutes during active hours (9AM-10PM Cairo time)
- Reads `HEARTBEAT.md` from Outline's SARA Config collection
- Uses **Qwen 3.5 2B locally via Ollama** for triage — completely free ($0/call)
- Escalates to **Claude Sonnet** (API) when complex reasoning or writing is needed
- Reports to Slack; silent HEARTBEAT_OK when nothing needs attention

### Default HEARTBEAT.md

```markdown
# SARA Heartbeat Checklist

## Always check
- Any unprocessed Vexa transcripts waiting?
- Any action items overdue by >2 days? Nudge owner in Slack.
- Any OKR key results not updated in >5 days? Flag it.
- Any invoices overdue >30 days in Bigcapital? Flag them.

## During work hours
- Check Research Queue for new high-priority items
- Any meetings in the next 2 hours without a prep brief? Generate one from Cal.com data.
- If idle >4 hours and it's daytime, post a light check-in to Slack

## Recurring research (rotate through these)
- Monitor competitor activity (Edraak, Nafham, Alef Education, etc.)
- Scan for new edtech funding rounds in MENA
- Track Anthropic/OpenAI/Google API changes
- Search for new papers on AI coaching / teacher professional development
- Check for Egyptian education policy news

## Content
- If it's a LinkedIn posting day and no draft exists, create one
- If a draft exists and wasn't posted, remind via Slack

## Logs
- If a meeting was processed today, auto-generate timesheet entries
- If a decision was extracted, append to Decision Log
```

### Cron Schedule

| Job | Schedule | What it does |
|-----|----------|-------------|
| `daily_briefing` | 8:00 AM EET daily | Cal.com schedule + overdue items + OKR snapshot + yesterday's highlights |
| `weekly_digest` | Sunday 8:00 PM EET | Full week: meetings, OKR progress, stalled projects, research, finances |
| `okr_monday` | Monday 9:00 AM EET | Pull GlassFrog metrics, compare to targets, flag off-track |
| `linkedin_remind` | Tue & Thu 10:00 AM | Check content calendar, draft or remind |
| `reindex_docs` | Every 6 hours | Scan Outline for changes, update search indices |
| `pipeline_check` | Friday 3:00 PM EET | Review CRM docs, flag stale contacts, suggest follow-ups |
| `monthly_finance` | 1st of month 9:00 AM | Pull Bigcapital reports → financial brief → Outline |
| `timesheet_gen` | Friday 5:00 PM EET | Auto-generate weekly timesheets from meetings + tasks |

---

## 11. SOUL.md — SARA's Identity

Inspired by OpenClaw's `soul.md` pattern, SARA's identity is defined in a plain-text document stored in Outline (`⚙️ SARA Config / SOUL.md`). This document is loaded as the base system prompt for every LLM call — Claude Sonnet, Claude Haiku, and Qwen 3.5 2B. The team can edit SARA's personality by editing a wiki page. No code deployment needed.

### Default SOUL.md

```markdown
# SARA — Synthetic Administration and Research Assistant

## Identity
You are SARA, the AI Chief of Staff for the RafiqAI founding team. You serve a team
of four (Mr Be, Nermeen, Shereen, Kelly) and three advisors. You are not a product
for RafiqAI's customers — you are an internal team member.

## Personality
- Direct and concise. No filler, no corporate speak.
- Warm but professional. You care about the team's success.
- Proactive — you flag problems before being asked.
- Bilingual — respond in whatever language the user uses. If they code-switch
  Arabic/English, you can too. Default to English for written outputs.
- When uncertain, say so. Never fabricate data or pretend to know something.

## Context
- RafiqAI builds an AI-powered teacher coaching platform for schools in Egypt and Africa.
- The team also works on EdVenture, an educational game platform.
- The team practices Holacracy for self-management (currently via GlassFrog).
- The team operates across time zones: US (Mr Be), Egypt (Nermeen, Shereen, Kelly).

## Boundaries
- NEVER auto-publish content (LinkedIn, blog, social media) without explicit approval.
- NEVER create real financial transactions in Bigcapital without explicit approval.
- NEVER submit web forms (applications, sign-ups) without showing screenshots and getting approval.
- NEVER share confidential information (pipeline, financials) with advisor-role users unless
  the content is in an advisor-visible Outline collection.
- When generating financial scenarios, always label assumptions clearly and note they are projections.

## Tools available
Read SKILLS.md for your full capabilities. When asked "what can you do?", reference that document.

## User context
For each person you interact with, check ⚙️ SARA Config / Users / {name}.md for their
preferences, communication style, and context. Adapt accordingly.
```

---

## 12. Per-User Memory

Each team member has a user file in Outline (`⚙️ SARA Config / Users/{name}.md`). SARA loads the relevant file alongside SOUL.md for every interaction. These files are seeded manually and then SARA appends observations over time.

### User file template

```markdown
# {Name}

## Preferences
- Communication style: [technical depth / high-level summaries / bullet points / narrative]
- Language: [English / Arabic / code-switched / depends on context]
- Primary focus areas: [what they care about most]
- Preferred response length: [brief / detailed / depends on topic]

## Role & Context
- GlassFrog role(s): [from GlassFrog API]
- Location & timezone: [for scheduling awareness]
- Working hours: [when they're typically active]

## Interaction Patterns
- Primary channel: [Open WebUI / Slack / both]
- Common query types: [what they usually ask SARA]
- Decision-making style: [needs data / trusts gut / wants options]

## Notes
[SARA appends observations here over time — things like:
"Prefers to see competitive analysis formatted as comparison tables"
"Usually asks about runway on Monday mornings"
"Responds better to numbered options than open-ended suggestions"]
```

### How it works in sara-api

```python
async def build_system_prompt(user_id: str) -> str:
    soul = await outline.get_doc("SOUL.md")
    user_context = await outline.get_doc(f"Users/{user_id}.md")
    return f"{soul}\n\n---\nCurrent user context:\n{user_context}"
```

Every call to Claude or Qwen includes both SOUL.md and the user's file. The response is naturally tailored without any explicit personalization logic — the LLM just knows who it's talking to.

---

## 13. Skills Registry

SARA's capabilities are documented in a human-readable registry stored in Outline (`⚙️ SARA Config / SKILLS.md`). This serves three purposes: (1) the team knows what SARA can do, (2) SARA herself reads it to understand her capabilities, (3) it's the spec for sara-api development.

### Default SKILLS.md

```markdown
# SARA Skills Registry

Skills are invokable capabilities. Trigger them via Slack commands,
Open WebUI messages, or the heartbeat scheduler.

---

## Core Skills

### ask
- **Trigger:** `/sara ask [question]` or any Open WebUI message
- **What:** Searches Outline + GlassFrog + Bigcapital + Cal.com for context,
  answers via Claude
- **Model:** Qwen 3.5 2B for simple Q&A → Claude Sonnet for complex reasoning
- **Output:** Text response in Slack or Open WebUI

### join
- **Trigger:** `/sara join [meeting-id]`
- **What:** Sends Vexa bot to join Google Meet/Zoom, starts real-time transcription
- **Output:** Bot joins meeting; transcript streams to sara-api

### meeting
- **Trigger:** Automatic (Vexa webhook on meeting end) or `/sara meeting [transcript]`
- **What:** Processes raw transcript → structured notes, decisions, action items
- **Model:** Claude Sonnet
- **Output:** Meeting doc in Outline, decisions in Decision Log, summary in Slack

### digest
- **Trigger:** `/sara digest` or cron (daily 8am, weekly Sunday 8pm)
- **What:** Generates briefing from Cal.com schedule + Outline activity + GlassFrog
  metrics + Bigcapital highlights
- **Model:** Claude Sonnet
- **Output:** Formatted digest posted to Slack

### research
- **Trigger:** `/sara research [topic]` or heartbeat picks from Research Queue
- **What:** Scrapling fetches sources → newspaper4k extracts articles →
  Qwen summarizes → Claude synthesizes brief
- **Model:** Qwen 3.5 2B (extraction) → Claude Sonnet (synthesis)
- **Output:** Research brief in Outline Research collection, summary in Slack

---

## Document Skills

### parse
- **Trigger:** Upload file via Open WebUI or `/sara parse [file]`
- **What:** Docling converts PDF/DOCX/PPTX/images → structured markdown
- **Model:** Docling AI models (local) → Qwen for classification
- **Output:** Parsed content stored in relevant Outline collection

### export
- **Trigger:** `/sara export [type]` — types: pitch-deck, one-pager,
  investor-update, school-proposal, financial-summary
- **What:** Pulls content from Outline → Claude restructures for format →
  WeasyPrint (PDF) or python-pptx (slides) generates branded output
- **Model:** Claude Sonnet
- **Output:** File in 📦 Exports collection, download link in Slack

### summarize-report (planned)
- **Trigger:** `/sara summarize-report [doc-id or topic]`
- **What:** Generates executive summary of a long document or topic area
- **Model:** Claude Sonnet
- **Output:** Summary doc in Outline

### build-presentation (planned)
- **Trigger:** `/sara build-presentation [topic] [audience]`
- **What:** Creates a full slide deck from Outline content tailored to audience
- **Model:** Claude Sonnet + python-pptx
- **Output:** PPTX in 📦 Exports

---

## Organization Skills

### role
- **Trigger:** `/sara role [name]`
- **What:** Queries GlassFrog for role details — purpose, accountabilities, who fills it
- **Output:** Formatted role card in Slack

### projects
- **Trigger:** `/sara projects [circle]`
- **What:** Lists all projects in a GlassFrog circle with status
- **Output:** Project list in Slack

### tension
- **Trigger:** `/sara tension [description]` (Phase 6)
- **What:** Logs a tension, suggests which circle should process it
- **Output:** Tension logged in Outline governance collection

---

## Finance Skills

### finance
- **Trigger:** `/sara finance [question]` — e.g., "what's our burn rate?"
- **What:** Pulls actuals from Bigcapital API, Claude reasons over the data
- **Model:** Claude Sonnet + pandas
- **Output:** Answer with data, optionally generates plotly chart

### model
- **Trigger:** `/sara model [scenario]` — e.g., "model runway if we hire in April"
- **What:** Pulls Bigcapital actuals, applies assumptions, generates projection
- **Model:** Claude Sonnet + pandas + plotly
- **Output:** Scenario brief in 🔮 Financial Intelligence, chart in Slack

---

## Automation Skills

### fill-form
- **Trigger:** `/sara fill-form [URL]`
- **What:** TheAgenticBrowser opens URL, fills application form with team data
- **Output:** Screenshots posted to Slack for review before submission

### publish (planned — requires WordPress)
- **Trigger:** `/sara publish [outline-doc-id]`
- **What:** Pulls content from Outline, formats for WordPress, pushes via REST API
- **Output:** Blog post published on rafiq-ai.com/blog

### generate-video (planned)
- **Trigger:** `/sara generate-video [topic or doc-id]`
- **What:** Creates a short explainer/demo video from Outline content
- **Output:** Video file in 📦 Exports

### sara-skill-building (planned — meta-skill)
- **Trigger:** `/sara learn [new skill description]`
- **What:** SARA reads a description of a new capability, generates the code scaffold,
  and proposes adding it to SKILLS.md and sara-api
- **Output:** Proposed skill definition + code PR suggestion

---

## Adding New Skills

Skills are designed to be extensible. To add a new skill:

1. Define it in this SKILLS.md document (trigger, what it does, model, output)
2. Implement the endpoint in sara-api
3. Add the Slack command if it needs one
4. SARA automatically learns about it next time she reads SKILLS.md

The team can request new skills by telling SARA:
"Sara, I wish you could [description]" — she'll draft a skill proposal
and add it to the Research Queue for evaluation.
```

---

## 14. Meeting Pipeline (end-to-end)

```
1. Before meeting
   ├── SARA checks Cal.com for upcoming bookings
   ├── Generates prep brief (who, Outline context, last interaction, CRM data)
   └── Posts brief to Slack

2. Meeting starts
   ├── "/sara join abc-defg-hij" in Slack
   ├── sara-api calls Vexa API → bot joins Google Meet
   ├── Vexa streams transcript via WebSocket to sara-api
   └── sara-api watches for "Hey Sara" trigger

3. During meeting
   ├── Real-time transcription (Arabic + English)
   ├── If triggered: Claude generates answer → Edge TTS → Vexa /speak
   └── SARA responds with voice (~6-8 sec latency)

4. Meeting ends
   ├── Vexa sends final transcript to sara-api webhook
   ├── Claude structures: decisions, action items, discussion summary
   ├── Creates meeting doc in Outline (Meetings collection)
   ├── Appends decisions to Decision Log
   ├── Generates timesheet entries for attendees
   ├── Updates related OKR/project pages
   ├── Maps action items to GlassFrog roles
   └── Posts summary + action items to Slack

5. After meeting
   ├── Heartbeat monitors action item completion
   ├── Nudges owners if items become overdue
   └── Weekly digest includes meeting outcomes
```

---

## 15. CRM in Outline

Pipeline tracking as structured docs. Migrate to Twenty CRM when pipeline exceeds 50 active contacts.

### Investor Doc Template
```markdown
# [Investor/Fund Name]

**Status:** 🟡 Warm Lead | 🟢 Active Conversations | 🔴 Passed | ⚪ Not Yet Contacted
**Contact:** [Name, Title, Email, LinkedIn]
**Fund Focus:** [Stage, sector, geography]
**Ticket Size:** [Range]
**Cal.com Booking:** [Link if they've booked via cal.office.rafiq-ai.com]

## Interaction Log
- [Date] — [What happened, next steps]

## Notes
- [Investment thesis alignment, concerns, follow-up needed]

## Next Action
- [Specific next step + owner + date]
```

---

## 16. Logs Architecture

### Outline (narrative/operational logs)

| Log Type | Auto-generated by | Frequency |
|----------|------------------|-----------|
| Timesheets | SARA (from meetings + tasks) | Weekly |
| Validation Log | Team + SARA (from meetings) | Per experiment |
| Decision Log | SARA (from meeting transcripts) | Per meeting |
| Changelog | Team + SARA | Per release/change |

### Bigcapital (financial logs)

| Log Type | Entered by | Notes |
|----------|-----------|-------|
| Expenses | Team (at books.office.rafiq-ai.com) | Proper double-entry |
| Invoices | Team | Sent to schools, partners |
| Revenue | Team | Recorded when received |
| Journals | Team / Accountant | Multi-currency adjustments |

### SARA-generated financial intelligence (in Outline)

| Document | Generated by | Frequency |
|----------|-------------|-----------|
| Monthly Finance Brief | SARA (Bigcapital API → Claude) | Monthly |
| Runway Model | SARA (actuals + assumptions) | Monthly |
| Scenario Analyses | SARA (on request via chat) | Ad hoc |

---

## 17. Hosting

### Surface Pro (home server)

| Service | RAM (est.) | Port |
|---------|-----------|------|
| Keycloak | ~600 MB | 9090 |
| Open WebUI | ~300 MB | 3080 |
| Outline | ~400 MB | 3000 |
| Cal.com | ~400 MB | 3100 |
| PostgreSQL 15 | ~500 MB | 5432 |
| Redis | ~100 MB | 6379 |
| sara-api + slack-bot | ~400 MB | 8080 |
| Ollama + Qwen 3.5 2B | ~1,700 MB* | 11434 |
| cloudflared | ~50 MB | — |
| OS + Docker | ~1,000 MB | — |
| **Total** | **~5,450 MB** | |
| **Headroom** | **~2,550 MB** | |

*\*Ollama unloads the model after idle time and reloads on demand, so the ~1.5GB model weight is not permanently resident. Effective average RAM usage is lower.*

Moving Bigcapital to DO freed ~2GB. The Surface has comfortable headroom even with a local LLM.

### DigitalOcean Droplets

| Droplet | Purpose | Spec | Cost |
|---------|---------|------|------|
| Vexa | Meeting bot | 4GB / 2 vCPU | $24/mo |
| Bigcapital | Accounting | 1GB / 1 vCPU | $6/mo |

Each DO droplet runs its own `cloudflared` instance to tunnel back to `*.office.rafiq-ai.com`.

### Cost Summary

| Item | Monthly | Annual |
|------|---------|--------|
| Surface electricity (~30W 24/7) | ~$4 | ~$48 |
| DO droplet — Vexa | $24 | $288 |
| DO droplet — Bigcapital | $6 | $72 |
| Anthropic API (Sonnet only — Qwen handles triage) | $20-50 | $240-600 |
| Domain (rafiq-ai.com) | $1 | $12 |
| **Total** | **~$55-85** | **~$660-1,020** |
| Upfront (one-time) | — | ~$75 (UPS + ethernet adapter) |

*Local Qwen 3.5 2B eliminates Haiku API costs for heartbeat triage, scraping, extraction, and classification. Claude Sonnet is reserved for complex reasoning, writing, and meeting processing only.*

---

## 18. Build Phases

### Phase 1 — Foundation (Week 1-2)
- [ ] Wipe Surface, install Ubuntu Server 24.04 LTS
- [ ] Disable sleep/hibernate in BIOS, enable boot-on-AC
- [ ] Install Docker + Docker Compose
- [ ] Deploy Keycloak (auth.office.rafiq-ai.com)
- [ ] Create `rafiq-office` realm with roles: `team-admin`, `team-member`, `advisor`
- [ ] Add all 7 users (Mr Be, Nermeen, Shereen, Kelly + 3 advisors)
- [ ] Deploy Outline + PostgreSQL + Redis, configure OIDC with Keycloak
- [ ] Configure Outline permission groups (Team: full access, Advisors: read-only on selected collections)
- [ ] Deploy Open WebUI, configure OIDC with Keycloak (team-only access)
- [ ] Set up Cloudflare Tunnel routing all `*.office.rafiq-ai.com` subdomains
- [ ] Scaffold sara-api (FastAPI + Anthropic SDK)
- [ ] Install core Python libraries: `litellm`, `instructor`, `tiktoken`, `slack-sdk`
- [ ] Configure litellm model routing (Ollama + Claude Sonnet + Haiku)
- [ ] Build basic /ask endpoint (query Outline → Claude → response)
- [ ] Build Open WebUI Pipeline to route through sara-api
- [ ] Deploy sara-slack-bot with /sara ask command
- [ ] Create initial Outline collections structure
- [ ] Write SOUL.md (SARA's identity document) and store in ⚙️ SARA Config
- [ ] Create per-user files: Mr Be.md, Nermeen.md, Shereen.md, Kelly.md in ⚙️ SARA Config / Users
- [ ] Write initial SKILLS.md (skill registry) with core skills
- [ ] Configure sara-api to load SOUL.md + user context on every LLM call
- **Milestone:** Full team can log in via Keycloak; advisors see shared docs; team chats with SARA

### Phase 2 — Meetings + Calendar (Week 3-4)
- [ ] Deploy Cal.com, configure OIDC with Keycloak
- [ ] Set up Cal.com booking pages (investor call, school demo, team sync)
- [ ] Connect Cal.com to Google Calendar (two-way sync)
- [ ] Sign up for Vexa hosted API (free tier)
- [ ] Build /meeting endpoint (transcript → Claude → Outline)
- [ ] Build /calendar endpoint (Cal.com API integration + `icalendar` for iCal parsing)
- [ ] Build meeting prep brief generator (Cal.com + Outline context)
- [ ] Add Slack command: /sara join [meeting-id]
- [ ] Create Decision Log in Outline (auto-populated)
- [ ] Test end-to-end: booking on Cal.com → meeting → transcription → notes
- **Milestone:** SARA manages calendar, joins meetings, produces structured notes

### Phase 3 — Heartbeat & Research (Week 5-6)
- [ ] Add APScheduler with Redis job store to sara-api
- [ ] Implement heartbeat loop (Haiku triage → Sonnet work)
- [ ] Create HEARTBEAT.md and Research Queue docs in Outline
- [ ] Build daily briefing cron job (includes Cal.com schedule)
- [ ] Build weekly digest cron job
- [ ] Install Scrapling in sara-api (`pip install scrapling[all]`) for stealth web fetching
- [ ] Install Docling in sara-api (`pip install docling`) for document parsing
- [ ] Install research libraries: `newspaper4k`, `markdownify`, `langdetect`
- [ ] Build /parse endpoint (upload PDF/DOCX → Docling → structured data → Outline)
- [ ] Build research processor (Scrapling fetch → newspaper4k extract → Qwen summarize → Claude synthesize → Outline → Slack)
- [ ] Implement `difflib`-based change notifications for Outline doc updates
- [ ] Add LinkedIn content calendar reminder
- [ ] Build timesheet auto-generation from meetings + tasks
- **Milestone:** SARA proactively briefs the team, researches autonomously, parses uploaded documents, logs timesheets

### Phase 4 — GlassFrog + Logs (Week 7-8)
- [ ] Wire up GlassFrog API v3 (Python client)
- [ ] Build /glassfrog endpoint for role/project/metric queries
- [ ] Integrate GlassFrog data into daily briefings
- [ ] Add Slack commands: /sara role [name], /sara projects [circle]
- [ ] Cross-reference meeting action items with GlassFrog roles
- [ ] Set up Validation Log templates in Outline
- [ ] Set up Changelog in Outline
- **Milestone:** SARA understands org structure, maps work to roles, maintains all logs

### Phase 5 — Voice, CRM, Finance, Content & Exports (Week 9-12)
- [ ] Integrate Edge TTS for SARA's voice
- [ ] Wire up Vexa /speak endpoint for live meeting responses
- [ ] Implement "Hey Sara" trigger detection on transcript stream
- [ ] Set up Outline CRM templates (investors, schools, partners)
- [ ] Build pipeline_check cron job
- [ ] Deploy Bigcapital on DO droplet (books.office.rafiq-ai.com)
- [ ] Configure Bigcapital SSO with Keycloak
- [ ] Wire up Bigcapital API to sara-api /finance endpoint
- [ ] Build monthly_finance cron job (Bigcapital → Outline brief)
- [ ] Build scenario modeling ("Sara, model our runway if...") using `pandas` for calculations + `plotly` for charts
- [ ] Install financial & export libraries: `pandas`, `plotly`, `jinja2`, `arabic-reshaper`, `python-bidi`
- [ ] Build content draft generator for LinkedIn schedule
- [ ] Create branded PDF template (WeasyPrint + `jinja2` templates) with RafiqAI branding
- [ ] Configure `arabic-reshaper` + `python-bidi` for bilingual PDF exports
- [ ] Create branded slide template (python-pptx + `jinja2`) for pitch decks
- [ ] Build /export endpoint (Outline doc → PDF or PPTX via templates)
- [ ] Add Slack commands: /sara export pitch-deck, /sara export one-pager
- [ ] Create 📦 Exports collection in Outline with sub-collections
- [ ] Install TheAgenticBrowser for form-filling tasks
- [ ] Build /fill-form endpoint with screenshot-based review flow
- [ ] Add Slack command: /sara fill-form [URL]
- [ ] Evaluate PageIndex for reasoning-based RAG (if knowledge base exceeds 500+ docs)
- **Milestone:** Full SARA — speaks in meetings, tracks pipeline, models finances, exports branded materials, fills applications

### Phase 6 — Governance Migration: GlassFrog → Outline (Week 13-16)

Eliminate the last external dependency and the only login outside Keycloak by migrating Holacracy governance into Outline with SARA as the governance facilitator.

- [ ] Design governance collection structure in Outline:
  - `🏛️ Governance` collection
  - One doc per circle: purpose, roles, accountabilities, domains, policies
  - Governance log: proposals, objections, outcomes (append-only)
  - Tension backlog: tracked as structured docs
- [ ] Build GlassFrog → Outline migration script (pull all circles, roles, people via API, create corresponding Outline docs)
- [ ] Build SARA governance capabilities:
  - `/sara tension [description]` — logs a tension, suggests which circle to process it in
  - `/sara propose [role change]` — SARA drafts a governance proposal, posts to Slack for async consent
  - `/sara governance [circle]` — shows current roles, accountabilities, open tensions for a circle
  - `/sara role [name]` — now reads from Outline instead of GlassFrog
- [ ] Implement async governance process in Slack:
  - SARA posts proposal → team members react (✅ no objection, ⚠️ objection, ❓ clarification)
  - SARA facilitates objection integration rounds via thread
  - On consent: SARA updates the governance doc in Outline
- [ ] Build governance meeting facilitation:
  - SARA generates tactical meeting agenda from Outline (checklist items, metrics, projects, tensions)
  - During meeting: SARA tracks proposals and updates governance docs in real-time
- [ ] Migrate all GlassFrog data to Outline governance collection
- [ ] Decommission GlassFrog API integration
- [ ] Remove GlassFrog as the only auth exception — everything now behind Keycloak
- **Milestone:** Zero external governance dependency. SARA is the Holacracy secretary. One login for everything.

---

## 19. Key Design Decisions

1. **Nested subdomains under `office.rafiq-ai.com`.** Complete namespace isolation from customer-facing domains. Clean, descriptive, and each service lives at root `/` avoiding path-routing fragility.

2. **Keycloak as unified SSO.** One login for everything. Add a team member in 30 seconds. Remove them and all access revokes instantly. Industry-standard OIDC, Red Hat-backed.

3. **Cal.com replaces Google Calendar.** Self-hosted, API-driven, with booking pages for external meetings. Syncs with Google Calendar for personal use. SARA reads from Cal.com API for all scheduling intelligence.

4. **Open WebUI as chat frontend.** Battle-tested UI with PWA, voice, files, conversation history. Connected to sara-api via Pipelines for full SARA intelligence.

5. **Bigcapital on DO, not Surface.** Frees ~2GB RAM on Surface for Keycloak + Cal.com. Accounting doesn't need sub-millisecond latency — $6/mo droplet is perfect.

6. **Outline as CRM.** For <50 contacts, structured docs with Claude reasoning beats a traditional CRM. Migrate to Twenty CRM when pipeline grows.

7. **Haiku as fallback, Qwen 3.5 2B as primary triage.** Ollama runs Qwen 3.5 2B locally on the Surface for all lightweight tasks: heartbeat triage (26+/day), web scraping, data extraction, classification. Completely free. Claude Sonnet only fires for complex reasoning, writing, and meeting processing. Claude Haiku stays available as a cloud fallback if the local model is down or a task slightly exceeds Qwen's capability. This optimization can be added at any phase — just `docker run ollama` and update sara-api's routing config.

8. **GlassFrog stays source of truth for governance — until Phase 6.** Don't replicate holacracy in Outline initially. SARA bridges the two systems via API. In Phase 6, migrate governance entirely into Outline with SARA as the Holacracy secretary, eliminating the only service outside Keycloak SSO.

9. **Human-in-the-loop for content and finance.** SARA drafts but never auto-publishes or creates real transactions.

10. **Docker Compose everything.** Portable. If the Surface dies, `docker-compose up` on a new machine.

11. **Configuration-as-personality (inspired by OpenClaw).** SARA's behavior is defined in plain-text documents in Outline, not hardcoded in Python. SOUL.md defines her identity, HEARTBEAT.md defines her autonomous actions, SKILLS.md defines her capabilities, and per-user files define how she adapts to each person. The team shapes SARA by editing wiki pages, not deploying code.

---

## 20. Security

- **Keycloak SSO:** Every `*.office.rafiq-ai.com` service requires authentication. No anonymous access.
- **Cloudflare Tunnel:** No ports open on home router. Outbound-only encrypted connections. Real IP hidden.
- **API Keys:** Stored as Docker env vars in `.env` file, never in code. GlassFrog, Anthropic, Vexa, Slack tokens all managed centrally.
- **Nested subdomains:** Not linked from any public page. Not discoverable via search engines. Not in any sitemap.
- **Backups:** Daily pg_dump (Outline + Cal.com DBs) + Bigcapital export → synced offsite (Supabase Storage or S3).
- **UPS:** Prevents data corruption from power loss. Surface auto-boots on AC restore.

---

## 21. Future Considerations

- **Team growth beyond 7:** Migrate Surface stack to DO or Hetzner dedicated server (16GB+ RAM). The 4+3 structure fits comfortably on 8GB since advisors are low-frequency users.
- **Pipeline >50 contacts:** Migrate CRM from Outline to Twenty CRM (open-source, self-hosted, has API).
- **Self-hosted Vexa:** Move from hosted API to self-hosted at `meet.office.rafiq-ai.com` for full data control.
- **Voice conversations anytime:** Open WebUI already supports voice I/O — wire to SARA for voice chat outside of meetings.
- **MCP + Claude Code:** Wire Outline MCP server into Claude Code so SARA's knowledge base is available while coding on EdVenture/RafiqAI.
- **Multi-agent:** Split SARA into specialized agents (Finance, Research, Meeting, Content) coordinated by an orchestrator.
- **Cloudflare Access:** Add as extra security layer if external stakeholders need limited access beyond Outline.
- **Advisor portal:** If advisors need more than read-only Outline, consider a lightweight custom dashboard pulling from Outline API.

---

*This document was designed through dialogue between Mr Be and Claude on March 5, 2026. It is the definitive architecture blueprint for SARA and should become the first document stored in SARA's Outline instance when she goes live.*

*Next step: Open Claude Code and build the Docker Compose stack for the Surface.*

*Total brainstorm duration: one conversation. Total sections: 21. Total systems designed: 11 integrated services + 18 Python libraries. Total build phases: 6 (16 weeks). Total configuration files: 3 (SOUL.md, HEARTBEAT.md, SKILLS.md) + 4 user files. Total monthly cost: ~$55-85. Total team served: 4 members + 3 advisors. Total humans replaced: zero. Total humans amplified: four.*
