# SARA — Synthetic Administration and Research Assistant

## Architecture & Build Blueprint v5.0 (Final — Ready for Claude Code)

**Project:** Internal AI-powered operating system for the RafiqAI founding team
**Authors:** Mr Be + Claude (brainstorm session, March 2026)
**Status:** Design complete — ready to build with Claude Code
**Domain:** `*.office.rafiq-ai.com` (nested subdomain, isolated from customer-facing domains)

-----

## 1. What is SARA?

SARA is an AI Chief of Staff for the RafiqAI founding team. She is a self-hosted system that combines a knowledge base, accounting, calendar scheduling, meeting intelligence, holacracy integration, proactive research, CRM, and operational logging into a single AI-powered platform — unified behind one SSO login and accessible via a polished chat UI, Slack, and voice in meetings.

SARA is not a product for RafiqAI’s customers. She is an internal tool that makes a 4-person founding team (plus 3 advisors) operate like a 15-person one.

### What SARA does

- **Knows everything:** Stores and reasons over all team docs, meeting notes, OKRs, decisions, financial data, and project context
- **Attends meetings:** Joins Google Meet/Zoom as a bot, transcribes (Arabic + English code-switching), structures notes, extracts action items
- **Speaks in meetings:** Answers questions live with voice (“Hey Sara, are we on track for the pilot?”)
- **Manages the org:** Reads and writes to GlassFrog (holacracy roles, circles, projects, metrics, tensions)
- **Tracks finances:** Pulls real data from Bigcapital (P&L, balance sheet, expenses) and models future scenarios with Claude
- **Manages scheduling:** Owns the team calendar via Cal.com — booking pages for investors, meeting coordination, automated reminders
- **Tracks the pipeline:** Maintains investor, school, and partner contacts as structured docs in Outline
- **Keeps the books:** Logs timesheets (auto-generated from meetings), validation records, and team decisions
- **Researches proactively:** During idle time, scans the web for competitors, funding news, edtech trends, AI developments
- **Reminds and nudges:** Posts daily briefings, weekly digests, LinkedIn posting reminders, overdue action item alerts
- **Converses naturally:** Via Open WebUI (polished ChatGPT-like interface), Slack bot, or voice — grounded in the full knowledge base
- **Unified login:** All services behind Keycloak SSO — one account, one login, access everything

-----

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
│  obs.office.rafiq-ai.com    → OpenLIT (LLM obs) │
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
- **Cloudflare proxy** — real server IP is hidden behind Cloudflare’s network
- **All HTTPS** — Cloudflare handles SSL termination automatically

-----

## 3. Architecture Overview

```
SARA Ecosystem — Single Droplet + Surface Ollama Node
═════════════════════════════════════════════════════

DigitalOcean — Single 8GB Droplet ($48/mo)         Surface Pro (i5-7300U / 8GB)
8GB RAM / 4 vCPU (shared)                           Ubuntu Server — headless, SSH only
Ubuntu Server 24.04 LTS                              │
│                                                    ├── Ollama
├── Keycloak            (SSO, port 9090)             │   ├── Qwen 3.5 2B / Gemma 3 1B
├── Outline             (wiki, port 3000)            │   ├── Free inference for triage
├── Open WebUI          (chat, port 3080)            │   ├── Auto-unloads after idle
├── Cal.com             (calendar, port 3100)        │   └── OpenAI-compatible :11434
├── sara-api            (brain, port 8080)           │
│   ├── FastAPI server                               └── autossh reverse tunnel
│   ├── Slack bot (built-in)                             Surface:11434 → DO:11434
│   ├── APScheduler (cron jobs)                          (Ollama appears local to sara-api)
│   ├── Heartbeat loop
│   └── Research processor
├── Vexa Lite           (meeting bot, port 8056)
│   └── Transcription via Groq Whisper API
├── Bigcapital          (accounting, port 5000)
│   ├── MariaDB         (port 3306)
│   ├── MongoDB          (port 27017)
│   └── Redis 2          (port 6380)
├── OpenLIT             (observability, port 3300)
│   ├── ClickHouse       (port 8123)
│   └── OTel Collector   (port 4317)
├── PostgreSQL 15       (shared DB, port 5432)
├── Redis               (shared cache, port 6379)
└── Cloudflared         (tunnel daemon)


External SaaS (API calls only — no containers)
├── OpenRouter       LLM gateway (500+ models, one API key)
├── Groq             Whisper transcription ($0.04/hr)
├── GlassFrog        Holacracy governance
└── Slack            Team communication


LLM Routing — Three-Tier Model Strategy
────────────────────────────────────────
Tier 1: Surface Ollama (free, ~3-5 tok/sec)
  └── Heartbeat triage, classification, tagging, yes/no decisions

Tier 2: OpenRouter cheap models (Haiku, Gemini Flash)
  └── Research summaries, voice Q&A, briefing drafts

Tier 3: OpenRouter premium models (Sonnet, GPT-4o)
  └── Meeting processing, financial modeling, investor comms

Fallback chain: Surface down → OpenRouter cheap
               OpenRouter down → Surface (degraded)
               Both down → queue task, alert Slack
```

-----

## 4. Technology Stack

|Component              |Technology          |Domain                             |Where hosted        |
|-----------------------|--------------------|-----------------------------------|--------------------|
|SSO / Auth             |Keycloak            |auth.office.rafiq-ai.com           |Surface             |
|Chat UI                |Open WebUI          |chat.office.rafiq-ai.com           |Surface             |
|Knowledge base         |Outline             |wiki.office.rafiq-ai.com           |Surface             |
|Calendar               |Cal.com             |cal.office.rafiq-ai.com            |Surface             |
|Accounting             |Bigcapital          |books.office.rafiq-ai.com          |DO droplet ($6/mo)  |
|Meeting bot            |Vexa                |meet.office.rafiq-ai.com (future)  |DO droplet ($24/mo) |
|AI service             |FastAPI + Claude SDK|internal (port 8080)               |Surface             |
|Database (Outline, Cal)|PostgreSQL 15       |internal                           |Surface             |
|Database (Bigcapital)  |MariaDB + MongoDB   |internal                           |DO droplet          |
|Cache / Queue          |Redis               |internal                           |Surface + DO droplet|
|LLM gateway            |OpenRouter          |openrouter.ai (SaaS)               |—                   |
|LLM observability      |OpenLIT             |obs.office.rafiq-ai.com (port 3300)|DO main             |
|Web scraping           |Scrapling           |internal (pip library)             |DO main             |
|Document parsing       |Docling (IBM)       |internal (pip library)             |DO main             |
|Form filling           |TheAgenticBrowser   |internal (pip + Playwright)        |DO main             |
|PDF generation         |WeasyPrint          |internal (pip library)             |DO main             |
|Slide generation       |python-pptx         |internal (pip library)             |DO main             |
|STT                    |Whisper (via Vexa)  |—                                  |DO Vexa             |
|TTS                    |Edge TTS (Python)   |—                                  |DO main             |
|Holacracy              |GlassFrog API v3    |glassfrog.com (SaaS)               |—                   |
|Tunnel                 |Cloudflare Tunnel   |*.office.rafiq-ai.com              |DO main + DO Vexa   |
|Scheduling             |APScheduler + Redis |internal                           |DO main             |
|Slack                  |Slack Bolt (Python) |—                                  |DO main             |
|Containerization       |Docker Compose      |—                                  |All                 |

