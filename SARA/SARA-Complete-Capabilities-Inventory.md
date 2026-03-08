# SARA Complete Capabilities Inventory

**Everything discussed across all sessions — core + plugins + explored options**
**For:** RafiqAI founding team (Mr Be, Nermeen, Shereen, Kelly + 3 advisors)

-----

## CORE (always present — every SARA installation needs these)

These seven capabilities are non-negotiable. Without any one of them, the system doesn’t function as an AI-powered office.

|# |Capability            |What it answers                         |Service                  |Replaces                                      |Docker Image                                       |RAM    |Subdomain               |Status              |
|--|----------------------|----------------------------------------|-------------------------|----------------------------------------------|---------------------------------------------------|-------|------------------------|--------------------|
|C1|**Identity & Access** |Who are we? Who can see what?           |Keycloak                 |Okta, Auth0, Google Workspace login           |`quay.io/keycloak/keycloak`                        |~600 MB|auth.office.rafiq-ai.com|✅ In v5             |
|C2|**Knowledge Base**    |What do we know? Where is it written?   |Outline                  |Notion, Confluence, Google Docs               |`docker.getoutline.com/outlinewiki/outline`        |~400 MB|wiki.office.rafiq-ai.com|✅ In v5             |
|C3|**AI Intelligence**   |What should we do? What does this mean? |sara-api + Open WebUI    |ChatGPT Teams, Notion AI, human chief of staff|Custom Dockerfile + `ghcr.io/open-webui/open-webui`|~700 MB|chat.office.rafiq-ai.com|✅ In v5             |
|C4|**Scheduling & Time** |When do we do it? Who’s available?      |Cal.com                  |Calendly, Google Calendar, Outlook            |`calcom/cal.com`                                   |~400 MB|cal.office.rafiq-ai.com |✅ In v5             |
|C5|**Automation**        |How do we repeat it? What triggers what?|n8n                      |Zapier, Make, IFTTT                           |`docker.n8n.io/n8nio/n8n`                          |~300 MB|auto.office.rafiq-ai.com|🆕 Added this session|
|C6|**Database & Cache**  |Where does structured data live?        |PostgreSQL 15 + Redis 7  |Supabase, Firebase, managed DB                |`postgres:15-alpine` + `redis:7-alpine`            |~600 MB|internal only           |✅ In v5             |
|C7|**Network & Branding**|How do people get in? How does it look? |Cloudflared + nginx-proxy|Ngrok, Tailscale, corporate VPN               |`cloudflare/cloudflared` + `nginx:alpine`          |~80 MB |*.office.rafiq-ai.com   |✅ In v5             |

**Core total: ~3,080 MB RAM. Fits on a 4GB droplet ($24/mo).**

-----

## PLUGINS (extend the core — add what you need)

Each plugin connects to the core through Keycloak (SSO), PostgreSQL (shared data where applicable), n8n (automation triggers), sara-api (AI reasoning), and nginx-proxy (unified branding).

### Communication

|# |Capability                      |What it adds                                     |Service             |Replaces                        |Docker Image                                             |RAM    |Subdomain                  |Status    |
|--|--------------------------------|-------------------------------------------------|--------------------|--------------------------------|---------------------------------------------------------|-------|---------------------------|----------|
|P1|**Email**                       |Send/receive email, spam filtering, encryption   |Stalwart Mail Server|Gmail, Outlook, Google Workspace|`stalwartlabs/mail-server`                               |~400 MB|mail.office.rafiq-ai.com   |🆕 Explored|
|P2|**Webmail UI**                  |Browser-based email client                       |Roundcube           |Gmail web UI, Outlook web       |`roundcube/roundcubemail`                                |~100 MB|webmail.office.rafiq-ai.com|🆕 Explored|
|P3|**Newsletter & Email Marketing**|Subscriber lists, campaigns, analytics           |Listmonk            |Mailchimp, ConvertKit, Brevo    |`listmonk/listmonk`                                      |~100 MB|news.office.rafiq-ai.com   |🆕 Explored|
|P4|**Team Chat**                   |Channels, threads, DMs, file sharing, mobile apps|Rocket.Chat         |Slack, Teams, Discord           |`rocketchat/rocket.chat` (shares MongoDB with Bigcapital)|~600 MB|team.office.rafiq-ai.com   |🆕 Explored|

