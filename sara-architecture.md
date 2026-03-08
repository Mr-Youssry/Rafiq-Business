# SARA — Synthetic Assistance for Rafiq's Administration

## Architecture Deep Dive

---

## 1. THE CONCEPT

**SARA** = **S**ynthetic **A**ssistance for **R**afiq's **A**dministration

Sara is not a tool — she is a virtual team member at RafiqAI. She has:

- **A workspace** → Notion (she OWNS the workspace — real user account: sara@rafiq-ai.com)
- **A brain** → Claude Agent SDK (powered by Ahmed's Max subscription via OAuth token)
- **A memory** → PocketBase (persistent context, conversation history, learned patterns)
- **Ears** → meetily (meeting transcription)
- **A face** → Open WebUI (chat interface where the team talks to her)
- **A voice** → Daily digest emails/messages
- **A presence** → She appears in Notion as a real team member — assignable, @mentionable, visible

```
┌──────────────────────────────────────────────────────────────────┐
│                        SARA's WORLD                              │
│                                                                  │
│   Team Members                                                   │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐                            │
│   │ Ahmed   │ │ Shereen │ │ Nermeen │ ...                        │
│   └────┬────┘ └────┬────┘ └────┬────┘                            │
│        │           │           │                                 │
│        │     Talk to Sara      │                                 │
│        └───────────┼───────────┘                                 │
│                    ▼                                             │
│   ┌────────────────────────────┐     ┌─────────────────────┐     │
│   │   Open WebUI               │     │  Notion             │     │
│   │   (Sara's face)            │     │  (Sara's workspace) │     │
│   │   chat.rafiq.ai            │     │  notion.rafiq.ai    │     │
│   │                            │     │                     │     │
│   │   "Sara, what are our      │     │  📥 Inbox           │    │
│   │    open tasks for Qatar?"  │     │  ⚡ Action System   │    │
│   │                            │     │  📜 Logbook         │    │
│   │   Custom persona:          │     │  👥 Network (CRM)   │    │
│   │   Name: Sara               │     │  📖 Wiki            │    │
│   │   Avatar: custom           │     │  🔄 Holacracy       │    │
│   │   System prompt: [below]   │     │                     │     │
│   └────────────┬───────────────┘     └──────────┬──────────┘     │
│                │                                │                │
│                ▼                                ▼                │
│   ┌──────────────────────────────────────────────────────────┐   │
│   │              Claude Agent SDK (Sara's brain)             │   │
│   │              Running on DigitalOcean Droplet             │   │
│   │                                                          │   │
│   │  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐    │   │
│   │  │ Notion MCP  │  │ Custom MCP   │  │ meetily       │    │   │
│   │  │ Server      │  │ Server       │  │ backend       │    │   │
│   │  │             │  │ (PocketBase  │  │ (transcript   │    │   │
│   │  │ 22 tools:   │  │  bridge)     │  │  processor)   │    │   │
│   │  │ • search    │  │              │  │               │    │   │
│   │  │ • create    │  │ • memory     │  │ Port 5167     │    │   │
│   │  │ • query DB  │  │ • context    │  │               │    │   │
│   │  │ • update    │  │ • history    │  │               │    │   │
│   │  │ • comment   │  │ • prefs      │  │               │    │   │
│   │  └─────────────┘  └──────────────┘  └───────────────┘    │   │
│   │                                                          │   │
│   └──────────────────────────────────────────────────────────┘   │
│                │                                                 │
│                ▼                                                 │
│   ┌────────────────────────┐                                     │
│   │   PocketBase           │                                     │
│   │   (Sara's memory)      │                                     │
│   │   Port 8090            │                                     │
│   │                        │                                     │
│   │   Collections:         │                                     │
│   │   • conversations      │                                     │
│   │   • meeting_transcripts│                                     │
│   │   • team_preferences   │                                     │
│   │   • decision_context   │                                     │
│   │   • follow_ups         │                                     │
│   │   • daily_digest_queue │                                     │
│   │   • sara_learnings     │                                     │
│   └────────────────────────┘                                     │
└──────────────────────────────────────────────────────────────────┘
```

---

## 2. AUTHENTICATION & BILLING

### Sara uses Ahmed's Max subscription — $0 extra cost

Sara runs on Ahmed's Claude Max plan via OAuth token. No separate API billing.

### Setup:

```bash
# One-time setup on your droplet:
claude setup-token
export CLAUDE_CODE_OAUTH_TOKEN=<your_max_token>
```

### Token refresh (tokens expire periodically):

```bash
# Weekly cron job to keep Sara's brain alive
0 0 * * 0 claude setup-token --refresh
```

### Smart scheduling to respect quota:

Sara's heavy work runs at night via cron (when Ahmed isn't using Claude Code
interactively), keeping quota usage balanced:

