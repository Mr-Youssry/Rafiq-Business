# SARA
## Mobile & Desktop Experience — Rocket.Chat as the Unified Interface

*Architecture & Strategy Document*

**For:** RafiqAI Founding Team  
**Authors:** Mr Be + Claude  
**Date:** March 2026  
**Status:** Brainstorm Complete — Ready for Build Planning

---

## 1. The Vision

SARA Office is not a collection of tools — it is a single unified workspace where the RafiqAI founding team does everything: chat, design, write docs, manage tasks, plan, report, and research. The team should never need to open a browser, remember a URL, or manage multiple logins.

**The core principle:** Rocket.Chat becomes the front door to the entire SARA ecosystem. The team downloads the stock Rocket.Chat app from the App Store or Google Play, enters the server URL, logs in once, and has access to every SARA capability — on mobile, desktop, and web.

**The strategic goal:** Build a super app for startups that replaces the entire SaaS stack. One hosting (droplet), one domain, one LLM provider, one app. No more paying for Slack + Notion + Google Calendar + QuickBooks + Zoom + Figma + Mailchimp + ChatGPT + Zapier separately.

---

## 2. Why Rocket.Chat

### 2.1 What Rocket.Chat Gives Us for Free

- **App Store presence:** Official iOS and Android apps already published and approved. No fork, no app store submission, no review process. Team downloads it today.
- **Desktop app:** Official Electron-based desktop app for Windows, macOS, and Linux. Same experience across all platforms.
- **Self-hosted:** Runs on the same droplet as the rest of SARA. No SaaS dependency, no per-seat pricing.
- **Apps Engine:** A built-in framework for creating custom integrations that render interactive UI inside the chat — modals, contextual bars, buttons, forms. No fork required.
- **Bot framework:** Native support for bots with rich message formatting, attachments, and interactive elements.
- **SSO:** Keycloak OIDC integration out of the box. One login covers Rocket.Chat and all SARA services.
- **Push notifications:** Built-in push notification gateway for mobile. SARA can alert the team on their phones.
- **File sharing:** Upload, preview, and share files directly in chat. SARA can send generated PDFs, slides, and charts as attachments.
- **Threads:** Threaded conversations keep discussions organized without cluttering the main channel.
- **WebSocket API:** Real-time bidirectional communication for SARA bot responses.

### 2.2 What Rocket.Chat Replaces

| SaaS Tool | Monthly Cost (5 users) | Rocket.Chat Alternative |
|---|---|---|
| Slack Pro | $36/mo | Rocket.Chat (self-hosted, $0) |
| Microsoft Teams | $30/mo | Rocket.Chat (self-hosted, $0) |
| Discord (Nitro) | $50/mo | Rocket.Chat (self-hosted, $0) |

---

## 3. The Daily Experience

### 3.1 Channel Structure as Workspace

The Rocket.Chat channel structure becomes the team's organizational framework. Each channel serves a specific purpose, and SARA participates in all of them.

| Channel | Purpose | SARA's Role |
|---|---|---|
| **#general** | Team conversation | Listens, answers when mentioned |
| **#sara** | Primary SARA interaction channel | Responds to all messages |
| **#briefings** | Daily and weekly digests | Auto-posts briefings on schedule |
| **#research** | Research briefs and findings | Posts completed research |
| **#decisions** | Auto-logged from meetings | Extracts and logs decisions |
| **#alerts** | Overdue items, heartbeat flags | Proactive notifications |
| **DMs with SARA** | Private queries | Personal finance, tasks, preferences |

### 3.2 Two Tiers of Interaction

**Tier 1 — Chat-Native (80% of daily work):** Text responses, markdown formatting, images (charts as PNGs), file attachments (PDFs, slides), message buttons, and Apps Engine widgets. Everything happens inside Rocket.Chat without leaving the app.

**Tier 2 — Full UI (20% of work):** For deep editing (writing a long wiki doc, configuring calendar booking pages, reviewing detailed P&L), SARA sends a link that opens in Rocket.Chat's in-app browser. Keycloak session carries over, so no re-login is needed.

---

## 4. Widget Architecture — SARA-Triggered UI

This is the core innovation: SARA doesn't just respond with text — she launches the right UI widget at the right moment based on natural language understanding. The conversation is the command line; widgets are the GUI.

### 4.1 How It Works

