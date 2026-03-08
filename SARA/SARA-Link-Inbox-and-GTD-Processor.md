# SARA Link Inbox & GTD Processor

**Feature Specification — Capture, Organize, Act**

**For:** RafiqAI Founding Team
**Authors:** Mr Be + Claude
**Date:** March 2026
**Status:** Brainstorm Complete — Ready for Build Planning
**Capability ID:** S20 — Link Inbox / GTD Processor

-----

## 1. The Problem

Every day, the team encounters dozens of interesting links — articles, competitor announcements, research papers, funding news, tools, inspiration. These links appear while scrolling LinkedIn, reading Twitter, browsing the web, or receiving messages from contacts.

Today, these links die in browser tabs, get lost in bookmarks folders nobody opens, or disappear into Slack threads that scroll away. There is no system to capture them quickly, process them thoughtfully, and route them to where they create value.

This is a classic GTD (Getting Things Done) problem: the capture moment and the processing moment are different. You find a link during a quick scroll on the train. You have time to think about it during your morning routine. The system needs to respect that gap.

-----

## 2. The Vision

SARA becomes a **personal knowledge inbox** for the entire team. Two taps to capture. AI-powered organization. Scheduled review sessions that turn raw links into actionable knowledge.

The principle: **capture fast, process later, never lose anything.**

-----

## 3. The Capture Experience

### 3.1 Mobile Capture (Primary)

The team member is on their phone — scrolling LinkedIn, reading an article, browsing Twitter. They find something worth saving.

**Flow:**