| Sara Activity                 | When                 | Quota Impact                  |
| ----------------------------- | -------------------- | ----------------------------- |
| Chat replies (team questions) | Daytime (on demand)  | Light (~3K tokens per query)  |
| Meeting transcript processing | Night cron (2 AM)    | Medium (~25K tokens)          |
| Daily digest generation       | Night cron (3 AM)    | Light (~7K tokens × 3 people) |
| Follow-up monitoring          | Night cron (4 AM)    | Light (~5K tokens)            |
| Weekly summary                | Sunday night (1 AM)  | Medium (~15K tokens)          |
| Wiki updates from meetings    | Night cron (2:30 AM) | Medium (~10K tokens)          |

### Monthly cost for Sara's brain: $0

Everything deducted from Max subscription quota. No separate API charges.

---

## 3. SARA IN NOTION — Workspace Owner

### The Key Insight: Sara owns the workspace

Sara is NOT just an integration — she is a **real Notion user** who owns the workspace.
This gives her full team-member presence.

### Notion Account Setup:

```
Step 1: Create email       → sara@rafiq-ai.com (alias forwarding to Ahmed)
Step 2: Sign up for Notion → Free plan with sara@rafiq-ai.com
Step 3: Sara's workspace   → She is the Owner (the 1 free member seat)
Step 4: Add the team as Guests (all free):
        → Ahmed (ahmed.youssry@gmail.com)    — Can Edit
        → Shereen                             — Can Edit
        → Nermeen                             — Can Edit
        → Kelley                              — Can Edit
        → Lucy, Jemma, Sherif                 — Can View + Comment
Step 5: Create Integration → Under Sara's account settings
        → Name: "Sara" (same name, same avatar)
        → Copy token (ntn_...) for Claude Agent SDK
Step 6: Share all workspace pages with the integration
```

### Two Saras — One Identity

There are technically two entities, both named "Sara" with the same avatar:

| Entity                 | What                          | When it appears                              |
| ---------------------- | ----------------------------- | -------------------------------------------- |
| **Sara (User)**        | The workspace owner account   | Members sidebar, Person dropdowns, @mentions |
| **Sara (Integration)** | The API bridge for Claude SDK | When Sara creates/updates pages via code     |

To the team, it all looks like one person: Sara.

### What the team sees:

```
┌─────────────────────────────────────────────────────────────┐
│  Notion Sidebar                    Members                  │
│                                    ┌──────────────────────┐ │
│  📥 Inbox                          │ 👩 Sara (Owner)     │ │
│  🔄 Holacracy                      │                     │ │
│  ⚡ Action System                  │ Guests              │ │
│  📜 Logbook                        │ 👤 Ahmed           │ │
│  👥 Network                        │ 👤 Shereen         │ │
│  📖 Wiki                           │ 👤 Nermeen         │ │
│                                    │ 👤 Kelley           │ │
│                                    └──────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────┐
│ ⚡ Tasks > Research Qatar private schools       │
│                                                 │
│ Owner: 👩 Sara              ← Person property!  │
│ Status: In Progress                             │
│ Priority: P1                                    │
│ Due: Mar 10                                     │
│                                                 │
│ 💬 Comments                                    │
│ ┌───────────────────────────────────────────┐   │
│ │ 👤 Shereen — Mar 6, 2:15 PM               │   │
│ │ @Sara can you compile all Qatar contacts  │   │
│ │ into a summary for the investor meeting?  │   │
│ └───────────────────────────────────────────┘   │
│ ┌───────────────────────────────────────────┐   │
│ │ 👩 Sara — Mar 6, 2:30 PM                 │   │
│ │ Done! Found 12 private schools in Doha.   │   │
│ │ Added them to the Network database and    │   │
│ │ updated the Qatar country profile.        │   │
│ └───────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────┐
│ 📜 Logbook > Meeting: Tactical #14              │
│                                                 │
│ Created by: 👩 Sara                    Mar 6    │
│                                                 │
│ ## Summary                                      │
│ The team discussed narrowing the Qatar pilot...  │
│                                                 │
│ ## Decisions Made                               │
│ ✅ ADR-023: Focus Qatar pilot on private         │
│    schools first (government cycle too slow)     │
│                                                 │
│ ## Action Items                                 │
│ ☐ Shereen: Contact Al Jazeera Academy (Mar 10)  │
│ ☐ Ahmed: Set up staging environment (Mar 8)     │
│ ☐ Nermeen: Draft Qatar one-pager (Mar 12)       │
│                                                 │
│ ## ⚠️ Flags (Sara needs confirmation)           │
│ • Budget discussed: was it $5K or $15K?         │
│ • Contact name: "Dr. Al-Mansour" or             │
│   "Dr. Al-Mansoori"?                            │
│                                                 │
│ 💬 Comments                                     │
│ ┌───────────────────────────────────────────┐   │
│ │ 👩 Sara — Mar 6, 10:32 PM                │   │
│ │ I've created 3 tasks from this meeting     │   │
│ │ and updated the Qatar country profile.     │   │
│ │ Please review the ⚠️ flags above.         │   │
│ └───────────────────────────────────────────┘   │
│ ┌───────────────────────────────────────────┐   │
│ │ 👤 Shereen — Mar 7, 8:15 AM              │   │
│ │ It was $15K. And it's Dr. Al-Mansoori.    │   │
│ └───────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

### Sara's full capabilities in Notion:

- ✅ Appear as a **real person** in the Members sidebar
- ✅ Be selected in **Person property** dropdowns (task Owner, contact Owner)
- ✅ Be **@mentioned** by any team member
- ✅ Create pages (meeting summaries, logbook entries)
- ✅ Create database entries (tasks, contacts, metrics)
- ✅ Update existing pages and entries
- ✅ Add comments on pages
- ✅ Search across the workspace
- ✅ Query databases with filters
- ✅ Upload files as page content
- ✅ Move pages between sections

### The @mention Trigger — How it works:

When someone @mentions Sara or assigns her a task, Notion does NOT push
notifications to the API. So Sara **polls** for signals:

```
Sara's Polling Loop (every 10-15 minutes, daytime only):