### LLM Strategy — OpenRouter + Model Profiles

All LLM calls go through **OpenRouter** (openrouter.ai) — a unified API gateway that gives SARA access to 500+ models from every major provider through one API key and one billing account. No vendor lock-in. Switch models by changing a config value, not code.

**Why OpenRouter instead of direct API keys:**

- One API key, one credit balance, one invoice — regardless of how many providers SARA uses
- Automatic fallback: if Claude is down, OpenRouter can route to GPT-4o or Gemini automatically
- No markup on provider pricing — you pay exactly what each provider charges
- Model experimentation: try DeepSeek, Qwen, Mistral, Llama without new API accounts
- OpenAI-compatible API — works with litellm, instructor, and Open WebUI out of the box

**Model Profile Configuration** (stored in Outline `⚙️ SARA Config / MODEL_PROFILES.md`):

The team can swap models per task type by editing a wiki document. No code deployment needed.

```yaml
# MODEL_PROFILES.md — SARA reads this on every heartbeat
# Change any model and SARA picks it up within 30 minutes

profiles:
  triage:
    # Lightweight classification, heartbeat checks, yes/no decisions
    model: "anthropic/claude-haiku"
    max_tokens: 500
    temperature: 0.1
    fallback: "google/gemini-2.0-flash"

  extraction:
    # Web scraping summaries, data extraction, document classification
    model: "anthropic/claude-haiku"
    max_tokens: 1000
    temperature: 0.0
    fallback: "deepseek/deepseek-chat-v3"

  reasoning:
    # Meeting processing, research synthesis, financial modeling, digest writing
    model: "anthropic/claude-sonnet"
    max_tokens: 4000
    temperature: 0.3
    fallback: "openai/gpt-4o"

  creative:
    # Blog drafts, pitch deck narratives, investor communications
    model: "anthropic/claude-sonnet"
    max_tokens: 4000
    temperature: 0.7
    fallback: "anthropic/claude-haiku"

  voice:
    # Meeting Q&A — needs speed (low latency) over depth
    model: "anthropic/claude-haiku"
    max_tokens: 500
    temperature: 0.2
    fallback: "google/gemini-2.0-flash"
```

**How it works in sara-api:**

```python
import openlit
from litellm import completion

openlit.init()  # One line — auto-instruments all LLM calls

async def call_llm(profile: str, messages: list, user_id: str):
    config = await load_model_profiles()  # reads from Outline
    profile_config = config["profiles"][profile]

    response = completion(
        model=f"openrouter/{profile_config['model']}",
        messages=messages,
        max_tokens=profile_config["max_tokens"],
        temperature=profile_config["temperature"],
        metadata={"profile": profile, "user": user_id},  # tracked in OpenLIT
    )
    return response
```

### OpenLIT — LLM Observability

**OpenLIT** is a lightweight, self-hosted, open-source observability platform that tracks every LLM call SARA makes. It answers: “How much are we spending per task type? Which model is fastest? Where are we wasting tokens?”

- **Dashboard:** Token usage, cost per call, latency, error rates — all filterable by model, profile, and time range
- **Integration:** One line of code (`openlit.init()`) auto-instruments all litellm/OpenAI calls via OpenTelemetry
- **Stack:** 3 Docker containers — OpenLIT app + ClickHouse (analytics DB) + OpenTelemetry Collector
- **RAM footprint:** ~500-700MB total (lightweight compared to Langfuse v3 which needs 4GB+)
- **Domain:** `obs.office.rafiq-ai.com` (SSO via Keycloak)
- **Retention:** 90 days of trace data (configurable)

**What gets tracked per LLM call:**

- Model used, profile name, tokens in/out, cost, latency
- User who triggered it (Mr Be, Nermeen, etc.)
- Task type (heartbeat, meeting, research, digest, ask)
- Success/failure, retry count

This lets the team make data-driven decisions about model selection: “Haiku handles 95% of triage correctly and costs 1/10th of Sonnet — keep it. But for research synthesis, Sonnet produces noticeably better briefs — worth the cost.”

### Python Utility Libraries (pip install, zero infrastructure)

All of these live inside sara-api as pip dependencies. No new containers, no RAM impact beyond negligible.

|Library                          |Purpose                                                                                                          |When to add|
|---------------------------------|-----------------------------------------------------------------------------------------------------------------|-----------|
|**litellm**                      |Unified LLM API — routes all calls through OpenRouter. Model switching, fallbacks, token tracking.               |Phase 1    |
|**openlit**                      |LLM observability SDK — auto-instruments all LLM calls, sends traces to OpenLIT dashboard. One line of code.     |Phase 1    |
|**instructor**                   |Structured output from LLMs via Pydantic models. Guarantees clean JSON for action items, contact extraction, etc.|Phase 1    |
|**newspaper4k**                  |Clean article extraction from news URLs — title, text, authors, date. For research pipeline.                     |Phase 3    |
|**markdownify**                  |HTML → clean markdown conversion. Glue between Scrapling web fetches and Outline storage.                        |Phase 3    |
|**jinja2**                       |Template engine for PDF/PPTX generation. Powers `{{ company_name }}` in export templates.                        |Phase 5    |
|**pandas**                       |Data analysis and financial modeling. Burn rate, runway projections, unit economics.                             |Phase 5    |
|**plotly**                       |Interactive charts — burn rate graphs, OKR progress, pipeline funnels. Embeddable in Outline.                    |Phase 5    |
|**arabic-reshaper + python-bidi**|Proper Arabic text rendering in exported PDFs. Essential for bilingual MENA materials.                           |Phase 5    |
|**langdetect**                   |Detect Arabic vs English in code-switched text. Helps segment bilingual meeting transcripts.                     |Phase 3    |
|**icalendar**                    |Parse/generate iCal files for Cal.com webhook processing and calendar sync.                                      |Phase 2    |
|**slack-sdk**                    |Extended Slack features — file uploads (share PDFs in Slack), Block Kit (interactive buttons), threading.        |Phase 1    |
|**difflib** (stdlib)             |Text diff comparison. Meaningful change notifications when Outline docs are updated.                             |Phase 3    |