1. Team member sends a natural language message to SARA in Rocket.Chat
2. Rocket.Chat bot webhook sends the message to sara-api
3. sara-api processes intent using the triage LLM profile
4. sara-api determines: does this need a widget or a text response?
5. If widget: sara-api calls the Rocket.Chat Apps Engine API to open a contextual bar or modal, pre-filled with data extracted from the user's message
6. User confirms or tweaks the pre-filled data, submits
7. Widget sends the action back to sara-api, which writes to the appropriate backend (Outline, Cal.com, Bigcapital, etc.)
8. SARA confirms completion with a chat message

### 4.2 The Key Principle: AI Pre-Fills Everything

SARA never opens a blank form. She parses the user's natural language, extracts the relevant data (who, what, when, where), and pre-populates the widget. The user just confirms or tweaks. This is fundamentally different from traditional tools where users navigate to a screen, find a button, and fill forms from scratch.

### 4.3 Widget Interaction Examples

**Add Task Widget**

> **You:** *"Sara let's add a task for Nermeen to finish the pilot proposal by Friday"*
>
> **SARA:** "Got it — here's the task details:"
>
> ```
> ┌──────────────────────────────┐
> │ ✅ New Task                  │
> │                              │
> │ Title: Finish pilot proposal │
> │ Assignee: Nermeen      [▾]  │
> │ Due: Friday Mar 13    [📅]  │
> │ Priority: Normal       [▾]  │
> │ Project: RafiqAI       [▾]  │
> │                              │
> │      [Cancel]  [Create Task] │
> └──────────────────────────────┘
> ```

**Book Meeting Widget**

> **You:** *"Sara schedule a call with Ahmed from Flat6Labs next week"*
>
> **SARA:** "I found Ahmed's contact. Here's the booking:"
>
> ```
> ┌──────────────────────────────┐
> │ 📅 New Meeting               │
> │                              │
> │ With: Ahmed (Flat6Labs)      │
> │ Type: Investor Call    [▾]  │
> │ Duration: 30 min       [▾]  │
> │ Suggested slots:             │
> │   ○ Mon Mar 9, 2:00 PM      │
> │   ○ Tue Mar 10, 11:00 AM    │
> │   ○ Wed Mar 11, 3:00 PM     │
> │                              │
> │      [Cancel]  [Send Invite] │
> └──────────────────────────────┘
> ```

**Save Link Widget**

> **You:** *"Sara save this link for the research on Egyptian edtech market"*
>
> **SARA:** "Where should I file this?"
>
> ```
> ┌──────────────────────────────┐
> │ 🔗 Save Link                 │
> │                              │
> │ URL: [paste or type]         │
> │ Collection: Research   [▾]  │
> │ Tags: edtech, egypt    [+]  │
> │ Note: [optional]             │
> │                              │
> │      [Cancel]  [Save Link]   │
> └──────────────────────────────┘
> ```

**Proactive Meeting Prep (SARA-Initiated)**

> **SARA:** "Morning Mr Be — you have a call with Flat6Labs in 2 hours. Here's your prep:"
>
> ```
> ┌──────────────────────────────┐
> │ 📋 Meeting Prep              │
> │                              │
> │ Ahmed — Managing Partner     │
> │ Last spoke: Feb 20           │
> │ Open items: term sheet review│
> │ Their focus: edtech, Series A│
> │                              │
> │ Key docs:                    │
> │   📄 RafiqAI One-Pager       │
> │   📄 Financial Summary Q1    │
> │                              │
> │  [Open Full Brief] [Dismiss] │
> └──────────────────────────────┘
> ```

---

## 5. Widget Library — Rollout Plan

Each widget is a Rocket.Chat App built on the Apps Engine. Widgets are added incrementally based on real team usage patterns. If the team keeps asking SARA to do something via text, that's the signal to build a widget for it.