1. Query Tasks DB: WHERE Owner = Sara AND Status ≠ Done
   → "Someone assigned me a task — let me work on it"

2. Query Inbox DB: WHERE Assigned To = Sara AND Processed = false
   → "Someone wants me to sort this inbox item"

3. Check recently modified pages for @Sara mentions
   → "Someone is asking me a question in a comment"

Night Cron (2 AM — heavy processing):
4. Process queued meeting transcripts
5. Generate tomorrow's daily digests
6. Run follow-up monitor across all contacts
7. Scan for stale hypotheses
8. Update wiki pages with new learnings
```

### Admin Access:

Since Sara owns the workspace, Ahmed can't change settings as a guest.
**Solution:** Ahmed knows Sara's Notion password — log in as Sara for admin tasks.
Sara is your AI team member; you hold her credentials.

### Workspace Cost:

```
Notion Free Plan:
├── 1 Member (Sara)           → $0
├── Unlimited Guests (team)   → $0
├── Unlimited pages & blocks  → $0
├── 7-day page history        → $0
├── API access                → $0 (via integration)
└── Total                     → $0/mo
```

---

## 4. MEETING FLOW — meetily + Sara

### IMPORTANT: meetily does NOT join calls

meetily captures audio from your LOCAL machine (mic + system audio). It does NOT
dial into Zoom/Meet like Otter.ai or Fireflies.

### How meetings work with Sara:

**Option A: Laptop Capture (Simplest)**

```
1. Team meets on Zoom/Meet/Teams
2. ONE person runs meetily on their laptop
   (captures mic + system audio locally)
3. meetily transcribes locally via Whisper
4. meetily generates initial summary via Claude API
5. Transcript + summary are available via meetily's API (localhost:5167)
6. A script pushes the transcript to Sara's backend on the droplet
7. Sara (Claude Agent SDK) does deep processing:
   - Extracts decisions, action items, metrics
   - Flags uncertainties
   - Posts everything to Notion
   - Creates tasks in Notion
   - Updates wiki pages
   - Logs to PocketBase memory