-----

## 5. Container Inventory & Docker Compose

Everything runs in a single `docker-compose.yml` on one DigitalOcean droplet. 17 containers total — 16 official images pulled, 1 custom image built.

### Official images (pull and run, zero code changes)

|#|Container     |Image                                             |Purpose                                        |RAM    |Port|
|-|--------------|--------------------------------------------------|-----------------------------------------------|-------|----|
|1|postgres      |`postgres:15-alpine`                              |Shared DB: Outline, Cal.com, sara-api, Keycloak|~500 MB|5432|
|2|redis         |`redis:7-alpine`                                  |Shared cache: Outline, APScheduler, sara-api   |~100 MB|6379|
|3|keycloak      |`quay.io/keycloak/keycloak:latest`                |SSO / OIDC identity provider                   |~600 MB|9090|
|4|outline       |`docker.getoutline.com/outlinewiki/outline:latest`|Knowledge base, wiki, CRM, config store        |~400 MB|3000|
|5|cloudflared   |`cloudflare/cloudflared:latest`                   |Tunnel: routes *.office.rafiq-ai.com           |~50 MB |—   |
|6|clickhouse    |`clickhouse/clickhouse-server:latest`             |Analytics DB for OpenLIT traces                |~300 MB|8123|
|7|otel-collector|`otel/opentelemetry-collector:latest`             |Receives traces, forwards to ClickHouse        |~100 MB|4317|

### Official images with custom config (pull, configure via env vars / mounted config)

|# |Container         |Image                               |What we customize                                                  |RAM    |Port |
|--|------------------|------------------------------------|-------------------------------------------------------------------|-------|-----|
|8 |calcom            |`calcom/cal.com:latest`             |OIDC (Keycloak), PostgreSQL, booking defaults                      |~400 MB|3100 |
|9 |open-webui        |`ghcr.io/open-webui/open-webui:main`|OIDC, Pipeline → sara-api endpoint                                 |~300 MB|3080 |
|10|openlit           |`ghcr.io/openlit/openlit:latest`    |ClickHouse connection, retention policy                            |~200 MB|3300 |
|11|vexa-lite         |`vexaai/vexa-lite:latest`           |Groq Whisper as transcriber, admin token                           |~350 MB|8056 |
|12|bigcapital        |`bigcapitaltech/bigcapital:latest`  |MariaDB/MongoDB connection, locale                                 |~300 MB|5000 |
|13|bigcapital-mariadb|`mariadb:10`                        |Bigcapital data store                                              |~200 MB|3306 |
|14|bigcapital-mongo  |`mongo:6`                           |Bigcapital documents                                               |~200 MB|27017|
|15|bigcapital-redis  |`redis:7-alpine`                    |Bigcapital cache (separate from shared Redis)                      |~50 MB |6380 |
|16|nginx-proxy       |`nginx:alpine`                      |Reverse proxy: injects shared nav bar + CSS theme into all services|~30 MB |80   |

### Custom build (our Dockerfile)

|# |Container|What it does                                                                                                                              |RAM    |Port|
|--|---------|------------------------------------------------------------------------------------------------------------------------------------------|-------|----|
|17|sara-api |SARA’s brain: FastAPI server + Slack bot + APScheduler + heartbeat + research processor. All endpoints, all LLM routing, all integrations.|~400 MB|8080|

**Total RAM: ~4,480 MB. Headroom on 8GB droplet: ~3,520 MB.**

### Docker volumes (persistent data)

```yaml
volumes:
  postgres-data:       # Outline, Cal.com, Keycloak, sara-api databases
  redis-data:          # Shared cache
  clickhouse-data:     # OpenLIT trace history (90-day retention)
  outline-data:        # Uploaded files and attachments
  vexa-data:           # Meeting recordings and transcripts
  bigcapital-data:     # MariaDB accounting data
  mongo-data:          # Bigcapital document store
```

sara-api is **stateless** — it stores nothing on disk. All state lives in PostgreSQL (structured data), Outline (config + documents), and Redis (ephemeral cache). If the container dies, it restarts and reloads everything from those sources.

-----

## 6. Configuration Architecture

SARA’s configuration lives in two layers with a clear boundary: infrastructure config (requires restart) vs behavior config (live-editable via Outline).

### Layer 1: Infrastructure Config (droplet filesystem)

Secrets, API keys, database URLs, service connections. Changed by SSH into the droplet, editing `.env`, and restarting the affected container.

```
/opt/sara/
├── docker-compose.yml            # All 17 containers defined here
├── .env                          # All secrets — never in git
│   ├── OPENROUTER_API_KEY
│   ├── GROQ_API_KEY
│   ├── SLACK_BOT_TOKEN
│   ├── SLACK_SIGNING_SECRET
│   ├── OUTLINE_API_KEY
│   ├── GLASSFROG_API_KEY
│   ├── KEYCLOAK_ADMIN_PASSWORD
│   ├── POSTGRES_PASSWORD
│   ├── DATABASE_URL
│   ├── VEXA_ADMIN_TOKEN
│   ├── VEXA_TRANSCRIBER_URL      # → Groq Whisper endpoint
│   └── VEXA_TRANSCRIBER_API_KEY  # → Groq API key
├── config/
│   ├── otel-collector.yaml       # OpenTelemetry routing rules
│   ├── cloudflared-config.yml    # Tunnel subdomain routing
│   └── keycloak/
│       └── realm-export.json     # Keycloak realm backup (importable)
└── data/                         # Docker volume mount points
    ├── postgres/
    ├── redis/
    ├── clickhouse/
    ├── outline/
    ├── bigcapital/
    ├── mongo/
    └── vexa/
```

**When to touch this layer:** Rotating API keys, adding new services, changing ports, database maintenance, initial setup.

### Layer 2: Behavior Config (Outline wiki pages — live-editable)

SARA’s personality, model selection, heartbeat actions, skills, and per-user preferences. The team edits these in Outline’s web UI — no SSH, no restart, no deployment. SARA reads them from Outline’s API every heartbeat cycle (30 min) and caches in Redis.