| Phase | Widget | Backend Service | What It Does |
|---|---|---|---|
| **Phase 1** | Ask SARA | sara-api + LLM | Text Q&A, no widget needed |
| **Phase 1** | Add Task | Outline | Create task doc with assignee, due date, project |
| **Phase 1** | Book Meeting | Cal.com | Schedule meeting with suggested time slots |
| **Phase 1** | Save Link | Outline | Bookmark URL to a collection with tags |
| **Phase 1** | Log Expense | Bigcapital | Quick expense entry with category and amount |
| **Phase 2** | Draft Document | Outline + LLM | AI-drafted doc saved to wiki |
| **Phase 2** | Research Request | sara-api + Scrapling | Submit topic for SARA to research |
| **Phase 2** | Log Decision | Outline | Record a team decision with context |
| **Phase 2** | Submit Tension | Outline (governance) | Log a holacracy tension for processing |
| **Phase 3** | Model Runway | Bigcapital + pandas | What-if financial scenarios with chart output |
| **Phase 3** | Export Pitch Deck | Outline + python-pptx | Generate branded PPTX from wiki content |
| **Phase 3** | Fill Application | TheAgenticBrowser | Auto-fill accelerator/grant forms |
| **Phase 3** | Action Items Dashboard | Outline + Cal.com | View all open tasks across the team |
| **Phase 3** | Create OKR | Outline | Define objective and key results |

---

## 6. Technical Architecture

### 6.1 Message Flow

Every interaction follows a consistent pattern from the user's message through SARA's processing to the widget response.

```
Team member types message in Rocket.Chat
         │
         ▼
Rocket.Chat webhook → sara-api /rocket/message
         │
         ▼
sara-api intent classification (triage LLM profile)
         │
    ┌────┴────┐
    │ Text?  │───→ Respond with formatted message
    │ Widget?│───→ Call Rocket.Chat Apps Engine API
    │ Link?  │───→ Send link with in-app browser hint
    └────────┘
         │
         ▼
Widget opens in Rocket.Chat (modal or contextual bar)
Pre-filled with data SARA extracted from the message
         │
         ▼
User confirms → sara-api writes to backend service
         │
         ▼
SARA posts confirmation message in chat
```

### 6.2 Rocket.Chat Apps Engine Integration

Each widget is a Rocket.Chat App registered through the Apps Engine. Apps are installed via the Rocket.Chat admin panel — no app store submission required. They run server-side within the Rocket.Chat process.

- **Contextual Bars:** Slide-in panels from the right side of the screen. Best for displaying information (meeting prep, action items dashboard, finance snapshot).
- **Modals:** Popup dialogs with form fields. Best for data input (add task, book meeting, log expense, save link).
- **Action Buttons:** Buttons attached to messages. Best for quick actions (approve proposal, acknowledge alert, open full brief).
- **Slash Commands:** Typed commands as fallback. `/sara ask`, `/sara task`, `/sara meet`, `/sara expense`.

### 6.3 sara-api Endpoint Structure

| Endpoint | Purpose | Widget Triggered |
|---|---|---|
| `POST /rocket/message` | Receive incoming messages from Rocket.Chat | Intent classification → routes to widget or text |
| `POST /rocket/widget/task` | Handle task creation form submission | Add Task modal |
| `POST /rocket/widget/meeting` | Handle meeting booking submission | Book Meeting modal |
| `POST /rocket/widget/link` | Handle link saving submission | Save Link modal |
| `POST /rocket/widget/expense` | Handle expense logging submission | Log Expense modal |
| `GET /rocket/widget/prep/{id}` | Get meeting prep data for contextual bar | Meeting Prep contextual bar |
| `GET /rocket/widget/actions` | Get open action items for dashboard | Action Items contextual bar |

---

## 7. Desktop Experience — Electron Wrapper

For the desktop experience, the team uses the stock Rocket.Chat desktop app (Electron-based, available for Windows, macOS, and Linux). This provides the same chat-first experience as mobile with native desktop integration: system tray icon, desktop notifications, auto-start on login, and keyboard shortcuts.

For team members who need frequent access to the full UI of other services (Outline wiki editing, Bigcapital accounting, Cal.com configuration), a future enhancement is a custom Electron shell that wraps all SARA services with a sidebar tab bar — similar to Microsoft Teams. This would show Rocket.Chat as the primary tab, with Wiki, Calendar, Books, and other services as additional tabs using embedded webviews sharing a single Keycloak session.

**Phase 1 approach:** Use stock Rocket.Chat desktop app. Simple, proven, zero maintenance.

**Future enhancement:** Custom SARA Office desktop shell if the team needs faster switching between services than browser tabs provide.

---

## 8. Mobile-Specific Considerations

### 8.1 Push Notifications

SARA's proactive behaviors (daily briefings, overdue item alerts, meeting prep) are delivered as Rocket.Chat messages. The Rocket.Chat mobile app handles push notifications natively. This means SARA's nudges appear as phone notifications — just like Slack or Teams — without any custom notification infrastructure.