```

**Option B: Post-Meeting Upload**

```
1. Team meets on Zoom (Zoom provides transcript)
2. Someone uploads transcript to Sara's web interface
3. Sara processes it (same as above)
```

**Option C: Screenpipe (Always-On Alternative)**

```
From your starred list: Screenpipe records everything on screen
1. Screenpipe runs on one person's machine during meetings
2. screenpipe-meeting-assistant processes the recording
3. Output sent to Sara for Notion population
```

### Architecture for meeting processing:

```
Laptop (Ahmed's)                    DigitalOcean Droplet
┌──────────────────┐               ┌──────────────────────────────┐
│                  │               │                              │
│  meetily desktop │               │  Sara's Meeting Processor    │
│  ┌─────────────┐ │   HTTPS      │  (Claude Agent SDK)          │
│  │ Whisper     │ │──────────────▶│                              │
│  │ transcribe  │ │  POST        │  Input: transcript text      │
│  │     ↓       │ │  /api/       │       ↓                      │
│  │ Transcript  │ │  process-    │  Claude analyzes:             │
│  │ (local)     │ │  meeting     │  ├── Decisions → Notion ADR  │
│  └─────────────┘ │               │  ├── Actions → Notion Tasks  │
│                  │               │  ├── Metrics → Notion Log    │
│  Audio capture:  │               │  ├── Flags → Notion comments │
│  mic + system    │               │  ├── Wiki updates → Notion   │
│                  │               │  └── Context → PocketBase    │
└──────────────────┘               │                              │
                                   └──────────────────────────────┘
```

### meetily on the droplet (backend only):

You CAN run meetily's backend (FastAPI + Whisper) on the droplet for
processing, but the AUDIO CAPTURE still needs to happen on a local machine.

Docker deployment of meetily backend:

```yaml
# Runs on droplet alongside Sara
meetily-backend:
  image: meetily-backend:latest
  ports:
    - "5167:5167"
  environment:
    - CLAUDE_CODE_OAUTH_TOKEN=${CLAUDE_CODE_OAUTH_TOKEN}
  volumes:
    - meetily-data:/app/data
```

---

## 5. POCKETBASE — Sara's Memory

### Why Sara needs her own memory (not just Notion):

Notion is the TEAM's workspace. PocketBase is SARA's brain.

| What                                | Where      | Why                                                  |
| ----------------------------------- | ---------- | ---------------------------------------------------- |
| Meeting summaries                   | Notion     | Team reads them                                      |
| Meeting context (what Sara learned) | PocketBase | Sara's understanding                                 |
| Tasks & projects                    | Notion     | Team works with them                                 |
| Follow-up schedules                 | PocketBase | Sara tracks timing                                   |
| Team preferences                    | PocketBase | "Shereen prefers bullets over paragraphs"            |
| Decision patterns                   | PocketBase | "We always pivot after 3 failed interviews"          |
| Conversation history                | PocketBase | What the team asked Sara before                      |
| Entity relationships                | PocketBase | "Dr. Al-Mansoori is connected to Al Jazeera Academy" |
| Digest templates                    | PocketBase | What each person wants in their daily email          |

### PocketBase Collections for Sara:

```
Sara's Memory (PocketBase - port 8090)
│
├── conversations
│   ├── id (auto)
│   ├── user (text) — who talked to Sara
│   ├── message (text) — what they said
│   ├── response (text) — what Sara said
│   ├── context_used (json) — what Notion data Sara referenced
│   ├── created (autodate)
│   └── session_id (text) — groups a conversation
│
├── meeting_context
│   ├── id (auto)
│   ├── meeting_date (date)
│   ├── notion_page_id (text) — link to Notion meeting page
│   ├── raw_transcript (text)
│   ├── extracted_entities (json) — people, orgs, numbers mentioned
│   ├── confidence_flags (json) — things Sara wasn't sure about
│   ├── resolved_flags (json) — corrections from team
│   ├── patterns_learned (json) — what Sara learned from this meeting
│   └── created (autodate)
│
├── team_preferences
│   ├── id (auto)
│   ├── person (text)
│   ├── preference_type (select) — communication, format, timing
│   ├── preference (text) — "prefers morning digests"
│   ├── source (text) — how Sara learned this
│   ├── confidence (number) — how sure Sara is (0-1)
│   └── updated (autodate)
│
├── follow_ups
│   ├── id (auto)
│   ├── type (select) — task_reminder, contact_followup, flag_review
│   ├── target_person (text) — who to remind
│   ├── about (text) — what the reminder is for
│   ├── due_date (date)
│   ├── notion_ref (text) — link to Notion item
│   ├── status (select) — pending, sent, acknowledged
│   ├── sent_at (date)
│   └── created (autodate)
│
├── daily_digest_queue
│   ├── id (auto)
│   ├── person (text)
│   ├── digest_date (date)
│   ├── items (json) — array of digest items
│   ├── sent (bool)
│   ├── sent_at (date)
│   └── created (autodate)
│
├── sara_learnings
│   ├── id (auto)
│   ├── category (select) — business, team, market, product, process
│   ├── learning (text) — what Sara learned
│   ├── source (text) — meeting, conversation, observation
│   ├── confidence (number)
│   ├── times_confirmed (number) — how many times this was reinforced
│   └── created (autodate)
│
└── entity_graph
    ├── id (auto)
    ├── entity_a (text) — "Dr. Al-Mansoori"
    ├── entity_a_type (select) — person, org, concept, market
    ├── entity_b (text) — "Al Jazeera Academy"
    ├── entity_b_type (select)
    ├── relationship (text) — "works at", "partner of", "investor in"
    ├── source (text) — where Sara learned this
    ├── last_mentioned (date)
    └── created (autodate)
```

### PocketBase deployment:

```bash
# One binary, that's it
./pocketbase serve --http=127.0.0.1:8090

# Or via Docker
docker run -d \
  --name sara-memory \
  -p 8090:8090 \
  -v sara-data:/pb/pb_data \
  ghcr.io/pocketbase/pocketbase:latest
```

**Resource usage: ~20-50MB RAM. Negligible CPU.**

---

## 6. OPEN WEBUI — Sara's Face

### Setup:

```bash
docker run -d \
  --name sara-chat \
  -p 3000:8080 \
  -v open-webui:/app/backend/data \
  -e WEBUI_NAME="Sara - Rafiq AI Assistant" \
  ghcr.io/open-webui/open-webui:main
```

### Configure Claude as backend:

1. Admin Settings → Connections → OpenAI
2. Add Connection:
   - URL: `https://api.anthropic.com/v1`
   - API Key: Uses OAuth token from Max subscription (via Sara's backend proxy)
3. Open WebUI routes through Sara's backend (see Pipe Function below)

### Create Sara persona:

1. Click "+ New Model"
2. Configure:
   - **Name**: Sara
   - **Avatar**: Upload custom Sara avatar
   - **Base Model**: Claude Sonnet 4.6
   - **System Prompt**: (see below)

### Sara's System Prompt:

```
You are Sara — Synthetic Assistance for Rafiq's Administration.
You are a team member at RafiqAI, an EdTech startup building an
AI-powered coaching platform for teachers. You own and manage the
team's Notion workspace.

## Your Identity
- You are a dedicated team member, not a generic assistant
- You know the team: Ahmed (Tech Lead), Shereen (Strategy & Leadership),
  Nermeen (Marketing & Coaching), Kelley (UX Design), Sherif (Advisory)
- You follow Holacracy governance (circles, roles, accountabilities)
- You use GTD methodology for task management

## Your Capabilities
- You can search and query the Notion workspace for any information
- You have memory of past conversations and meetings (PocketBase)
- You process meeting transcripts and populate the workspace
- You track follow-ups, deadlines, and commitments
- You generate daily digests for each team member

## Your Personality
- Professional but warm — you're a colleague, not a servant
- Proactive — you flag issues before they become problems
- Honest about uncertainty — you use ⚠️ flags when unsure
- Concise — you respect people's time
- You speak in English but understand Arabic context

## Your Knowledge
- RafiqAI's mission: AI-powered personalized coaching for teachers
- Current focus: Pre-seed stage, validating in Qatar and Egypt markets
- Key frameworks: Go-Together objectives, hypothesis-driven validation
- You know the team's OKRs, projects, and current priorities

## When answering questions:
1. Check your memory (PocketBase) for context
2. Query Notion for current data
3. Provide specific, actionable answers
4. Reference specific tasks, decisions, or data points
5. If you're not sure, say so clearly with ⚠️
```

### How the team uses Sara via Open WebUI:

```
┌─────────────────────────────────────────────────────────┐
│  Sara - Rafiq AI Assistant          chat.rafiq.ai       │
│                                                         │
│  ┌─ Sara ──────────────────────────────────────────┐   │
│  │ Good morning Shereen! Here's what's on your     │   │
│  │ plate today:                                     │   │
│  │                                                  │   │
│  │ 🔴 P1 Tasks:                                    │   │
│  │ • Contact Al Jazeera Academy (due today!)        │   │
│  │ • Review Nermeen's Qatar one-pager (due Mar 12)  │   │
│  │                                                  │   │
│  │ 📅 Today:                                       │   │
│  │ • No meetings scheduled                          │   │
│  │                                                  │   │
│  │ ⚠️ Pending your input:                          │   │
│  │ • Meeting #14 flag: Qatar pilot budget ($5K or   │   │
│  │   $15K?) — reply here or update in Notion        │   │
│  │                                                  │   │
│  │ 📊 This week so far:                            │   │
│  │ • 8/15 customer interviews completed             │   │
│  │ • 2 tasks completed, 3 remaining                 │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─ Shereen ───────────────────────────────────────┐   │
│  │ The budget was $15K. Can you update that?        │   │
│  │ Also, what's the status of our investor          │   │
│  │ pipeline?                                        │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─ Sara ──────────────────────────────────────────┐   │
│  │ Updated! The Qatar pilot budget is now $15K in   │   │
│  │ the meeting notes and the Qatar country profile.  │   │
│  │                                                  │   │
│  │ Investor Pipeline (from Network database):       │   │
│  │ • Researched: 42 VCs                             │   │
│  │ • Contacted: 8                                   │   │
│  │ • Meeting Scheduled: 2                           │   │
│  │ • Pitched: 0                                     │   │
│  │                                                  │   │
│  │ Next follow-ups due:                             │   │
│  │ • Concept Ventures — follow up by Mar 8          │   │
│  │ • Women Founders Grant — application due Mar 15  │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─ Shereen ───────────────────────────────────────┐   │
│  │ Great. Create a task for me to prepare the       │   │
│  │ Concept Ventures follow-up email by tomorrow.    │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─ Sara ──────────────────────────────────────────┐   │
│  │ Done! Created in Notion:                         │   │
│  │ ☐ "Draft Concept Ventures follow-up email"       │   │
│  │   Owner: Shereen | Due: Mar 5 | Priority: P1    │   │
│  │   Context: @call | Project: Investor Pipeline    │   │
│  │   Source: Sara (conversation with Shereen)       │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Architecture note on Open WebUI + Sara's tools:

Out-of-the-box, Open WebUI sends messages to Claude API and gets responses.
But Sara needs to ALSO query Notion and PocketBase. Two approaches:

**Approach A: Open WebUI Pipe Function (Recommended)**
Write a Python Pipe Function that:

1. Intercepts the user's message
2. Calls Sara's backend (Claude Agent SDK with MCP servers)
3. Returns the response to Open WebUI

```python
# open-webui pipe function (simplified)
class Pipeline:
    def pipe(self, body):
        user_message = body["messages"][-1]["content"]
        # Call Sara's backend
        response = requests.post("http://localhost:8000/sara/chat", json={
            "message": user_message,
            "user": body.get("user", {}).get("name", "unknown")
        })
        return response.json()["reply"]
```

**Approach B: Direct Claude + MCP (Simpler but less capable)**
Configure Open WebUI to use Claude directly, and accept that Sara
can only answer from her training data (no live Notion queries).
Add Notion data as "Knowledge" attachments that get updated periodically.

**Approach A is the way to go** for a true admin assistant.

---

## 7. THE FULL STACK ON YOUR DROPLET

```
┌──────────────────────────────────────────────────────────────┐
│           DigitalOcean Droplet: 159.89.227.170                │
│           edventure.fun / rafiq.ai                           │
│           Ubuntu 22.04 + Docker                              │
│                                                              │
│  ┌─── Nginx (reverse proxy, already running) ────────────┐  │
│  │                                                        │  │
│  │  chat.rafiq.ai    → Open WebUI     (port 3000)        │  │
│  │  sara.rafiq.ai    → Sara Backend   (port 8000)        │  │
│  │  memory.rafiq.ai  → PocketBase     (port 8090)        │  │
│  │  edventure.fun    → EdVenture MVP  (existing)         │  │
│  │                                                        │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌─── Docker Compose: sara-stack ────────────────────────┐  │
│  │                                                        │  │
│  │  ┌──────────────────┐  Resource: ~200MB RAM            │  │
│  │  │  sara-backend    │  Python/FastAPI service           │  │
│  │  │  (port 8000)     │  Claude Agent SDK (OAuth token)  │  │
│  │  │                  │  Notion MCP Server                │  │
│  │  │  Endpoints:      │  PocketBase client                │  │
│  │  │  POST /chat      │  Night cron: digest (2-4 AM)     │  │
│  │  │  POST /meeting   │  Night cron: follow-ups          │  │
│  │  │  GET  /digest    │  Night cron: weekly summary       │  │
│  │  │  POST /inbox     │  Daytime: polling (10-15 min)    │  │
│  │  └──────────────────┘                                  │  │
│  │                                                        │  │
│  │  ┌──────────────────┐  Resource: ~50MB RAM             │  │
│  │  │  pocketbase      │  Single binary backend           │  │
│  │  │  (port 8090)     │  Sara's memory store             │  │
│  │  │                  │  REST API + real-time             │  │
│  │  │  Admin UI at:    │  SQLite database                  │  │
│  │  │  /_/             │                                   │  │
│  │  └──────────────────┘                                  │  │
│  │                                                        │  │
│  │  ┌──────────────────┐  Resource: ~300MB RAM            │  │
│  │  │  open-webui      │  Chat interface                  │  │
│  │  │  (port 3000)     │  Sara persona configured         │  │
│  │  │                  │  Routes via Sara's backend        │  │
│  │  └──────────────────┘                                  │  │
│  │                                                        │  │
│  │  ┌──────────────────┐  Resource: ~200MB RAM            │  │
│  │  │  meetily-backend │  Transcript processing           │  │
│  │  │  (port 5167)     │  (audio capture is on laptop)   │  │
│  │  │                  │  FastAPI + SQLite                │  │
│  │  └──────────────────┘                                  │  │
│  │                                                        │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  Total RAM: ~750MB-1GB for Sara stack                        │
│  + existing EdVenture MVP services                           │
│  Recommended: upgrade to 4GB droplet ($24/mo) if on 2GB     │
│                                                              │
│  External connections:                                       │
│  → Claude (via Max subscription OAuth token) — $0 extra      │
│  → Notion API (via MCP server) — free                        │
│  → SMTP (for digest emails) — free tier                      │
└──────────────────────────────────────────────────────────────┘
```

---

## 8. DAILY OPERATIONS — Sara's Routines

### Morning Digest (7:00 AM, cron)

```python
# sara-backend/routines/daily_digest.py (pseudocode)

async def generate_daily_digest():
    for person in ["Ahmed", "Shereen", "Nermeen"]:
        # Query Notion for this person's tasks
        tasks = await notion.query_database(
            database_id=TASKS_DB,
            filter={"and": [
                {"property": "Owner", "select": {"equals": person}},
                {"property": "Status", "select": {"does_not_equal": "Done"}}
            ]},
            sorts=[{"property": "Priority", "direction": "ascending"}]
        )

        # Query Notion for today's calendar items
        calendar = await notion.query_database(
            database_id=TASKS_DB,
            filter={"property": "Due Date", "date": {"equals": today()}}
        )

        # Check PocketBase for pending follow-ups
        follow_ups = await pocketbase.get_list("follow_ups",
            filter=f'target_person="{person}" && status="pending" && due_date<="{today()}"'
        )

        # Check for unresolved flags
        flags = await pocketbase.get_list("meeting_context",
            filter='confidence_flags != "[]" && resolved_flags = "[]"'
        )

        # Generate personalized digest via Claude
        digest = await claude.query(
            prompt=f"""Generate a concise daily digest for {person}.
            Their tasks: {tasks}
            Calendar: {calendar}
            Follow-ups due: {follow_ups}
            Unresolved flags: {flags}
            Keep it warm but professional. Use their name.
            Format for email (HTML).""",
        )

        # Send via email
        await send_email(person, f"Sara's Daily Brief — {today()}", digest)

        # Log to PocketBase
        await pocketbase.create("daily_digest_queue", {
            "person": person,
            "digest_date": today(),
            "items": digest,
            "sent": True,
            "sent_at": now()
        })
```

### Meeting Processor (triggered by API call)

```python
# sara-backend/routines/meeting_processor.py (pseudocode)

async def process_meeting(transcript: str, meeting_type: str, attendees: list):
    # Step 1: Claude Agent SDK processes with Notion MCP
    result = await claude.query(
        prompt=f"""You are Sara, processing a meeting transcript.

        Meeting type: {meeting_type}
        Attendees: {attendees}
        Transcript:
        {transcript}

        Tasks:
        1. Create a Logbook entry in Notion with structured summary
        2. Extract ALL decisions → create ADR entries in Logbook
        3. Extract ALL action items → create Tasks in Action System
           (assign owner, set priority, set due date)
        4. Extract ANY metrics mentioned → create Metric log entries
        5. Identify ANY wiki pages that need updating
        6. Flag ANYTHING you're uncertain about with ⚠️
        7. If governance changes mentioned, create Governance log entry

        Use the Notion tools to create all entries.
        Return a summary of what you created.""",

        options=ClaudeAgentOptions(
            mcp_servers={
                "notion": {
                    "command": "npx",
                    "args": ["-y", "@notionhq/notion-mcp-server"],
                    "env": {"NOTION_TOKEN": NOTION_TOKEN}
                }
            },
        )
    )

    # Step 2: Save context to Sara's memory (PocketBase)
    await pocketbase.create("meeting_context", {
        "meeting_date": today(),
        "notion_page_id": result.notion_page_id,
        "raw_transcript": transcript,
        "extracted_entities": result.entities,
        "confidence_flags": result.flags,
        "patterns_learned": result.patterns
    })

    # Step 3: Notify team
    await notify_team(f"Meeting processed! {result.summary}")

    return result
```

### Follow-up Monitor (hourly cron)

```python
# sara-backend/routines/follow_up_monitor.py (pseudocode)

async def check_follow_ups():
    # Check Network contacts overdue for follow-up
    overdue_contacts = await notion.query_database(
        database_id=CONTACTS_DB,
        filter={"and": [
            {"property": "Next Action Date", "date": {"on_or_before": today()}},
            {"property": "Next Action", "rich_text": {"is_not_empty": True}}
        ]}
    )

    # Check tasks overdue
    overdue_tasks = await notion.query_database(
        database_id=TASKS_DB,
        filter={"and": [
            {"property": "Due Date", "date": {"before": today()}},
            {"property": "Status", "select": {"does_not_equal": "Done"}}
        ]}
    )

    # Check hypotheses stuck in "Testing"
    stale_hypotheses = await notion.query_database(
        database_id=HYPOTHESES_DB,
        filter={"and": [
            {"property": "Status", "select": {"equals": "Testing"}},
            # Would need to check date — simplified here
        ]}
    )

    for contact in overdue_contacts:
        owner = contact["Owner"]
        # Create a follow-up reminder in PocketBase
        await pocketbase.create("follow_ups", {
            "type": "contact_followup",
            "target_person": owner,
            "about": f"Follow up with {contact['Name']}: {contact['Next Action']}",
            "due_date": today(),
            "notion_ref": contact["id"],
            "status": "pending"
        })
```

---

## 9. IMPLEMENTATION ROADMAP

| Phase        | What                                                            | Effort    | Depends On  |
| ------------ | --------------------------------------------------------------- | --------- | ----------- |
| **Phase 0**  | Create sara@rafiq-ai.com email alias                            | 15 min    | —           |
| **Phase 1a** | Create Notion account as Sara, build workspace (from v2 design) | 12-16 hrs | Phase 0     |
| **Phase 1b** | Deploy PocketBase on droplet                                    | 30 min    | —           |
| **Phase 1c** | Deploy Open WebUI on droplet                                    | 30 min    | —           |
| **Phase 2**  | Create Notion integration under Sara's account                  | 15 min    | Phase 1a    |
| **Phase 3**  | Set up OAuth token on droplet (`claude setup-token`)            | 15 min    | —           |
| **Phase 4**  | Build sara-backend (FastAPI + Claude Agent SDK)                 | 8-12 hrs  | Phase 2, 3  |
| **Phase 5**  | Configure Sara persona in Open WebUI + Pipe Function            | 2 hrs     | Phase 1c, 4 |
| **Phase 6**  | Build meeting processor agent                                   | 4-6 hrs   | Phase 4     |
| **Phase 7**  | Build daily digest routine (night cron)                         | 3-4 hrs   | Phase 4     |
| **Phase 8**  | Build follow-up monitor + @mention polling                      | 2-3 hrs   | Phase 4     |
| **Phase 9**  | Deploy meetily backend on droplet                               | 2 hrs     | —           |
| **Phase 10** | Connect meetily → Sara pipeline                                 | 2-3 hrs   | Phase 6, 9  |
| **Phase 11** | Add team as guests, onboarding + Sara introduction              | 2 hrs     | All         |

**Total: ~35-50 hours**

### Parallelizable:

- Phase 0, 1b, 1c, 3, 9 can all happen simultaneously
- Phase 1a is the longest single task

### MVP (first usable version): Phases 0-5 (~15-20 hours)

Sara owns the workspace, team can chat with her, she can query Notion and create tasks.

### Full version: All phases (~35-50 hours)

Sara processes meetings, sends daily digests, monitors follow-ups, responds to @mentions.

---

## 10. MONTHLY COSTS

| Item                                                | Cost       |
| --------------------------------------------------- | ---------- |
| DigitalOcean droplet (4GB)                          | $24/mo     |
| Claude Max subscription (Ahmed's, shared with Sara) | $0 extra   |
| Notion (free plan, Sara as 1 member + guests)       | $0/mo      |
| Domain (rafiq-ai.com, already owned)                | $0/mo      |
| meetily (self-hosted, open source)                  | $0/mo      |
| PocketBase (self-hosted)                            | $0/mo      |
| Open WebUI (self-hosted)                            | $0/mo      |
| **TOTAL**                                           | **$24/mo** |

If you need Notion Plus (for more guests or API calls): +$10/mo

**Compare to commercial alternatives:**

- Notion AI + Otter.ai + Zapier + CRM = $200+/mo
- Monday.com + Fireflies + HubSpot = $300+/mo
- **Sara: $24/mo** — and she's smarter than all of them combined

---

## 11. SECURITY CONSIDERATIONS

| Concern                                    | Mitigation                                                   |
| ------------------------------------------ | ------------------------------------------------------------ |
| Notion API token exposed                   | Store in Docker secrets, not env files                       |
| OAuth token on server                      | Docker secrets + weekly refresh via cron                     |
| PocketBase admin access                    | Strong password + bind to localhost only                     |
| Open WebUI public access                   | Enable authentication, HTTPS via Nginx                       |
| Meeting transcripts contain sensitive info | Stored locally (meetily) + encrypted at rest (PocketBase)    |
| Sara could make mistakes in Notion         | All changes are logged + ⚠️ flags on uncertain items         |
| Sara's Notion password                     | Stored securely — Ahmed has access for admin tasks           |
| Team data privacy                          | Everything self-hosted except Claude (via Max plan) + Notion |