```
⚙️ SARA Config (Outline collection — team-only, hidden from advisors)
├── SOUL.md              → Identity, personality, tone, boundaries
├── HEARTBEAT.md         → Autonomous action checklist (what to check every 30 min)
├── SKILLS.md            → Capability registry (what SARA can do)
├── MODEL_PROFILES.md    → Model selection per task type + fallback chains
├── Research Queue       → Prioritized list of topics to research
├── Users/
│   ├── Mr Be.md         → Preferences, model overrides, interaction patterns
│   ├── Nermeen.md        → (each team member gets personalized SARA behavior)
│   ├── Shereen.md
│   └── Kelly.md
└── SARA Changelog       → Auto-logged: every config change, every model swap
```

**How sara-api loads config:**

```python
async def load_sara_config():
    """Called every heartbeat (30 min) — reads config from Outline, caches in Redis."""
    soul = await outline_api.get_doc_by_title("SOUL.md", collection="SARA Config")
    heartbeat = await outline_api.get_doc_by_title("HEARTBEAT.md", collection="SARA Config")
    models = await outline_api.get_doc_by_title("MODEL_PROFILES.md", collection="SARA Config")
    skills = await outline_api.get_doc_by_title("SKILLS.md", collection="SARA Config")

    await redis.set("sara:config:soul", soul, ex=1800)       # 30-min TTL
    await redis.set("sara:config:heartbeat", heartbeat, ex=1800)
    await redis.set("sara:config:models", models, ex=1800)
    await redis.set("sara:config:skills", skills, ex=1800)

async def load_user_context(user_id: str):
    """Loads per-user preferences for personalized responses."""
    cached = await redis.get(f"sara:user:{user_id}")
    if cached:
        return cached
    user_doc = await outline_api.get_doc_by_title(f"{user_id}.md", collection="SARA Config")
    await redis.set(f"sara:user:{user_id}", user_doc, ex=1800)
    return user_doc
```

**Force-reload command:** `/sara reload` in Slack clears the Redis cache and re-reads all config from Outline immediately (no waiting for next heartbeat).

### Configuration boundary — quick reference

|“I want to change…”             |Where                      |How                                |Restart?|
|--------------------------------|---------------------------|-----------------------------------|--------|
|SARA’s personality/tone         |Outline → SOUL.md          |Edit wiki page                     |No      |
|Which model handles meetings    |Outline → MODEL_PROFILES.md|Edit wiki page                     |No      |
|My personal model preferences   |Outline → Users/MyName.md  |Edit wiki page                     |No      |
|What SARA checks every 30 min   |Outline → HEARTBEAT.md     |Edit wiki page                     |No      |
|Add a new SARA skill            |Outline → SKILLS.md        |Edit wiki page                     |No      |
|Research topics                 |Outline → Research Queue   |Edit wiki page                     |No      |
|OpenRouter API key              |`.env` on droplet          |SSH + edit + restart sara-api      |Yes     |
|Add a new container/service     |`docker-compose.yml`       |SSH + edit + `docker-compose up -d`|Yes     |
|Tunnel routing for new subdomain|`cloudflared-config.yml`   |SSH + edit + restart cloudflared   |Yes     |
|Database backup/restore         |`pg_dump` / `pg_restore`   |SSH + run command                  |—       |

-----

## 7. Unified Authentication (Keycloak)

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

|Person   |Keycloak Role|Access Level                            |
|---------|-------------|----------------------------------------|
|Mr Be    |`team-admin` |Full access — all services              |
|Nermeen  |`team-admin` |Full access — all services              |
|Shereen  |`team-member`|Full access — all services              |
|Kelly    |`team-member`|Full access — all services              |
|Advisor 1|`advisor`    |Read-only — selected Outline collections|
|Advisor 2|`advisor`    |Read-only — selected Outline collections|
|Advisor 3|`advisor`    |Read-only — selected Outline collections|

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

|Collection              |Team      |Advisors                                    |
|------------------------|----------|--------------------------------------------|
|📋 OKRs                  |Read/Write|✅ Read-only                                 |
|📝 Meetings              |Read/Write|✅ Read-only                                 |
|🚀 Projects              |Read/Write|✅ Read-only                                 |
|🔬 Research              |Read/Write|✅ Read-only                                 |
|💼 Pipeline (CRM)        |Read/Write|❌ Hidden                                    |
|📣 Content & Marketing   |Read/Write|❌ Hidden                                    |
|📊 Logs                  |Read/Write|❌ Hidden                                    |
|🔮 Financial Intelligence|Read/Write|✅ Read-only (narrative briefs only)         |
|⚙️ SARA Config           |Read/Write|❌ Hidden                                    |
|📚 Knowledge Base        |Read/Write|✅ Read-only                                 |
|📦 Exports               |Read/Write|✅ Read-only (shared pitch decks, one-pagers)|

**Clients (one per service):**

|Client ID   |Service                  |Protocol      |
|------------|-------------------------|--------------|
|`outline`   |wiki.office.rafiq-ai.com |OIDC          |
|`open-webui`|chat.office.rafiq-ai.com |OIDC          |
|`bigcapital`|books.office.rafiq-ai.com|SAML / OIDC   |
|`calcom`    |cal.office.rafiq-ai.com  |OIDC          |
|`sara-api`  |Internal API auth        |JWT validation|

### Authentication Exception: GlassFrog

GlassFrog is the one service that lives outside Keycloak SSO. It’s a SaaS product at `glassfrog.com` that doesn’t support external OIDC on the free plan.

- **Team (humans):** Log into GlassFrog separately with GlassFrog credentials — the one separate login
- **SARA (AI):** Queries GlassFrog via API key — no browser login needed
- **Advisors:** Get free view-only accounts on GlassFrog if they need to see the org structure

This exception goes away in Phase 6 when governance moves into Outline.

### Adding a new team member

1. Create one account in Keycloak
1. Assign role (admin or user)
1. They can immediately access all services

No per-service account creation needed.

-----

## 8. Unified Theming — SARA Office Look & Feel

All SARA services (Outline, Open WebUI, Cal.com, Bigcapital, OpenLIT) ship with their own default branding. To make the platform feel like one product (“SARA Office”) rather than five stitched-together tools, we apply two layers of visual unification — without forking any repos.

### Layer 1: Shared CSS theme (injected per container)

A single `rafiq-theme.css` file defines the RafiqAI brand palette, typography, border-radius, and button styles. It’s mounted into each container via Docker volume and injected through each tool’s custom CSS mechanism.

**File location on droplet:**

```
/opt/sara/config/theme/
├── rafiq-theme.css          # Shared brand variables + overrides
├── outline-overrides.css    # Outline-specific selector adjustments
├── openwebui-overrides.css  # Open WebUI-specific adjustments
├── calcom-overrides.css     # Cal.com-specific adjustments
├── bigcapital-overrides.css # Bigcapital-specific adjustments
└── openlit-overrides.css    # OpenLIT-specific adjustments
```