### Meetings & Collaboration

|# |Capability              |What it adds                              |Service                 |Replaces                     |Docker Image                                                |RAM    |Subdomain               |Status                                   |
|--|------------------------|------------------------------------------|------------------------|-----------------------------|------------------------------------------------------------|-------|------------------------|-----------------------------------------|
|P5|**Video Meetings**      |Encrypted video calls, screen sharing     |Jitsi Meet              |Google Meet, Zoom, Teams     |`jitsi/web` + `jitsi/prosody` + `jitsi/jvb` + `jitsi/jicofo`|~1.5 GB|meet.office.rafiq-ai.com|🆕 Explored (separate droplet recommended)|
|P6|**Meeting Intelligence**|Join meetings, transcribe, extract actions|Vexa Lite + Groq Whisper|Otter.ai, Fireflies, Fathom  |`vexaai/vexa-lite`                                          |~350 MB|internal API            |✅ In v5                                  |
|P7|**Whiteboard**          |Collaborative visual canvas, diagrams     |Excalidraw (self-hosted)|Miro, FigJam, Google Jamboard|`excalidraw/excalidraw`                                     |~100 MB|draw.office.rafiq-ai.com|🆕 Explored                               |

### Design & Creative

|# |Capability              |What it adds                                                                   |Service|Replaces                      |Docker Image                                                     |RAM    |Subdomain                 |Status    |
|--|------------------------|-------------------------------------------------------------------------------|-------|------------------------------|-----------------------------------------------------------------|-------|--------------------------|----------|
|P8|**Design & Prototyping**|Vector editing, UI design, prototyping, design systems, real-time collaboration|Penpot |Canva, Figma, Sketch, Adobe XD|`penpotapp/frontend` + `penpotapp/backend` + `penpotapp/exporter`|~800 MB|design.office.rafiq-ai.com|🆕 Explored|

### Storage & Files

|#  |Capability                |What it adds                               |Service  |Replaces                       |Docker Image      |RAM    |Subdomain                |Status    |
|---|--------------------------|-------------------------------------------|---------|-------------------------------|------------------|-------|-------------------------|----------|
|P9 |**Object Storage (S3)**   |S3-compatible file storage for all services|Garage   |AWS S3, MinIO, Backblaze B2    |`dxflrs/garage`   |~50 MB |internal API             |🆕 Explored|
|P10|**File Sync & Sharing UI**|Dropbox-like UI, mobile sync, sharing      |Nextcloud|Google Drive, Dropbox, OneDrive|`nextcloud:apache`|~500 MB|files.office.rafiq-ai.com|🆕 Explored|

### Finance & Operations

|#  |Capability    |What it adds                                |Service                  |Replaces                      |Docker Image                                           |RAM    |Subdomain                   |Status |
|---|--------------|--------------------------------------------|-------------------------|------------------------------|-------------------------------------------------------|-------|----------------------------|-------|
|P11|**Accounting**|Invoices, expenses, reports, runway tracking|Bigcapital               |QuickBooks, Xero, Wave        |`bigcapitaltech/bigcapital` + MariaDB + MongoDB + Redis|~750 MB|books.office.rafiq-ai.com   |✅ In v5|
|P12|**CRM**       |Investor/school/partner pipeline tracking   |Outline (structured docs)|HubSpot, Salesforce, Pipedrive|— (built into Outline)                                 |—      |wiki.office.rafiq-ai.com/crm|✅ In v5|

### Governance & Process

|#  |Capability              |What it adds                       |Service                             |Replaces        |Docker Image       |RAM|Subdomain|Status             |
|---|------------------------|-----------------------------------|------------------------------------|----------------|-------------------|---|---------|-------------------|
|P13|**Holacracy Governance**|Roles, circles, tensions, proposals|GlassFrog API → later Outline-native|GlassFrog (SaaS)|— (API integration)|—  |—        |✅ In v5 (Phase 4+6)|

### Observability & DevOps