1. Tap the share button on the phone (iOS or Android native share sheet)
1. Select Rocket.Chat from the share targets
1. Pick the DM with SARA (or the #inbox channel)
1. Optionally add a quick voice note or text: “competitive intel” or “use for pitch” or “interesting AI research”
1. Send

**Total time: 3–5 seconds. Back to scrolling.**

Rocket.Chat’s mobile app already supports receiving shared content from other apps — URLs, images, files, and text. No custom development needed for the share sheet integration.

### 3.2 Desktop Capture

From a browser, the team member can:

- Paste a link into the #inbox channel or DM with SARA in Rocket.Chat desktop
- Use a slash command: `/sara save [URL] [optional note]`
- Forward an email containing a link to SARA (future: when Stalwart email is active)

### 3.3 Voice Capture

In Rocket.Chat mobile, record a voice message to SARA:

> “Sara, save this — I just saw that Alef Education raised a Series B, look it up and save it for competitive research”

SARA transcribes via Groq Whisper, identifies the intent, searches the web for the relevant link, and captures it.

### 3.4 Capture During Conversation

During a regular chat with SARA, if a link comes up naturally:

> **You:** “Sara, what do you know about Nafham’s pricing model?”
> **SARA:** “Based on their website, Nafham offers… [response]. Here’s their pricing page: [link]”
> **You:** “Save that link for the competitive analysis”
> **SARA:** “Saved to your inbox. Tagged: competitor, pricing, Nafham.”

-----

## 4. What SARA Does on Capture

When a link arrives in the inbox, SARA immediately processes it in the background using the triage LLM profile (cheap, fast):

### 4.1 Content Extraction

- Fetch the page using Scrapling (handles anti-bot, paywalls, JS-rendered content)
- Extract clean content using newspaper4k (title, author, publish date, body text, images)
- Convert to markdown via markdownify
- Detect language using langdetect (Arabic, English, or mixed)

### 4.2 AI Classification

SARA classifies each link into one or more categories:

|Category                |Examples                                               |
|------------------------|-------------------------------------------------------|
|**Competitive Intel**   |Competitor funding, product launches, pricing changes  |
|**Funding & Investors** |VC announcements, fund launches, LP news               |
|**EdTech Research**     |Academic papers, industry reports, market analysis     |
|**AI & Technology**     |New models, API changes, framework releases, tools     |
|**Content Inspiration** |Posts to reference, formats to emulate, trending topics|
|**Policy & Regulation** |Education policy, data privacy, government programs    |
|**Talent & Hiring**     |Job market trends, compensation data, key hires        |
|**Partnership Leads**   |Potential schools, NGOs, corporate partners            |
|**Personal Development**|Books, courses, talks, frameworks                      |
|**General Interest**    |Doesn’t fit above — saved for later review             |

### 4.3 Metadata Generation

For each captured link, SARA creates a structured record:

```yaml
id: inbox-2026-03-07-001
url: https://techcrunch.com/2026/03/06/alef-education-series-b
title: "Alef Education Raises $30M Series B to Expand AI Learning Platform"
source: TechCrunch
author: Sarah Perez
published: 2026-03-06
captured_at: 2026-03-07T15:42:00+02:00
captured_by: mr-be
user_note: "competitive intel"
language: en
category: [competitive-intel, funding-investors]
tags: [alef-education, series-b, mena, edtech, competitor]
status: unprocessed
summary: >
  Alef Education closed a $30M Series B led by Emaar Capital.
  The UAE-based edtech company plans to expand into Egypt and
  Saudi Arabia. Their AI-powered learning platform now serves
  200+ schools across the Gulf.
sara_analysis: >
  Direct competitor in MENA edtech space. Their funding round
  is 3x our target raise. Egypt expansion is directly relevant
  to our market. Worth tracking their school acquisition strategy.
relevance_score: 0.92
suggested_destination: Research > Competitive Landscape
suggested_actions:
  - "Update competitive analysis doc"
  - "Mention in next investor update"
  - "Research their Egypt partners"
```

### 4.4 Confirmation Message

After processing (usually 5–15 seconds), SARA confirms in chat:

```
SARA: "Saved ✓"
      ┌──────────────────────────────────┐
      │ 📰 Alef Education Raises $30M    │
      │    Series B                      │
      │                                  │
      │ 🏷️ competitive-intel, funding    │
      │ 📂 Suggested: Research >         │
      │    Competitive Landscape         │
      │                                  │
      │ Will include in your next review │
      └──────────────────────────────────┘
```

-----

## 5. The GTD Review Session

This is where raw captures become organized knowledge and actionable items. SARA initiates review sessions based on each team member’s schedule, configured in their user file (Users/{name}.md).

### 5.1 Review Schedule

Each team member defines when they want to process their inbox:

```markdown
# Mr Be.md (excerpt)

## Inbox Preferences
- Review time: 8:30 AM EET (morning commute)
- Review frequency: Daily
- Max items per session: 10
- Auto-archive after: 7 days unprocessed (with reminder on day 5)
- Preferred review style: Quick decisions (show summary + actions, minimal detail)
```

### 5.2 Review Initiation

At the scheduled time, SARA starts the review in DM:

```
SARA: "Morning Mr Be — you have 7 items in your inbox 
       from the last 2 days. Ready to process?"

       [Start Review]  [Snooze 1 hour]  [Skip Today]
```

The review can also be triggered manually:

- `/sara inbox` — start review now
- `/sara inbox count` — how many items pending
- `/sara inbox [category]` — review only competitive intel, for example

### 5.3 Review Flow — One Item at a Time

SARA presents each item with her analysis and suggested actions:

```
SARA: "Item 1 of 7:"
      ┌──────────────────────────────────┐
      │ 📰 Alef Education Raises $30M    │
      │    Series B                      │
      │                                  │
      │ Source: TechCrunch               │
      │ Saved: Yesterday, 3:42 PM       │
      │ Your note: "competitive intel"   │
      │                                  │
      │ SARA's take: Direct competitor   │
      │ in MENA edtech. Their round is   │
      │ 3x larger than our target raise. │
      │ Relevant to investor pitch.      │
      │                                  │
      │ [Save to Research]               │
      │ [Add to Pitch Deck Notes]        │
      │ [Share with Team]                │
      │ [Create Action Item]             │
      │ [Read Later]                     │
      │ [Archive]                        │
      │ [Delete]                         │
      └──────────────────────────────────┘
```

### 5.4 Action Definitions

Each action button maps to a real operation:

|Action                   |What SARA Does                                                                                    |Backend                              |
|-------------------------|--------------------------------------------------------------------------------------------------|-------------------------------------|
|**Save to Research**     |Creates doc in Outline Research collection with full extracted content, tags, and SARA’s summary  |Outline API                          |
|**Add to [specific doc]**|Appends a summary block to an existing Outline document (competitive landscape, pitch notes, etc.)|Outline API                          |
|**Share with Team**      |Posts to the relevant Rocket.Chat channel (#research, #general) with SARA’s summary and the link  |Rocket.Chat API                      |
|**Create Action Item**   |Opens the Add Task widget pre-filled: “Review Alef Education’s Egypt expansion strategy”          |Outline + Rocket.Chat widget         |
|**Read Later**           |Marks as “deferred” — SARA will re-surface it in the next review session                          |Internal state                       |
|**Start Research**       |Adds to SARA’s Research Queue — she’ll do a deep dive and produce a brief                         |Outline + sara-api research processor|
|**Archive**              |Moves to archived state — searchable but not in active inbox                                      |Internal state                       |
|**Delete**               |Permanently removes from inbox                                                                    |Internal state                       |

### 5.5 Quick Processing Mode

For fast review (like during a commute), SARA can present items in a streamlined format:

```
SARA: "Quick review — 7 items. Swipe through:"

      1/7: Alef Education $30M Series B (TechCrunch)
      SARA suggests: Save to Research > Competitive
      [✓ Accept] [→ Skip] [🗑 Delete]

      2/7: New Whisper model from OpenAI (OpenAI Blog)
      SARA suggests: Save to Research > AI Technology
      [✓ Accept] [→ Skip] [🗑 Delete]

      3/7: LinkedIn post format that went viral (LinkedIn)
      SARA suggests: Save to Content Inspiration
      [✓ Accept] [→ Skip] [🗑 Delete]
```

In quick mode, SARA’s suggested destination is the default action. One tap to accept, one to skip, one to delete. Process 7 items in under 2 minutes.

### 5.6 Review Summary

After processing all items:

```
SARA: "Inbox review complete ✓"
      ┌──────────────────────────────────┐
      │ 📊 Session Summary               │
      │                                  │
      │ Processed: 7 items               │
      │ Saved to Research: 3             │
      │ Shared with team: 1              │
      │ Action items created: 1          │
      │ Archived: 1                      │
      │ Deferred to tomorrow: 1          │
      │                                  │
      │ Inbox: 0 items remaining         │
      └──────────────────────────────────┘
```

-----

## 6. Beyond Links — Universal Inbox

The inbox is designed for links, but it naturally extends to other capture types:

|Capture Type       |How It Arrives                   |SARA Processing                                |
|-------------------|---------------------------------|-----------------------------------------------|
|**URL/Link**       |Share sheet, paste, slash command|Fetch, extract, classify, summarize            |
|**Voice Memo**     |Voice message in Rocket.Chat     |Transcribe, extract intent, classify           |
|**Screenshot**     |Image shared to SARA             |OCR, extract text/URLs, classify               |
|**Forwarded Email**|Forward to SARA’s email (future) |Parse, extract links and content, classify     |
|**Quick Note**     |Text message to inbox            |Store as-is, classify by content               |
|**File**           |Document shared to SARA          |Parse via Docling, extract key points, classify|

All types flow through the same GTD review process. The inbox is type-agnostic — SARA handles classification and routing regardless of input format.

-----

## 7. Periodic Inbox Intelligence

Beyond individual reviews, SARA generates insights from inbox patterns:

### 7.1 Weekly Inbox Digest

Included in the Sunday weekly digest:

```
SARA: "This week's inbox activity:"

      📥 Captured: 23 items
      ✅ Processed: 19 items
      📂 Top categories:
         - Competitive Intel (8)
         - AI Research (6)
         - Funding News (5)
      ⏳ Still pending: 4 items
      
      🔍 Pattern: You've saved 12 articles about 
         AI in education this month. Want me to 
         synthesize a research brief?
         
      [Yes, create brief]  [No thanks]
```

### 7.2 Auto-Synthesis

When SARA detects enough related captures (5+ on the same topic), she proactively offers to create a research synthesis:

```
SARA: "I notice you've saved 6 articles about MENA edtech 
       funding in the past 2 weeks. I can create a funding 
       landscape brief with key rounds, investors, and trends.
       
       Should I?"
       
       [Yes, create brief]  [Add to Research Queue]  [Not now]
```

### 7.3 Stale Item Nudges

Part of the heartbeat check:

```
SARA: "You have 3 inbox items older than 5 days:
       
       1. AI coaching research paper (5 days)
       2. Egyptian school procurement process (6 days)  
       3. Notion alternative comparison (7 days — auto-archive tomorrow)
       
       [Review now]  [Archive all]  [Extend 3 days]"
```

-----

## 8. Data Architecture

### 8.1 Outline Collection Structure

```
📥 Inbox (Outline collection — per user, hidden from advisors)
├── Mr Be
│   ├── [Unprocessed items as individual docs]
│   ├── [Deferred items]
│   └── Archive/
│       └── [Processed items kept for searchability]
├── Nermeen
│   └── ...
├── Shereen
│   └── ...
└── Kelly
    └── ...
```

Each inbox item is a structured Outline document with the metadata from Section 4.3. This means the full power of Outline search applies — SARA can search across all captured links when answering questions or generating research.

### 8.2 Database Schema (PostgreSQL)

```sql
CREATE TABLE inbox_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id VARCHAR(50) NOT NULL,
    url TEXT,
    title TEXT,
    source VARCHAR(200),
    author VARCHAR(200),
    published_at TIMESTAMP,
    captured_at TIMESTAMP NOT NULL DEFAULT NOW(),
    user_note TEXT,
    language VARCHAR(10) DEFAULT 'en',
    categories TEXT[] DEFAULT '{}',
    tags TEXT[] DEFAULT '{}',
    status VARCHAR(20) DEFAULT 'unprocessed',
    -- unprocessed, deferred, processing, processed, archived, deleted
    summary TEXT,
    sara_analysis TEXT,
    relevance_score FLOAT,
    suggested_destination TEXT,
    suggested_actions TEXT[],
    processed_at TIMESTAMP,
    processed_action VARCHAR(50),
    destination_doc_id VARCHAR(100),
    outline_doc_id VARCHAR(100),
    raw_content TEXT,
    content_type VARCHAR(20) DEFAULT 'link',
    -- link, voice_memo, screenshot, email, note, file
    review_count INT DEFAULT 0,
    deferred_until TIMESTAMP
);

CREATE INDEX idx_inbox_user_status ON inbox_items(user_id, status);
CREATE INDEX idx_inbox_captured ON inbox_items(captured_at DESC);
CREATE INDEX idx_inbox_categories ON inbox_items USING GIN(categories);
```

### 8.3 Redis Cache

```
sara:inbox:{user_id}:count        → current unprocessed count
sara:inbox:{user_id}:last_review  → timestamp of last review session
sara:inbox:{user_id}:categories   → category distribution for patterns
```

-----

## 9. sara-api Endpoints

|Endpoint                   |Method|Purpose                                                 |
|---------------------------|------|--------------------------------------------------------|
|`/inbox/capture`           |POST  |Receive new link/content from Rocket.Chat webhook       |
|`/inbox/items`             |GET   |List inbox items (filterable by status, category, date) |
|`/inbox/items/{id}`        |GET   |Get single item with full content                       |
|`/inbox/items/{id}/process`|POST  |Process item (save, share, archive, delete, defer)      |
|`/inbox/review/start`      |POST  |Begin a review session — returns first unprocessed item |
|`/inbox/review/next`       |POST  |Get next item in review session                         |
|`/inbox/review/summary`    |GET   |Get summary of current review session                   |
|`/inbox/stats`             |GET   |Inbox analytics (counts, categories, patterns)          |
|`/inbox/synthesize`        |POST  |Trigger synthesis of related items into a research brief|

-----

## 10. Rocket.Chat Integration

### 10.1 Channels

|Channel/DM           |Purpose                                    |
|---------------------|-------------------------------------------|
|**DM with SARA**     |Personal inbox captures and review sessions|
|**#inbox** (optional)|Team-shared captures — visible to everyone |

### 10.2 Slash Commands

|Command                     |Action                                               |
|----------------------------|-----------------------------------------------------|
|`/sara save [URL] [note]`   |Capture a link with optional note                    |
|`/sara inbox`               |Start inbox review session                           |
|`/sara inbox count`         |Show unprocessed item count                          |
|`/sara inbox [category]`    |Review items in a specific category                  |
|`/sara inbox clear`         |Archive all unprocessed items                        |
|`/sara inbox search [query]`|Search across all captured items (including archived)|

### 10.3 Widgets

|Widget                |Type                                   |When It Appears                     |
|----------------------|---------------------------------------|------------------------------------|
|**Inbox Review Card** |Message with action buttons            |During review session — one per item|
|**Quick Review Strip**|Compact message with accept/skip/delete|When user picks quick mode          |
|**Save Confirmation** |Brief message with tags and destination|After capture                       |
|**Inbox Summary**     |Stats card                             |After review session ends           |
|**Synthesis Offer**   |Message with action buttons            |When SARA detects pattern           |

-----

## 11. Heartbeat Integration

Added to HEARTBEAT.md:

```markdown
## Inbox
- Any team member with >10 unprocessed inbox items? Nudge them.
- Any items older than 5 days? Send reminder with option to extend or archive.
- Any items hitting 7-day auto-archive threshold? Warn 24 hours before.
- Detect patterns: 5+ items on same topic → offer synthesis.
- At scheduled review times, initiate review sessions for each user.
```

-----

## 12. User Preferences (Users/{name}.md Addition)

```markdown
## Inbox Preferences
- Review time: [when to initiate daily review]
- Review frequency: [daily / every-other-day / manual-only]
- Max items per session: [cap to prevent overwhelm]
- Default review style: [detailed / quick]
- Auto-archive after: [days] unprocessed (with reminder on day [n])
- Auto-categorize: [on/off — let SARA classify automatically]
- Share captures to team: [never / ask / always for specific categories]
- Synthesis threshold: [number of related items before SARA offers to synthesize]
```

-----

## 13. Integration with Existing SARA Capabilities

The inbox connects naturally to SARA’s existing features:

|SARA Capability              |Inbox Connection                                                                             |
|-----------------------------|---------------------------------------------------------------------------------------------|
|**Research Processor (S6)**  |“Start Research” action sends topic to Research Queue — SARA does a deep dive                |
|**Daily Briefing (S4)**      |Briefing includes inbox status: “You have X items to review”                                 |
|**Weekly Digest (S5)**       |Digest includes inbox analytics and pattern detection                                        |
|**Meeting Prep (S11)**       |If an inbox item is about a person/company with an upcoming meeting, SARA surfaces it in prep|
|**Content Drafts (S17)**     |Content inspiration captures feed directly into LinkedIn post drafts                         |
|**CRM (P12)**                |Links about investors or partners auto-suggest updating their CRM doc                        |
|**Heartbeat (S3)**           |Stale inbox items trigger nudges                                                             |
|**Change Notifications (S8)**|When a saved link’s source publishes an update, SARA can notify                              |

-----

## 14. Privacy and Per-User Isolation

Each team member’s inbox is private by default:

- Inbox items are only visible to the person who captured them
- Review sessions happen in private DMs with SARA
- Advisors have no access to team inbox data
- The “Share with Team” action is an explicit, conscious choice
- SARA never cross-references one person’s inbox with another’s without permission

The optional #inbox channel is for intentionally shared captures — things the team collectively wants to track.

-----

## 15. Build Plan

|Phase      |What                                                                                        |Effort  |
|-----------|--------------------------------------------------------------------------------------------|--------|
|**Phase 1**|Basic capture (share link to SARA DM → save with metadata) + manual review via `/sara inbox`|2–3 days|
|**Phase 2**|AI classification, auto-tagging, suggested destinations                                     |1–2 days|
|**Phase 3**|Scheduled review sessions with Rocket.Chat widgets (review cards, action buttons)           |2–3 days|
|**Phase 4**|Quick review mode, inbox stats, heartbeat integration                                       |1–2 days|
|**Phase 5**|Pattern detection, auto-synthesis offers, weekly inbox digest                               |2–3 days|
|**Phase 6**|Universal inbox (voice memos, screenshots, emails, files)                                   |3–4 days|

**Total estimated effort: 12–18 days**

This can be built incrementally alongside the main SARA build phases. Phase 1 of the inbox aligns with SARA’s Phase 1 (Foundation) — as soon as Rocket.Chat and sara-api are running, the basic capture flow works.

-----

## 16. Success Metrics

How we know the inbox is working:

- **Capture rate:** Team saves 3+ links per day (up from losing most of them)
- **Processing rate:** 80%+ of inbox items processed within 48 hours
- **Zero loss:** Nothing interesting encountered by the team is ever lost again
- **Research acceleration:** Auto-synthesis produces at least 2 research briefs per month from captured links
- **Action conversion:** 20%+ of captured links result in a concrete action item or knowledge update

-----

*This feature was designed through dialogue between Mr Be and Claude, March 2026.*

*It extends the SARA capabilities inventory as S20: Link Inbox / GTD Processor.*

*The inbox is where SARA stops being a tool you query and becomes a system that organizes your information life.*