**How each tool loads the theme:**

|Service   |Injection method                              |Restart needed?          |
|----------|----------------------------------------------|-------------------------|
|Outline   |Admin → Appearance → Custom CSS field         |No (paste or API)        |
|Open WebUI|Admin → Settings → Custom CSS                 |No                       |
|Cal.com   |Admin → Appearance → Brand colors + custom CSS|No                       |
|Bigcapital|Mounted CSS override via Docker volume        |Yes (on first setup only)|
|OpenLIT   |Mounted CSS override via Docker volume        |Yes (on first setup only)|

**Brand variables (rafiq-theme.css):**

```css
:root {
  /* RafiqAI brand palette */
  --rafiq-primary: #1B4965;       /* Deep teal — headers, primary buttons */
  --rafiq-secondary: #62B6CB;     /* Light teal — accents, links */
  --rafiq-accent: #BEE9E8;        /* Pale mint — backgrounds, cards */
  --rafiq-warning: #CAE9FF;       /* Soft blue — notifications */
  --rafiq-text: #1B1B1E;          /* Near-black — body text */
  --rafiq-text-muted: #5F6368;    /* Gray — secondary text */
  --rafiq-bg: #FAFBFC;            /* Off-white — page background */
  --rafiq-surface: #FFFFFF;       /* White — card surfaces */
  --rafiq-border: #E1E4E8;        /* Light gray — borders */
  --rafiq-radius: 8px;            /* Consistent corner rounding */

  /* Typography */
  --rafiq-font-body: 'Inter', -apple-system, sans-serif;
  --rafiq-font-heading: 'Inter', -apple-system, sans-serif;
  --rafiq-font-mono: 'JetBrains Mono', 'Fira Code', monospace;
}

/* Global overrides applied to all tools */
body {
  font-family: var(--rafiq-font-body) !important;
  background-color: var(--rafiq-bg) !important;
  color: var(--rafiq-text) !important;
}

/* Primary buttons */
button[type="submit"],
.btn-primary,
[class*="primary"] {
  background-color: var(--rafiq-primary) !important;
  border-radius: var(--rafiq-radius) !important;
}

/* Links */
a {
  color: var(--rafiq-secondary) !important;
}
```

Each per-service override file (`outline-overrides.css`, etc.) targets that tool’s specific CSS selectors — replacing their brand purple/blue/green with `var(--rafiq-primary)`, swapping their default font with Inter, and adjusting any tool-specific quirks. These files are small (50-100 lines each) and only need updating when the upstream tool changes its CSS class names, which is rare.

### Layer 2: Shared navigation bar (reverse proxy injection)

A lightweight Nginx container sits between Cloudflared and all backend services. It injects a shared HTML header into every page response — giving the team a consistent top navigation bar across all tools.

**Container added to docker-compose:**

```yaml
  nginx-proxy:
    image: nginx:alpine
    volumes:
      - ./config/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./config/theme/nav-header.html:/etc/nginx/nav-header.html:ro
      - ./config/theme/rafiq-theme.css:/etc/nginx/rafiq-theme.css:ro
    ports:
      - "80:80"
    depends_on:
      - outline
      - open-webui
      - calcom
      - bigcapital
      - openlit
```

**What the nav bar provides:**

```
┌─────────────────────────────────────────────────────────────────┐
│  🏠 SARA Office    Wiki  ·  Chat  ·  Calendar  ·  Books  ·  Obs  │  👤 Mr Be ▾  │
└─────────────────────────────────────────────────────────────────┘
```

- RafiqAI logo + “SARA Office” branding (top-left)
- Quick links to all services: Wiki (Outline), Chat (Open WebUI), Calendar (Cal.com), Books (Bigcapital), Obs (OpenLIT)
- Current user avatar pulled from Keycloak session (top-right)
- Active page highlighted in the nav
- ~30 lines of HTML, injected via Nginx `sub_filter` directive

**Nginx config (simplified):**

```nginx
server {
    listen 80;
    server_name wiki.office.rafiq-ai.com;

    location / {
        proxy_pass http://outline:3000;
        sub_filter '</head>' '<link rel="stylesheet" href="/rafiq-theme.css"></head>';
        sub_filter '<body' '<body><div id="rafiq-nav"><!-- nav-header.html content --></div';
        sub_filter_once on;
    }
}
# Repeat for each service subdomain
```

### What this achieves

|Without theming                                           |With theming                                   |
|----------------------------------------------------------|-----------------------------------------------|
|Outline’s purple, Open WebUI’s dark mode, Cal.com’s orange|Consistent teal palette across all tools       |
|Each tool has its own nav, no cross-links                 |Shared top nav bar — one click between services|
|Feels like 5 separate products                            |Feels like one “SARA Office” platform          |
|Team asks “where do I go for X?”                          |Team sees all options in the nav bar           |

### What this does NOT do

- Does not change layouts or component structure (each tool keeps its own UX patterns)
- Does not require forking any repos (all stock Docker images, fully upgradeable)
- Does not affect mobile/PWA behavior (nav bar is responsive, collapses on small screens)

### Maintenance burden

Minimal. The CSS variables and nav bar are static files on the droplet. They survive container upgrades because they’re mounted as volumes, not baked into images. The only time you’d touch them is if a tool changes its CSS class names in a major version bump, or if you want to update the brand palette.

**Effort to implement:** 2-3 days during Phase 1. CSS theming first (1 day), nav bar injection second (1-2 days).

-----

## 9. Cal.com — Scheduling & Calendar

Cal.com replaces Google Calendar as the team’s scheduling hub. Self-hosted at `cal.office.rafiq-ai.com`.

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

- **Daily briefing:** Pulls today’s schedule from Cal.com API → includes in morning Slack briefing
- **Meeting prep:** Before each booked meeting, SARA generates a prep brief (who, context from Outline, last interaction, open items)
- **Follow-up tracking:** After meetings, SARA creates follow-up tasks and checks if they were completed
- **Pipeline integration:** When an investor books via Cal.com, SARA auto-creates or updates their CRM doc in Outline
- **Heartbeat check:** “Any meetings in the next 2 hours without a prep brief? Generate one.”

-----

## 10. Open WebUI — SARA’s Chat Interface

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

|Slack                                         |Open WebUI                                              |
|----------------------------------------------|--------------------------------------------------------|
|Quick queries, meeting commands, notifications|Deep conversations, scenario modeling, document drafting|
|Team-visible interactions                     |Private 1-on-1 with SARA                                |
|Mobile-quick (already in Slack)               |Mobile-rich (full markdown, voice, files)               |

-----

## 11. Outline Collections Structure

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
│   ├── MODEL_PROFILES.md (model selection per task type — OpenRouter)
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