|#  |Capability           |What it adds                                          |Service                              |Replaces                           |Docker Image                                                                               |RAM    |Subdomain              |Status |
|---|---------------------|------------------------------------------------------|-------------------------------------|-----------------------------------|-------------------------------------------------------------------------------------------|-------|-----------------------|-------|
|P14|**LLM Observability**|Token usage, cost per task, latency, model performance|OpenLIT + ClickHouse + OTel Collector|Langfuse, Helicone, manual tracking|`ghcr.io/openlit/openlit` + `clickhouse/clickhouse-server` + `otel/opentelemetry-collector`|~600 MB|obs.office.rafiq-ai.com|✅ In v5|

-----

## SARA-API BUILT-IN CAPABILITIES (no extra containers — features of the sara-api Python service)

These are capabilities sara-api provides through its own code, using the core + plugin services as data sources.

|#  |Capability                 |What it does                                                                                                                                             |Key Libraries                                   |Phase   |
|---|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|--------|
|S1 |**Conversational AI**      |Answer questions with full Outline context, bilingual Arabic/English                                                                                     |litellm, instructor, openlit                    |Phase 1 |
|S2 |**Slack Bot**              |/sara ask, /sara join, /sara research, /sara cal, /sara role, /sara export, /sara fill-form, /sara tension, /sara propose, /sara governance, /sara reload|slack-sdk, slack-bolt                           |Phase 1+|
|S3 |**Heartbeat**              |Autonomous triage every 30 min (9AM–10PM EET), checks HEARTBEAT.md from Outline                                                                          |apscheduler                                     |Phase 3 |
|S4 |**Daily Briefing**         |Morning summary: today’s calendar, overnight updates, pending tasks                                                                                      |apscheduler                                     |Phase 3 |
|S5 |**Weekly Digest**          |Sunday evening: week’s meetings, decisions, research, pipeline status                                                                                    |apscheduler                                     |Phase 3 |
|S6 |**Research Processor**     |Pick topic → stealth web scrape → extract → synthesize → write to Outline → post to Slack                                                                |scrapling, newspaper4k, markdownify, langdetect |Phase 3 |
|S7 |**Document Parsing**       |Upload PDF/DOCX/PPTX → structured data → Outline                                                                                                         |docling                                         |Phase 3 |
|S8 |**Change Notifications**   |Detect meaningful Outline doc changes → Slack notification with diff                                                                                     |difflib (stdlib)                                |Phase 3 |
|S9 |**Timesheet Generation**   |Auto-generate weekly timesheets from meetings + tasks                                                                                                    |apscheduler                                     |Phase 3 |
|S10|**Meeting Pipeline**       |Transcript → structured notes → decisions → action items → Outline + Slack                                                                               |litellm, instructor                             |Phase 2 |
|S11|**Meeting Prep Briefs**    |30 min before meeting: attendee context, previous meetings, relevant docs                                                                                |httpx (Cal.com API)                             |Phase 2 |
|S12|**Voice in Meetings**      |SARA speaks when addressed (“Hey Sara”)                                                                                                                  |edge-tts, Vexa /speak endpoint                  |Phase 5 |
|S13|**Financial Modeling**     |“Model our runway if…” → calculations → charts → Outline                                                                                                 |pandas, plotly                                  |Phase 5 |
|S14|**PDF Export**             |Outline doc → branded PDF with bilingual Arabic/English support                                                                                          |weasyprint, jinja2, arabic-reshaper, python-bidi|Phase 5 |
|S15|**Slide Export**           |Outline doc → branded PPTX pitch deck                                                                                                                    |python-pptx, jinja2                             |Phase 5 |
|S16|**Form Filling**           |Automated web form completion for grants/accelerators with screenshot review                                                                             |TheAgenticBrowser                               |Phase 5 |
|S17|**Content Drafts**         |LinkedIn posts, email drafts from content calendar in Outline                                                                                            |litellm                                         |Phase 5 |
|S18|**Governance Facilitation**|Tension logging, proposal drafting, async consent via Slack reactions, real-time governance meeting tracking                                             |litellm, slack-bolt                             |Phase 6 |
|S19|**Config Hot-Reload**      |Reads SOUL.md, HEARTBEAT.md, SKILLS.md, MODEL_PROFILES.md, Users/*.md from Outline every 30 min. `/sara reload` for instant refresh.                     |httpx (Outline API), redis                      |Phase 1 |

-----

## LLM INFRASTRUCTURE (not containers — external services + local inference)

|# |Component             |What it does                                                      |Cost                     |Status |
|--|----------------------|------------------------------------------------------------------|-------------------------|-------|
|L1|**Surface Pro Ollama**|Free local inference for triage, classification, tagging (Tier 1) |Free (~$2/mo electricity)|✅ In v5|
|L2|**OpenRouter**        |Unified LLM gateway: Haiku/Flash (Tier 2) + Sonnet/GPT-4o (Tier 3)|$10–18/mo                |✅ In v5|
|L3|**Groq Whisper**      |Meeting transcription at $0.04/hour                               |~$0.50/mo                |✅ In v5|
|L4|**MODEL_PROFILES.md** |Per-task model selection, editable as Outline wiki page           |Free                     |✅ In v5|
|L5|**Per-user overrides**|Each team member can prefer different models in Users/{name}.md   |Free                     |✅ In v5|

-----

## CONFIGURATION SYSTEM

|#   |Component                     |What it configures                                           |Where it lives    |Editable by   |
|----|------------------------------|-------------------------------------------------------------|------------------|--------------|
|CF1 |**.env**                      |API keys, secrets, database URLs                             |Droplet filesystem|SSH (Mr Be)   |
|CF2 |**docker-compose.yml**        |Container definitions, volumes, ports                        |Droplet filesystem|SSH (Mr Be)   |
|CF3 |**SOUL.md**                   |SARA’s identity, personality, tone, boundaries               |Outline wiki      |Team (browser)|
|CF4 |**HEARTBEAT.md**              |What SARA checks every 30 minutes                            |Outline wiki      |Team (browser)|
|CF5 |**SKILLS.md**                 |Capability registry — what SARA can do                       |Outline wiki      |Team (browser)|
|CF6 |**MODEL_PROFILES.md**         |Which model for which task + fallback chains                 |Outline wiki      |Team (browser)|
|CF7 |**Users/{name}.md**           |Per-person preferences, model overrides, interaction patterns|Outline wiki      |Each person   |
|CF8 |**Research Queue**            |Prioritized list of topics for SARA to research              |Outline wiki      |Team (browser)|
|CF9 |**SARA Changelog**            |Auto-logged config changes, model swaps, deployments         |Outline wiki      |SARA (auto)   |
|CF10|**rafiq-theme.css**           |Brand palette, typography, border-radius for all services    |Droplet filesystem|SSH (Mr Be)   |
|CF11|**nav-header.html**           |Shared navigation bar injected across all services           |Droplet filesystem|SSH (Mr Be)   |
|CF12|**otel-collector.yaml**       |OpenTelemetry trace routing to ClickHouse                    |Droplet filesystem|SSH (Mr Be)   |
|CF13|**cloudflared-config.yml**    |Tunnel subdomain → container port routing                    |Droplet filesystem|SSH (Mr Be)   |
|CF14|**keycloak/realm-export.json**|SSO realm backup (importable)                                |Droplet filesystem|SSH (Mr Be)   |

-----

## FULL DOMAIN MAP

|Subdomain                  |Service               |Access                         |
|---------------------------|----------------------|-------------------------------|
|auth.office.rafiq-ai.com   |Keycloak              |All users                      |
|wiki.office.rafiq-ai.com   |Outline               |All users (advisors: read-only)|
|chat.office.rafiq-ai.com   |Open WebUI            |Team only                      |
|cal.office.rafiq-ai.com    |Cal.com               |All users + public booking     |
|auto.office.rafiq-ai.com   |n8n                   |Team only                      |
|team.office.rafiq-ai.com   |Rocket.Chat           |Team only                      |
|books.office.rafiq-ai.com  |Bigcapital            |Team only                      |
|obs.office.rafiq-ai.com    |OpenLIT               |Team only                      |
|mail.office.rafiq-ai.com   |Stalwart              |Team only                      |
|webmail.office.rafiq-ai.com|Roundcube             |Team only                      |
|news.office.rafiq-ai.com   |Listmonk              |Team only                      |
|meet.office.rafiq-ai.com   |Jitsi Meet            |All users                      |
|draw.office.rafiq-ai.com   |Excalidraw            |Team only                      |
|design.office.rafiq-ai.com |Penpot                |Team only                      |
|files.office.rafiq-ai.com  |Nextcloud or Garage UI|Team only                      |

-----

## COST SUMMARY (current RafiqAI deployment — core + selected plugins)

### What we’re building now (v5 architecture)

|Item                                          |Monthly   |
|----------------------------------------------|----------|
|DigitalOcean 8GB droplet (core + most plugins)|$48       |
|OpenRouter LLM credits                        |$10–18    |
|Groq Whisper transcription                    |~$0.50    |
|Surface Pro electricity                       |~$2       |
|Domain                                        |$1        |
|**Subtotal**                                  |**$62–70**|

### If we add all explored plugins

|Additional Item              |Monthly                             |RAM Added                             |
|-----------------------------|------------------------------------|--------------------------------------|
|Rocket.Chat (team chat)      |$0 (same droplet, shares MongoDB)   |+600 MB                               |
|Stalwart + Roundcube (email) |$0 (same droplet)                   |+500 MB                               |
|Listmonk (newsletter)        |$0 (same droplet, shares PostgreSQL)|+100 MB                               |
|Penpot (design & prototyping)|$0 (same droplet, shares PostgreSQL)|+800 MB                               |
|Excalidraw (whiteboard)      |$0 (same droplet)                   |+100 MB                               |
|Garage (S3 storage)          |$0 (same droplet)                   |+50 MB                                |
|n8n (automation)             |$0 (same droplet)                   |+300 MB                               |
|Jitsi Meet (video meetings)  |+$24 (separate droplet)             |~1.5 GB                               |
|Nextcloud (file sync UI)     |$0 (same droplet)                   |+500 MB                               |
|**Additional total**         |**+$24**                            |**+4,450 MB**                         |
|**Grand total (everything)** |**~$86–94/mo**                      |**~7,530 MB on main + Jitsi separate**|

*With ALL plugins on the main droplet (except Jitsi), RAM usage would be ~7.5 GB — tight on an 8GB droplet. Recommended: upgrade to 16GB ($96/mo) if deploying all plugins, or keep the 8GB and only activate the plugins you actually use. Jitsi always needs its own droplet.*

-----

## BUILD TIMELINE

|Phase                          |Weeks|What gets built                                                                          |
|-------------------------------|-----|-----------------------------------------------------------------------------------------|
|1. Foundation                  |1–2  |Core (C1–C7) + Bigcapital (P10) + Vexa Lite (P6) + OpenLIT (P13) + unified theming       |
|2. Meetings + Calendar         |3–4  |Meeting pipeline (S10–S11) + Cal.com booking pages + decision log                        |
|3. Heartbeat & Research        |5–6  |Heartbeat (S3) + briefings (S4–S5) + research (S6) + doc parsing (S7) + timesheets (S9)  |
|4. GlassFrog + Logs            |7–8  |Governance integration (P12) + validation/change logs                                    |
|5. Voice, CRM, Finance, Exports|9–12 |Voice (S12) + CRM (P11) + finance (S13) + exports (S14–S15) + form filling (S16)         |
|6. Governance Migration        |13–16|GlassFrog → Outline (S18) + SARA as Holacracy secretary                                  |
|7. Extended plugins (future)   |TBD  |Email (P1–P2) + Newsletter (P3) + Video (P5) + Whiteboard (P7) + Files (P8–P9) + n8n (C5)|

**170 tasks for Phases 1–6. ~250 estimated hours. All executable with Claude Code.**

-----

## COUNTS

|Category                       |Count                   |
|-------------------------------|------------------------|
|Core capabilities              |7                       |
|Plugin capabilities            |14                      |
|sara-api built-in capabilities |19                      |
|LLM infrastructure components  |5                       |
|Configuration files            |14                      |
|Subdomains                     |15                      |
|Docker containers (current v5) |17                      |
|Docker containers (all plugins)|~32                     |
|Slack/Rocket.Chat commands     |12                      |
|Cron jobs                      |8                       |
|Build phases                   |7 (6 current + 1 future)|
|Total tasks                    |170+                    |
|Total estimated hours          |~250+                   |
|**Total capabilities**         |**45**                  |