### 8.2 Offline Behavior

Rocket.Chat mobile has built-in offline support for cached messages. When the team member comes back online, SARA's messages are synced. Widget interactions require connectivity since they hit sara-api and backend services.

### 8.3 Voice Input on Mobile

The Rocket.Chat mobile app supports voice messages. Combined with SARA's natural language understanding, team members can speak their requests: record a voice message saying "Sara schedule a meeting with Ahmed next Tuesday" and SARA transcribes it (via Groq Whisper), understands the intent, and opens the Book Meeting widget with the details pre-filled.

---

## 9. Architecture Changes from Base SARA

Adopting Rocket.Chat as the primary interface introduces the following changes to the base SARA architecture documented in Sara_Architecture v5.0:

| Component | Before (v5.0) | After (with Rocket.Chat) |
|---|---|---|
| **Primary chat interface** | Open WebUI + Slack | Rocket.Chat |
| **SARA bot platform** | Slack Bolt (Python) | Rocket.Chat webhook + Apps Engine |
| **Mobile access** | None (browser only) | Rocket.Chat iOS & Android apps |
| **Desktop access** | Browser tabs | Rocket.Chat desktop app |
| **Slack dependency** | Required (SaaS) | Eliminated (self-hosted) |
| **Open WebUI role** | Primary chat UI | Optional power-user tool for deep AI conversations |
| **Interactive UI in chat** | Slack Block Kit (limited) | Rocket.Chat Apps Engine (modals, contextual bars, buttons) |
| **Widget ecosystem** | Not planned | Progressive widget library (see Section 5) |
| **SaaS eliminated** | Slack remains | Full self-hosted stack |

### 9.1 RAM Impact

Rocket.Chat adds approximately 600 MB RAM to the droplet. It shares MongoDB with Bigcapital (already deployed). The total RAM footprint with Rocket.Chat is approximately 5,080 MB — still within the 8 GB droplet headroom.

---

## 10. The Super App Vision

SARA Office, powered by Rocket.Chat as the unified interface, is positioned to become a super app for startups. The progression:

1. **Now:** Build SARA for the RafiqAI founding team. Prove every widget, every integration, every workflow.
2. **Then:** Roll out to EdVenture and Ready4VC teams. This forces multi-instance design and surfaces edge cases.
3. **Later:** Package SARA as a product. One droplet, one domain, one LLM provider — a startup eliminates its entire SaaS stack for ~$70/month.

### 10.1 What SARA Replaces

| SaaS Tool | Typical Cost (5 users) | SARA Replacement | SARA Cost |
|---|---|---|---|
| Slack | $36/mo | Rocket.Chat | $0 |
| Notion | $50/mo | Outline | $0 |
| Google Calendar | $36/mo | Cal.com | $0 |
| QuickBooks | $30/mo | Bigcapital | $0 |
| Zoom | $13/mo | Jitsi Meet | $0 |
| Figma | $60/mo | Penpot | $0 |
| Mailchimp | $20/mo | Listmonk | $0 |
| ChatGPT Team | $125/mo | SARA (OpenRouter) | $10–18/mo |
| Zapier | $30/mo | n8n | $0 |
| Otter.ai | $50/mo | Vexa + Groq Whisper | $0.50/mo |
| **TOTAL SaaS** | **~$450/mo** | **TOTAL SARA** | **~$62–70/mo** |

**Annual savings per startup:** ~$4,500–$5,000. That's runway. That's the difference between a 12-month and a 16-month runway for an early-stage startup.

---

## 11. Next Steps

1. Add Rocket.Chat to the SARA docker-compose.yml (P4 in capabilities inventory — promoted from plugin to core)
2. Configure Rocket.Chat SSO with Keycloak
3. Build SARA bot integration: webhook receiver in sara-api + message formatting
4. Build Phase 1 widgets: Add Task, Book Meeting, Save Link, Log Expense
5. Set up channel structure and onboard the team
6. Team downloads Rocket.Chat from App Store / Google Play, enters `team.office.rafiq-ai.com`
7. Iterate: observe what the team asks SARA for most, build widgets for those patterns

---

*This document was designed through dialogue between Mr Be and Claude, March 2026.*  
*It extends the SARA Architecture v5.0 blueprint with mobile and desktop strategy.*