-----

## 12. Integrations Map

### Keycloak → All Services (SSO)

- **Protocol:** OIDC (OpenID Connect)
- **Services:** Outline, Open WebUI, Cal.com, Bigcapital
- **API Auth:** JWT token validation for sara-api

### Open WebUI → sara-api (via Pipeline)

- **Protocol:** OpenAI Chat Completions
- **Pipeline:** Custom Python plugin enriching every message with SARA’s full context

### Outline API (bi-directional)

- **Read:** Search docs, get content, list collections
- **Write:** Create meeting notes, research briefs, update OKRs, append logs
- **Events:** Webhooks on doc create/update/delete

### Cal.com API (bi-directional)

- **Read:** Today’s schedule, upcoming bookings, availability
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

### OpenRouter — LLM Gateway (outbound)

- **What:** Unified API gateway — all LLM calls go through OpenRouter regardless of provider
- **API:** OpenAI-compatible at `https://openrouter.ai/api/v1`
- **Models used:** See `MODEL_PROFILES.md` in Outline (team can swap any model by editing wiki)
- **Billing:** One credit balance, one API key, pay-per-token at provider prices (no markup)
- **Fallback:** If primary model fails, OpenRouter auto-routes to fallback model configured in profile
- **Observability:** Every call is instrumented via OpenLIT → token usage, cost, latency per task

**Model routing logic in sara-api:**

```
Task arrives at sara-api
        │
        ▼
Determine profile from task type
        │
    triage / extraction ──→ profile.triage or profile.extraction
        │                    (cheap model: Haiku, Gemini Flash, etc.)
        │
    meeting / research / finance ──→ profile.reasoning
        │                            (smart model: Sonnet, GPT-4o, etc.)
        │
    blog / pitch deck ──→ profile.creative
        │                  (creative model: Sonnet at higher temp)
        │
    meeting Q&A (live voice) ──→ profile.voice
                                  (fast model: Haiku, Gemini Flash)
        │
        ▼
litellm.completion(model=f"openrouter/{profile.model}")
        │
        ▼
OpenLIT auto-captures: tokens, cost, latency, profile name, user
```

### Scrapling — Web Scraping Engine (pip library in sara-api)

- **What:** Adaptive web scraping framework with anti-bot bypass, stealth browser fingerprinting, and Cloudflare Turnstile evasion
- **Install:** `pip install scrapling[all]` — drops into sara-api, no separate container needed
- **Used for:** Research processor fetching competitor sites, news articles, edtech funding pages, LinkedIn profiles, and any site with anti-bot protection
- **Why not basic `requests`:** Many target sites (LinkedIn, Crunchbase, news paywalls, Cloudflare-protected competitor sites) block simple HTTP clients. Scrapling’s StealthyFetcher impersonates real browser TLS fingerprints and handles JavaScript-rendered pages.
- **RAM/cost:** Negligible — it’s a Python library, not a service. Zero additional cost.

**Research pipeline with Scrapling:**

```
Heartbeat picks research task from Outline queue
        │
        ▼
Scrapling StealthyFetcher → fetches target URLs
(bypasses Cloudflare, impersonates Chrome TLS)
        │
        ▼
extraction profile model → extracts key data, summarizes
        │
        ▼
Interesting findings? → reasoning profile model → synthesize into brief
        │
        ▼
Write to Outline Research collection → post to Slack
```

### Docling — Document Parsing Engine (pip library in sara-api)

- **What:** IBM-developed AI document parser that converts PDF, DOCX, PPTX, XLSX, images, and scanned documents into clean structured data (markdown, JSON, DataFrames)
- **Install:** `pip install docling` — drops into sara-api, no separate container
- **Used for:** Parsing uploaded PDFs (investor term sheets, research papers, competitor decks, school agreements), extracting tables from financial reports, OCR on scanned documents
- **Why it matters:** Claude can’t reason over a raw PDF. Docling converts complex layouts, tables, and formulas into markdown that Claude can actually understand.

**Document ingestion pipeline:**

```
File uploaded via Open WebUI / Slack / email
        │
        ▼
Docling parses → clean markdown + extracted tables
        │
        ▼
extraction profile → classify document type, extract metadata
        │
        ▼
reasoning profile → summarize, identify key points
        │
        ▼
Store in relevant Outline collection with summary
Post notification to Slack
```

### TheAgenticBrowser — Form Filling Agent (fallback tool)

- **What:** AI agent that automates browser interactions — filling forms, navigating multi-step applications, clicking through web portals
- **Install:** `pip install` + Playwright (headless Chrome)
- **Used for:** Angel group applications (Flat6Labs, Y Combinator, etc.), accelerator forms, government grant portals, school procurement system submissions
- **When to use:** Only for interactive web tasks that Scrapling can’t handle (form filling, multi-step navigation, login-required portals)
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
- **Templates:** Stored in Outline’s `📦 Exports / Branding` collection

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
- **Has MCP server** — can plug directly into SARA’s tool ecosystem

-----

## 13. Heartbeat & Proactive Behaviors

### Heartbeat Loop

- Runs every 30 minutes during active hours (9AM-10PM Cairo time)
- Reads `HEARTBEAT.md` from Outline’s SARA Config collection
- Uses **triage profile** (cheap model via OpenRouter) for heartbeat checks — typically $0.001/call
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

|Job              |Schedule            |What it does                                                            |
|-----------------|--------------------|------------------------------------------------------------------------|
|`daily_briefing` |8:00 AM EET daily   |Cal.com schedule + overdue items + OKR snapshot + yesterday’s highlights|
|`weekly_digest`  |Sunday 8:00 PM EET  |Full week: meetings, OKR progress, stalled projects, research, finances |
|`okr_monday`     |Monday 9:00 AM EET  |Pull GlassFrog metrics, compare to targets, flag off-track              |
|`linkedin_remind`|Tue & Thu 10:00 AM  |Check content calendar, draft or remind                                 |
|`reindex_docs`   |Every 6 hours       |Scan Outline for changes, update search indices                         |
|`pipeline_check` |Friday 3:00 PM EET  |Review CRM docs, flag stale contacts, suggest follow-ups                |
|`monthly_finance`|1st of month 9:00 AM|Pull Bigcapital reports → financial brief → Outline                     |
|`timesheet_gen`  |Friday 5:00 PM EET  |Auto-generate weekly timesheets from meetings + tasks                   |

-----

## 14. SOUL.md — SARA’s Identity

Inspired by OpenClaw’s `soul.md` pattern, SARA’s identity is defined in a plain-text document stored in Outline (`⚙️ SARA Config / SOUL.md`). This document is loaded as the base system prompt for every LLM call, regardless of which model is active. The team can edit SARA’s personality by editing a wiki page. No code deployment needed.

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

-----

## 15. Per-User Memory

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

Every call to the LLM (via OpenRouter) includes both SOUL.md and the user’s file. The response is naturally tailored without any explicit personalization logic — the LLM just knows who it’s talking to.

-----

## 16. Skills Registry

SARA’s capabilities are documented in a human-readable registry stored in Outline (`⚙️ SARA Config / SKILLS.md`). This serves three purposes: (1) the team knows what SARA can do, (2) SARA herself reads it to understand her capabilities, (3) it’s the spec for sara-api development.

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
- **Model:** triage profile → reasoning profile for complex questions
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
  Qwen/Haiku summarizes → Sonnet synthesizes brief
- **Model:** extraction profile → reasoning profile
- **Output:** Research brief in Outline Research collection, summary in Slack

---

## Document Skills

### parse
- **Trigger:** Upload file via Open WebUI or `/sara parse [file]`
- **What:** Docling converts PDF/DOCX/PPTX/images → structured markdown
- **Model:** Docling AI models → extraction profile for classification
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

-----

## 17. Meeting Pipeline (end-to-end)

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

-----

## 18. CRM in Outline

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

-----

## 19. Logs Architecture

### Outline (narrative/operational logs)

|Log Type      |Auto-generated by              |Frequency         |
|--------------|-------------------------------|------------------|
|Timesheets    |SARA (from meetings + tasks)   |Weekly            |
|Validation Log|Team + SARA (from meetings)    |Per experiment    |
|Decision Log  |SARA (from meeting transcripts)|Per meeting       |
|Changelog     |Team + SARA                    |Per release/change|

### Bigcapital (financial logs)

|Log Type|Entered by                         |Notes                     |
|--------|-----------------------------------|--------------------------|
|Expenses|Team (at books.office.rafiq-ai.com)|Proper double-entry       |
|Invoices|Team                               |Sent to schools, partners |
|Revenue |Team                               |Recorded when received    |
|Journals|Team / Accountant                  |Multi-currency adjustments|

### SARA-generated financial intelligence (in Outline)

|Document             |Generated by                  |Frequency|
|---------------------|------------------------------|---------|
|Monthly Finance Brief|SARA (Bigcapital API → Claude)|Monthly  |
|Runway Model         |SARA (actuals + assumptions)  |Monthly  |
|Scenario Analyses    |SARA (on request via chat)    |Ad hoc   |

-----

## 20. Hosting — Single Droplet + Surface Ollama Node

### DigitalOcean — One droplet runs everything

|Droplet      |Spec                     |Cost  |
|-------------|-------------------------|------|
|**SARA Main**|8GB RAM / 4 vCPU (shared)|$48/mo|

17 containers, ~4,480 MB RAM used, ~3,520 MB headroom. See Section 5 for full container inventory.

### Surface Pro — Ollama inference node

The Surface Pro (i5-7300U / 8GB) runs headless Ubuntu Server with only Ollama + SSH. It connects to the DO droplet via a persistent reverse SSH tunnel, making Ollama’s API appear local to sara-api.

```bash
# On Surface — persistent reverse tunnel (runs via systemd service)
autossh -M 0 -N -R 11434:localhost:11434 sara@do-main-droplet
```

sara-api calls `localhost:11434` for Tier 1 (free) model inference. If the Surface is offline, sara-api falls back to OpenRouter cheap models automatically.

### Cost Summary

|Item                                    |Monthly    |Annual       |
|----------------------------------------|-----------|-------------|
|DO droplet — 8GB shared CPU             |$48        |$576         |
|OpenRouter credits (all cloud LLM calls)|$10-18     |$120-216     |
|Groq Whisper (meeting transcription)    |~$0.50     |~$6          |
|Surface electricity (~15W headless)     |~$2        |~$24         |
|Domain (rafiq-ai.com)                   |$1         |$12          |
|**Total**                               |**~$62-70**|**~$738-834**|

*Surface Ollama handles all triage calls (26+/day) for free, saving ~$2-5/mo on OpenRouter. Groq Whisper at $0.04/hr for ~10-15 meetings/month is negligible. OpenLIT dashboard shows exact spend per task type so the team can optimize model selection based on real data.*

-----

## 21. Build Phases

### Phase 1 — Foundation (Week 1-2)

- [ ] Provision DigitalOcean droplet: 8GB / 4 vCPU shared, Ubuntu Server 24.04 LTS
- [ ] Install Docker + Docker Compose
- [ ] Create `/opt/sara/` directory structure with `.env`, `config/`, `data/`
- [ ] Deploy Keycloak (auth.office.rafiq-ai.com)
- [ ] Create `rafiq-office` realm with roles: `team-admin`, `team-member`, `advisor`
- [ ] Add all 7 users (Mr Be, Nermeen, Shereen, Kelly + 3 advisors)
- [ ] Deploy PostgreSQL + Redis (shared services)
- [ ] Deploy Outline, configure OIDC with Keycloak
- [ ] Configure Outline permission groups (Team: full access, Advisors: read-only on selected collections)
- [ ] Deploy Open WebUI, configure OIDC with Keycloak (team-only access)
- [ ] Set up Cloudflare Tunnel routing all `*.office.rafiq-ai.com` subdomains
- [ ] Deploy OpenLIT + ClickHouse + OTel Collector (obs.office.rafiq-ai.com)
- [ ] Deploy Vexa Lite, configure Groq Whisper as transcription provider
- [ ] Deploy Bigcapital + MariaDB + MongoDB + Redis (books.office.rafiq-ai.com)
- [ ] Create OpenRouter account, load initial credits ($25), configure API key
- [ ] Create Groq account, get Whisper API key
- [ ] Scaffold sara-api (FastAPI + OpenRouter via litellm)
- [ ] Install core Python libraries: `litellm`, `openlit`, `instructor`, `slack-sdk`
- [ ] Configure litellm three-tier model routing (Surface Ollama + OpenRouter cheap + OpenRouter premium)
- [ ] Add `openlit.init()` to sara-api entrypoint
- [ ] Build basic /ask endpoint (query Outline → Claude → response)
- [ ] Build Open WebUI Pipeline to route through sara-api
- [ ] Deploy sara-slack-bot with /sara ask command
- [ ] Create initial Outline collections structure
- [ ] Write SOUL.md (SARA’s identity document) and store in ⚙️ SARA Config
- [ ] Create per-user files: Mr Be.md, Nermeen.md, Shereen.md, Kelly.md in ⚙️ SARA Config / Users
- [ ] Write initial SKILLS.md (skill registry) with core skills
- [ ] Configure sara-api to load SOUL.md + user context on every LLM call
- [ ] **Surface setup:** Install Ubuntu Server headless on Surface Pro, install Ollama + Qwen 3.5 2B, configure autossh reverse tunnel to DO droplet, verify sara-api can reach Ollama at localhost:11434
- [ ] **Unified theming:** Create `rafiq-theme.css` with brand palette, deploy nginx-proxy container with shared nav bar injection, apply per-service CSS overrides to Outline, Open WebUI, Cal.com, Bigcapital, OpenLIT
- **Milestone:** Full team can log in via Keycloak; advisors see shared docs; team chats with SARA; all services share consistent “SARA Office” branding and navigation

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
- [ ] Build research processor (Scrapling fetch → newspaper4k extract → extraction profile summarize → reasoning profile synthesize → Outline → Slack)
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

- [ ] Integrate Edge TTS for SARA’s voice
- [ ] Wire up Vexa /speak endpoint for live meeting responses
- [ ] Implement “Hey Sara” trigger detection on transcript stream
- [ ] Set up Outline CRM templates (investors, schools, partners)
- [ ] Build pipeline_check cron job
- [ ] Deploy Bigcapital on DO droplet (books.office.rafiq-ai.com)
- [ ] Configure Bigcapital SSO with Keycloak
- [ ] Wire up Bigcapital API to sara-api /finance endpoint
- [ ] Build monthly_finance cron job (Bigcapital → Outline brief)
- [ ] Build scenario modeling (“Sara, model our runway if…”) using `pandas` for calculations + `plotly` for charts
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

-----

## 22. Key Design Decisions

1. **Nested subdomains under `office.rafiq-ai.com`.** Complete namespace isolation from customer-facing domains. Clean, descriptive, and each service lives at root `/` avoiding path-routing fragility.
1. **Keycloak as unified SSO.** One login for everything. Add a team member in 30 seconds. Remove them and all access revokes instantly. Industry-standard OIDC, Red Hat-backed.
1. **Cal.com replaces Google Calendar.** Self-hosted, API-driven, with booking pages for external meetings. Syncs with Google Calendar for personal use. SARA reads from Cal.com API for all scheduling intelligence.
1. **Open WebUI as chat frontend.** Battle-tested UI with PWA, voice, files, conversation history. Connected to sara-api via Pipelines for full SARA intelligence.
1. **Bigcapital on DO, not Surface.** Frees ~2GB RAM on Surface for Keycloak + Cal.com. Accounting doesn’t need sub-millisecond latency — $6/mo droplet is perfect.
1. **Outline as CRM.** For <50 contacts, structured docs with Claude reasoning beats a traditional CRM. Migrate to Twenty CRM when pipeline grows.
1. **OpenRouter as unified LLM gateway with model profiles.** All LLM calls route through OpenRouter — one API key, one credit balance, 500+ models. Model profiles (stored in Outline as `MODEL_PROFILES.md`) let the team swap models per task type by editing a wiki page: Haiku for triage, Sonnet for reasoning, Gemini Flash as fallback. OpenLIT observability tracks every call so the team can optimize spend based on real data, not guesswork. No vendor lock-in — if a cheaper or better model appears, swap it in minutes.
1. **GlassFrog stays source of truth for governance — until Phase 6.** Don’t replicate holacracy in Outline initially. SARA bridges the two systems via API. In Phase 6, migrate governance entirely into Outline with SARA as the Holacracy secretary, eliminating the only service outside Keycloak SSO.
1. **Human-in-the-loop for content and finance.** SARA drafts but never auto-publishes or creates real transactions.
1. **Docker Compose everything.** Portable. If the Surface dies, `docker-compose up` on a new machine.
1. **Configuration-as-personality (inspired by OpenClaw).** SARA’s behavior is defined in plain-text documents in Outline, not hardcoded in Python. SOUL.md defines her identity, HEARTBEAT.md defines her autonomous actions, SKILLS.md defines her capabilities, and per-user files define how she adapts to each person. The team shapes SARA by editing wiki pages, not deploying code.

-----

## 23. Security

- **Keycloak SSO:** Every `*.office.rafiq-ai.com` service requires authentication. No anonymous access.
- **Cloudflare Tunnel:** No ports open on home router. Outbound-only encrypted connections. Real IP hidden.
- **API Keys:** Stored as Docker env vars in `.env` file, never in code. GlassFrog, Anthropic, Vexa, Slack tokens all managed centrally.
- **Nested subdomains:** Not linked from any public page. Not discoverable via search engines. Not in any sitemap.
- **Backups:** Daily pg_dump (Outline + Cal.com DBs) + Bigcapital export → synced offsite (Supabase Storage or S3).
- **UPS:** Prevents data corruption from power loss. Surface auto-boots on AC restore.

-----

## 24. Future Considerations

- **Team growth beyond 7:** Upgrade SARA Main droplet to 8GB ($96/mo) or move to Hetzner dedicated server for better value. The 4+3 structure fits comfortably on 4GB since advisors are low-frequency users.
- **Pipeline >50 contacts:** Migrate CRM from Outline to Twenty CRM (open-source, self-hosted, has API).
- **Self-hosted Vexa:** Move from hosted API to self-hosted at `meet.office.rafiq-ai.com` for full data control.
- **Voice conversations anytime:** Open WebUI already supports voice I/O — wire to SARA for voice chat outside of meetings.
- **MCP + Claude Code:** Wire Outline MCP server into Claude Code so SARA’s knowledge base is available while coding on EdVenture/RafiqAI.
- **Multi-agent:** Split SARA into specialized agents (Finance, Research, Meeting, Content) coordinated by an orchestrator.
- **Cloudflare Access:** Add as extra security layer if external stakeholders need limited access beyond Outline.
- **Advisor portal:** If advisors need more than read-only Outline, consider a lightweight custom dashboard pulling from Outline API.

-----

*This document was designed through dialogue between Mr Be and Claude on March 5, 2026. It is the definitive architecture blueprint for SARA and should become the first document stored in SARA’s Outline instance when she goes live.*

*Next step: Open Claude Code and build the Docker Compose stack for the Surface.*

*Total brainstorm duration: one conversation. Total sections: 24. Total containers: 17 (1 custom, 16 official). Total build phases: 6 (16 weeks). Total configuration files: 4 (SOUL.md, HEARTBEAT.md, SKILLS.md, MODEL_PROFILES.md) + 4 user files. Total monthly cost: ~$62-70. Total team served: 4 members + 3 advisors. Total humans replaced: zero. Total humans amplified: four.*