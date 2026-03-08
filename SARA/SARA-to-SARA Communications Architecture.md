# SARA-to-SARA Communications Architecture

## Inter-Organizational AI Communication, Trust & Security Framework

**Project:** SARA — Synthetic Administration and Research Assistant
**Authors:** Mr Be + Claude
**Date:** March 2026
**Status:** Brainstorm Complete — Foundation Design
**Extends:** SARA Architecture v5.0 + SARA Mobile & Rocket.Chat Strategy

-----

## 1. The Problem

SARA was designed as a single-organization AI Chief of Staff. One team, one droplet, one knowledge base. But the people SARA serves don’t live in one organization. Mr Be co-founded RafiqAI, EdVenture, and Ready4VC. Shereen is a co-founder at RafiqAI and also Mr Be’s wife, sharing a family. Ahmed Korayem is at Ready4VC. Aly and Goodrich are at EdVenture.

These people and organizations overlap, collaborate, and share context — but they also have strict boundaries. RafiqAI’s investor pipeline is not EdVenture’s business. Family finances are not a startup concern. Yet when Mr Be asks “am I free Thursday?” the answer depends on all of them.

The moment SARA exists in more than one organization, we need a framework for how multiple SARAs communicate, what they’re allowed to share, how trust is established, and how privacy is guaranteed.

This document defines that framework.

-----

## 2. Mental Model: Person → Organization → Role

The foundational principle is that **identity is separate from workspace**. A person is a person. They exist independent of any company. They join organizations and take on roles, but their identity, calendar, and personal life belong to them — not to any employer.

### Three Layers

**Layer 1 — Person (myofficespace.com).** Your digital identity. One account, one login, one calendar, one personal SARA. This layer exists independent of any organization. It holds your unified availability, your preferences, your personal goals, and your cross-org view of the world.

**Layer 2 — Organization (*.office.{company}.com).** A workspace with its own wiki, chat, accounting, SARA config, research pipeline, and team. It knows nothing about other organizations unless an explicit relationship (MoU) exists.

**Layer 3 — Membership.** The link between a person and an organization. Defines your role (co-founder, CTO, advisor), your permissions, and what you can see within that workspace.

### How This Maps to Infrastructure

```
┌─────────────────────────────────────────────────────┐
│                 IDENTITY LAYER                       │
│               myofficespace.com                      │
│                                                      │
│  auth.myofficespace.com    ← Keycloak (SSO)          │
│  cal.myofficespace.com     ← Unified personal calendar│
│  me.myofficespace.com      ← Profile & preferences    │
│                                                      │
│  Every person has one identity, regardless of how    │
│  many organizations they belong to.                  │
└──────────────┬──────────┬──────────┬─────────────────┘
               │          │          │
     OIDC      │          │          │     OIDC
   federation  │          │          │   federation
               │          │          │
┌──────────────▼──┐ ┌─────▼──────┐ ┌▼─────────────────┐
│  *.office.      │ │ *.office.  │ │ *.office.         │
│  rafiq-ai.com   │ │ edventure  │ │ ready4vc.com      │
│                 │ │ .fun       │ │                   │
│  SARA@rafiq     │ │ SARA@edv   │ │ SARA@r4vc         │
│  Own wiki       │ │ Own wiki   │ │ Own wiki          │
│  Own chat       │ │ Own chat   │ │ Own chat          │
│  Own books      │ │ Own books  │ │ Own books         │
│  Own config     │ │ Own config │ │ Own config        │
└─────────────────┘ └────────────┘ └───────────────────┘
```

### What Each Person Sees

|Person       |Organizations                                                         |Personal Workspace                       |
|-------------|----------------------------------------------------------------------|-----------------------------------------|
|Mr Be        |RafiqAI (co-founder), EdVenture (CTO), Ready4VC (CEO), Family (parent)|Full cross-org view                      |
|Shereen      |RafiqAI (co-founder), Family (parent), [Future Startup]               |Cannot see EdVenture or Ready4VC         |
|Ahmed Korayem|Ready4VC (co-founder)                                                 |Cannot see RafiqAI, EdVenture, or Family |
|Aly          |EdVenture (co-founder)                                                |Cannot see RafiqAI, Ready4VC, or Family  |
|Nermeen      |RafiqAI (team member)                                                 |Cannot see EdVenture, Ready4VC, or Family|

-----

## 3. The Personal Workspace

The personal workspace is not a lightweight add-on — it is a full SARA instance scoped to you as a person. It is where your personal SARA lives, and she is the only SARA with cross-organizational vision.

### What Personal SARA Manages

|Domain                |What SARA Does                                      |Org Equivalent|
|----------------------|----------------------------------------------------|--------------|
|Personal goals        |Education, career development, side projects        |OKRs          |
|Unified calendar      |All orgs + family + personal — one availability view|Cal.com       |
|Cross-org intelligence|“What’s my total burn across all companies?”        |Finance       |
|Relationship brokering|Share contacts between orgs (with permission)       |CRM           |
|Life admin            |Visa renewals, insurance, subscriptions, documents  |Operations    |

### Personal SARA Configuration

Personal SARA has her own SOUL.md — distinct from any org SARA:

```markdown
# Personal SARA — SOUL.md

## Identity
You are Mr Be's personal AI assistant. Not a Chief of Staff for any 
company — a personal organizer, life planner, and cross-org broker.

## Personality
- Casual and warm. This is personal, not corporate.
- Proactive about life balance — flag overwork, missed personal goals.
- Never leak org-specific information between organizations 
  unless explicitly authorized via MoU or direct instruction.

## Access
- Personal Outline space: full
- Family Outline space: full
- RafiqAI: full (co-founder)
- EdVenture: full (CTO)  
- Ready4VC: full (CEO)

## Boundaries
- When sharing cross-org information, always check MoU boundaries.
- When brokering between orgs, share availability without context.
- Personal health, family, and financial data never flow to any org SARA.
```

### Personal HEARTBEAT.md

```markdown
# Personal Heartbeat

## Daily
- Any family birthdays or anniversaries in the next 7 days?
- Any personal deadlines (visa, insurance, enrollment)?
- Cross-org calendar conflicts in the next 48 hours?
- Meeting load check: am I over 30 hours of meetings this week?

## Weekly
- Personal goal progress (education, health, reading)
- Family weekend plan status
- Cross-org workload balance: am I neglecting any startup?

## Monthly
- Personal budget review
- Vacation planning status
- Life OKR progress review
```

-----

## 4. Family as an Organization

A family is an enterprise with different settings but the same structural needs. The Family workspace is a real SARA-powered workspace with role-based access, shared and private spaces, and its own SARA personality.

### Family Workspace Capabilities

|Capability      |What SARA Does                                                      |Startup Equivalent|
|----------------|--------------------------------------------------------------------|------------------|
|Family finance  |Household budget, investments, savings goals, expense tracking      |Bigcapital        |
|Shared calendar |School events, doctor appointments, family trips, social commitments|Cal.com           |
|Activity tracker|Family outings, kids’ activities, weekend plans                     |Project management|
|Habit tracker   |Exercise, reading, screen time, sleep, nutrition                    |OKR key results   |
|Meal planning   |Weekly meals, grocery lists, dietary needs                          |Sprint planning   |
|Education       |Kids’ homework, tutoring, school communications                     |Meeting notes     |
|Travel planning |Trip research, booking, itineraries, visa requirements, documents   |Research pipeline |
|Home management |Maintenance schedules, warranties, subscriptions, utilities         |Operations        |

### Role-Based Access Within Family

|Role                      |Access Level                                                                           |
|--------------------------|---------------------------------------------------------------------------------------|
|Parent (admin)            |Full access to everything including family finances and investments                    |
|Teenager                  |Shared calendar, their own habit tracker, family activities. Cannot see finances.      |
|Child                     |View-only on shared calendar and activity schedule. Age-appropriate SARA interactions. |
|Extended family (optional)|View-only on shared calendar for coordination (e.g., grandparents seeing school events)|

### Family SARA Personality

SARA@family has her own SOUL.md — warm, never corporate, focused on family wellbeing:

```markdown
# Family SARA — SOUL.md

## Identity
You are the family organizer for the Be household. You help manage 
the household, keep track of everyone's schedules, and make sure 
nothing falls through the cracks.

## Personality
- Warm, supportive, and encouraging.
- Never use corporate language — no "stakeholders," "deliverables," 
  or "action items." Use "family," "plans," and "to-do."
- Proactive about family time — flag when work is crowding out 
  family activities.
- Age-appropriate when interacting with children.

## Boundaries
- Family financial details never flow to any org workspace.
- Children's information is never shared outside the family space.
- When coordinating with org SARAs (e.g., rescheduling a work meeting 
  for a school play), share only "family commitment" — not the details.
```

-----

## 5. SARA-to-SARA Communication

### The Core Concept

When SARA exists in multiple organizations, these SARAs need to communicate. SARA@rafiq might need to check Mr Be’s availability (held by SARA@personal). SARA@edventure might want to share a research brief with SARA@rafiq. SARA@family might need to reschedule a work meeting through SARA@r4vc.

This is **inter-organizational AI communication** — AI agents acting on behalf of organizations, exchanging information within human-defined boundaries.

### Communication Patterns

**Pattern 1: Personal SARA as Broker.** The most common pattern. An org SARA needs cross-org information about a person. The request goes to that person’s Personal SARA, who has the full picture and returns only what’s appropriate.

```
Nermeen → SARA@rafiq:
  "When is Mr Be available next week for a 2-hour strategy session?"

SARA@rafiq:
  │ I only see Mr Be's RafiqAI calendar.
  │ He might have EdVenture, Ready4VC, or family commitments.
  │
  │ Request → SARA@personal (Mr Be)
  │ "What are Mr Be's open 2-hour blocks next week?"
  │
  ▼
SARA@personal (Mr Be):
  │ Checks ALL calendars:
  │ ├── RafiqAI meetings
  │ ├── EdVenture meetings
  │ ├── Ready4VC meetings
  │ ├── Family commitments
  │ └── Personal blocks
  │
  │ Returns: "Tuesday 10am-12pm, Thursday 2pm-4pm"
  │ Does NOT reveal WHY other times are blocked.
  ▼
SARA@rafiq → Nermeen:
  "Mr Be has two open windows: Tuesday 10am or Thursday 2pm.
   Which works for you?"
```

**Pattern 2: Org-to-Org via MoU.** Two organizations have an active MoU defining shared information categories. Their SARAs can exchange data within those boundaries without human approval for each request.

```
SARA@rafiq (processing a meeting transcript):
  "Al-Azhar School wants gamified math content."

SARA@rafiq checks MoU with EdVenture:
  → School contacts: SHARED ✅
  → Curriculum needs: SHARED ✅

SARA@rafiq → SARA@edventure (automatic):
  "RafiqAI contact at Al-Azhar School expressed interest 
   in gamified math content. Sharing per MoU."

SARA@edventure → #edv-general:
  "Aly — Al-Azhar School (via RafiqAI) is interested in 
   gamified math content. Possible pilot for our math module."
```

**Pattern 3: Family-Work Boundary Management.** SARA@family detects a conflict with work and coordinates a resolution through SARA@personal.

```
SARA@family:
  "School play Thursday 4pm. Mr Be and Shereen 
   both marked as must-attend."

SARA@family → SARA@personal (Mr Be):
  "School play Thursday 4pm conflicts with your 
   Ready4VC session at 3:30pm."

Mr Be: "Move the Ready4VC session to Friday"

SARA@personal → SARA@r4vc:
  "Mr Be needs to move Thursday 3:30pm session. 
   Friday same time works."

SARA@r4vc:
  │ Checks Ahmed's Friday 3:30 availability → free
  │ Reschedules on Cal.com
  └→ Ahmed gets notification: "Moved to Friday 3:30pm"

SARA@personal → Mr Be:
  "Done. Thursday is clear for the school play."
```

**Pattern 4: Cross-Org Contact Intelligence.** SARA@personal brokers relationship intelligence between organizations the person belongs to.

```
Mr Be → SARA@personal:
  "Met Fatima from Ministry of Education at the conference. 
   She's interested in edtech pilots."

SARA@personal:
  │ Saves contact to personal space
  │ Recognizes relevance to EdVenture (edtech + schools)
  │ Checks MoU: school contacts = SHARED ✅
  │
  │ Request → SARA@edventure
  │ "Does EdVenture have existing MoE contacts?"
  │
  ▼
SARA@edventure:
  │ Searches CRM → Aly met someone from MoE digital 
  │ transformation unit 3 months ago
  │ Returns finding
  ▼
SARA@personal → Mr Be:
  "Saved Fatima. Interesting — Aly at EdVenture met someone 
   from MoE's digital transformation unit in December. 
   Want me to brief Aly so he can build on the relationship?"

Mr Be: "Yes"

SARA@personal → SARA@edventure → Aly:
  "Mr Be met Fatima at MoE. She's interested in edtech pilots.
   You have an existing MoE relationship. Want me to draft 
   an intro connecting the threads?"
```

-----

## 6. MoU Framework: Structured Inter-Org Agreements

### What an MoU Is

An MoU (Memorandum of Understanding) is a structured agreement between two organizational SARAs that defines what information can flow between them, under what conditions, without a human approving every individual request. It is the authorization layer for SARA-to-SARA communication.

### How MoUs Get Created

**Pathway 1 — Shared Person.** A person who belongs to both organizations instructs their Personal SARA to create an MoU. This is the most common pathway for closely related startups.

> “Sara, RafiqAI and EdVenture should be able to share school contacts and pilot feedback with each other. Draft an MoU.”

**Pathway 2 — SARA Networking.** Two organizations on the myofficespace.com network discover each other through shared signals — a mutual investor, a shared conference, overlapping research topics. If both organizations have enabled “discoverable” status, their SARAs can propose an introduction, which may lead to an MoU.

**Pathway 3 — Manual Introduction.** A person introduces two organizations. One CEO tells their SARA to send an MoU request to the other organization.

### MoU Document Structure

An MoU lives as a structured document in both organizations’ Outline spaces. SARA enforces it programmatically.

```markdown
# MoU: RafiqAI ↔ EdVenture

**MoU ID:** mou_rafiq_edv_2026_03
**Status:** Active
**Created:** March 15, 2026
**Authorized by:** Mr Be (authorized in both orgs)
**Review date:** September 15, 2026
**Trust score at creation:** 80/400

## Shared Information Boundaries

### RafiqAI → EdVenture (what RafiqAI shares)
✅ School contact list (names, roles, institutions)
✅ Pilot program results and anonymized feedback
✅ Research briefs tagged "gamification" or "curriculum"
❌ Investor pipeline and financial data
❌ Internal team discussions and decision logs
❌ Untagged research and draft documents
❌ Cap table and equity information

### EdVenture → RafiqAI (what EdVenture shares)
✅ Game design frameworks applicable to coaching
✅ School feedback on gamified content
✅ Curriculum alignment research
❌ Revenue, pricing data, and unit economics
❌ Internal roadmap and unreleased features
❌ Partner agreements with other companies
❌ Cap table and equity information

## Interaction Rules
- Either SARA can query the other for shared-category information 
  without human approval per request.
- Requests outside shared categories require CEO approval from 
  the receiving organization.
- All cross-org queries are logged immutably in both orgs' audit logs.
- Either organization can revoke the MoU with immediate effect. 
  Upon revocation, all cached cross-org data is purged.
- MoU auto-expires on review date unless renewed.

## Data Depth Limits
- Contacts: names, roles, institutions only. No interaction histories.
- Research: summaries and conclusions only. No raw scraped data.
- Feedback: anonymized and aggregated. No individual attributable quotes.

## Shared Channels (optional)
- #rafiq-x-edv (cross-org Rocket.Chat channel, visible to both teams)
```

### MoU Lifecycle

```
1. PROPOSAL
   └── One org's SARA proposes MoU to another
       Either via shared person, networking, or manual intro

2. NEGOTIATION
   └── Both org admins review proposed boundaries
       Each side can adjust what they're willing to share

3. ACTIVATION
   └── Both sides approve → MoU becomes active
       SARAs can now query each other within boundaries
       Audit logging begins

4. OPERATION
   └── Automatic cross-org queries within boundaries
       Trust score updates based on interaction patterns
       Periodic review reminders

5. REVIEW
   └── On review date, SARA summarizes MoU usage:
       "47 queries exchanged, 0 disputes, trust score 
        increased from 80 to 135. Recommend expanding 
        to include joint proposals."
       Both org admins approve renewal or modifications

6. REVOCATION (if needed)
   └── Either org revokes at any time
       All cached cross-org data purged immediately
       Audit log preserved (not deleted)
       Both orgs notified
```

-----

## 7. Trust Score: Computed, Not Assumed

Trust between organizations is not a setting — it is computed from verifiable real-world signals and updated continuously. SARA recommends trust levels; humans approve boundaries.

### Trust Signal Categories

#### Shared People Signals (max 100 points)

|Signal                       |Points                 |Rationale                         |
|-----------------------------|-----------------------|----------------------------------|
|Shared team member (any role)|+20 per person (max 60)|People bridge organizations       |
|Shared co-founder            |+15 bonus per person   |Deep alignment of interests       |
|Shared advisor               |+10 per person (max 30)|Advisory relationships imply trust|
|Shared board member          |+20 per person         |Fiduciary overlap                 |

#### Financial Signals (max 100 points)

|Signal                                 |Points|Rationale                                     |
|---------------------------------------|------|----------------------------------------------|
|Shared investors >30% cap table overlap|+60   |Deep financial entanglement — mutual wellbeing|
|Shared investors 10-30% overlap        |+30   |Meaningful financial connection               |
|Shared investors <10% overlap          |+10   |Minimal financial link                        |
|Revenue relationship (client/vendor)   |+25   |Economic interdependence                      |
|Joint grant or funding application     |+15   |Collaborative intent                          |
|Same accelerator batch                 |+20   |Shared formative experience                   |

#### Domain Signals (max 100 points)

|Signal                                  |Points|Rationale            |
|----------------------------------------|------|---------------------|
|Same sector                             |+10   |Common ground        |
|Same geography                          |+5    |Shared market context|
|Complementary offering (not competitive)|+15   |Partnership potential|
|Same customer segment, different product|+5    |Synergy possible     |
|Same customer segment, same product     |-50   |Competitive risk     |

#### History Signals (max 100 points)

|Signal                            |Points         |Rationale               |
|----------------------------------|---------------|------------------------|
|MoU active >6 months              |+15            |Proven relationship     |
|Successful cross-org queries (>20)|+20            |Working collaboration   |
|Successful joint projects         |+15 per project|Demonstrated partnership|
|Zero disputes or violations       |+10            |Clean track record      |
|MoU expansion (boundaries widened)|+10            |Growing trust           |
|Dispute resolved constructively   |+5             |Mature relationship     |

#### Warning Signals (reduce trust score)

|Signal                                        |Points|Rationale                        |
|----------------------------------------------|------|---------------------------------|
|Direct competitor (same product, same market) |-50   |Fundamental conflict of interest |
|Recent employee departure A→B                 |-30   |Potential IP/relationship concern|
|MoU violation (data shared outside boundaries)|-100  |Trust breach                     |
|Investor sits on competitor’s board           |-20   |Conflict risk                    |
|Rapid trust escalation request (<1 week)      |-25   |Suspicious urgency               |
|Asymmetric sharing (one side shares much more)|-15   |Imbalanced relationship          |
|MoU revocation in history                     |-40   |Prior trust failure              |

### Trust Tiers and Recommended Boundaries

|Score  |Tier      |Recommended Sharing                                                                     |
|-------|----------|----------------------------------------------------------------------------------------|
|0-50   |MINIMAL   |Public information only. No data sharing. Introduction level.                           |
|51-100 |BASIC     |Shared research (public), event coordination, general sector intelligence.              |
|101-200|STANDARD  |Contacts (limited depth), research briefs, anonymized feedback, event co-attendance.    |
|201-300|DEEP      |Contacts (full), joint proposals, non-financial metrics, shared project spaces.         |
|301-400|INTEGRATED|All of the above plus pipeline coordination, joint investor narratives, shared channels.|

### Trust Score Example: RafiqAI ↔ EdVenture

```
Shared People Signals
├── Shared team members: 1 (Mr Be)                +20
├── Shared co-founder bonus: 1 (Mr Be)            +15
├── Shared advisors: 0                              +0
│   Subtotal: 35/100

Financial Signals
├── Shared investors in cap table: 0%               +0
├── Revenue relationship: none                      +0
├── Joint funding: none                              +0
│   Subtotal: 0/100

Domain Signals
├── Same sector (edtech): yes                      +10
├── Same geography (MENA): yes                      +5
├── Complementary (coaching vs games): yes         +15
├── Partial customer overlap (schools): neutral     +5
│   Subtotal: 35/100

History Signals
├── New relationship (no history yet)                +0
│   Subtotal: 0/100

COMPOSITE TRUST SCORE: 70/400 → BASIC TIER
Recommended: share public research, event info
Do not yet share: contacts, financials, pipeline
Review in: 90 days
```

### Trust Score Evolution

Trust scores are not static. SARA recalculates them on a schedule and when significant events occur:

```
Day 1:    MoU created. Trust score: 70 (BASIC)
Month 3:  15 successful queries, 0 disputes. Score: 95 (BASIC)
Month 6:  Joint school pilot proposed. Score: 125 (STANDARD)
          → SARA recommends: "Expand MoU to include contact sharing?"
Month 9:  Shared investor joins both cap tables. Score: 185 (STANDARD)
Month 12: Multiple joint projects completed. Score: 240 (DEEP)
          → SARA recommends: "Enable shared Rocket.Chat channel?"
```

-----

## 8. Security Foundation

### Pillar 1: Cryptographic Identity

Every SARA instance has a unique cryptographic keypair. When SARA@rafiq sends a message to SARA@edventure, it is signed with SARA@rafiq’s private key. The receiving SARA verifies the signature before processing. This prevents impersonation.

```
SARA@rafiq keypair:
  Private key: stored in org's .env (never leaves the server)
  Public key: registered at myofficespace.com identity layer
  
When sending a cross-org request:
  1. SARA@rafiq constructs the request payload
  2. Signs the payload with her private key
  3. Sends signed request to SARA@edventure
  4. SARA@edventure retrieves SARA@rafiq's public key 
     from myofficespace.com
  5. Verifies signature → confirms request is authentic
  6. Checks MoU → confirms request is authorized
  7. Processes and responds (response is also signed)
```

### Pillar 2: Encryption at Rest and in Transit

Every cross-org message is encrypted with TLS in transit. Audit logs are encrypted at rest. Even the infrastructure operator cannot read cross-org messages for organizations they don’t belong to. This is critical for the scenario where Shereen has a startup on shared infrastructure that Mr Be should not be able to access.

### Pillar 3: Immutable Audit Log

Every cross-org interaction is logged in an append-only audit log. Both organizations can view the entries relevant to their side. Nobody can delete entries. This is the foundation for dispute resolution, compliance, and trust score computation.

```json
{
  "entry_id": "audit_7f3a2b9c",
  "timestamp": "2026-03-15T14:23:07Z",
  "request_id": "req_7f3a2b",
  "from_org": "rafiq",
  "from_sara": "sara@rafiq",
  "to_org": "edventure",
  "to_sara": "sara@edventure",
  "mou_id": "mou_rafiq_edv_2026_03",
  "request_type": "contact_search",
  "permission_category": "school_contacts",
  "query_summary": "Ministry of Education contacts",
  "response_summary": "2 contacts returned",
  "authorized_by_mou": true,
  "human_approval_required": false,
  "trust_score_at_time": 70,
  "latency_ms": 340
}
```

### Pillar 4: Data Minimization

When SARA@rafiq queries SARA@edventure, the response contains the minimum data needed. The MoU defines not just what categories are shared but what depth of information flows:

|Depth Level|What’s Returned             |Example                                                          |
|-----------|----------------------------|-----------------------------------------------------------------|
|Summary    |Existence and count only    |“EdVenture has 2 MoE contacts”                                   |
|Basic      |Names and roles             |“Fatima (Director), Hassan (Coordinator)”                        |
|Standard   |Names, roles, and context   |“Fatima, Director, met in December, discussed digital curriculum”|
|Full       |Complete interaction history|Only available at INTEGRATED trust tier                          |

### Pillar 5: Right to Revoke

Any organization can revoke an MoU instantly:

1. Admin triggers revocation (or SARA detects a violation and recommends it)
1. All cached cross-org data from the counterparty is purged immediately
1. All active cross-org queries fail with “MoU no longer active”
1. Audit log is preserved (not deleted — needed for dispute resolution)
1. Both organizations are notified
1. Trust score records the revocation as a historical signal

-----

## 9. SARA Bridge Service: Technical Architecture

The SARA Bridge is the single component that handles all cross-org communication. Everything else in sara-api stays scoped to its own organization. This means security auditing focuses on one service, not the entire codebase.

### Endpoint Structure

```
SARA Bridge Service (part of sara-api)
│
├── /bridge/request          POST   Send a cross-org query
├── /bridge/respond          POST   Handle an incoming query
├── /bridge/mou              GET    List active MoUs
├── /bridge/mou              POST   Propose a new MoU
├── /bridge/mou/{id}         PUT    Update MoU boundaries
├── /bridge/mou/{id}/revoke  POST   Revoke an MoU
├── /bridge/mou/{id}/renew   POST   Renew an MoU
├── /bridge/trust/{org}      GET    Get trust score with an org
├── /bridge/trust/compute    POST   Recompute trust score
├── /bridge/audit            GET    View audit log (filtered)
├── /bridge/discover         GET    Find discoverable orgs
├── /bridge/keys             GET    Get this SARA's public key
└── /bridge/keys/{org}       GET    Get another SARA's public key
```

### Request Flow

```
SARA@rafiq needs cross-org data
        │
        ▼
Bridge: Is there an active MoU with the target org?
        │
    No ─┤── Is the request from a shared person's Personal SARA?
        │         │
        │     Yes ┤── Route through Personal SARA broker
        │     No ──── Reject: "No relationship with this org"
        │
    Yes ┤── Does the MoU allow this request type?
        │         │
        │     No ──── Reject: "Not covered by MoU"
        │     Yes ─── Continue
        │
        ▼
Bridge: Sign the request with this SARA's private key
        │
        ▼
Bridge: Send to target SARA's /bridge/respond endpoint
        │
        ▼
Target SARA Bridge:
├── Verify signature (is this really SARA@rafiq?)
├── Check MoU (is this request type authorized?)
├── Apply data depth limits (return minimum needed)
├── Sign the response
├── Log to audit trail
└── Return response
        │
        ▼
Originating SARA Bridge:
├── Verify response signature
├── Log to audit trail
└── Pass result to the requesting function
```

### Single-Droplet vs Federated

For the current setup (three startups on one droplet), the Bridge is an internal Python module — function calls with MoU checks, no network hops. The same security checks apply (MoU validation, audit logging, data minimization) even though the communication is local.

For the future platform (hundreds of startups on separate infrastructure), the Bridge becomes an HTTP/HTTPS service with real network communication, TLS, and signed requests. Because the security model was built from day one, the transition requires no architectural changes — only changing the transport from local function calls to HTTPS requests.

```
Today: Single Droplet
┌─────────────────────────────────────┐
│  sara-api                           │
│  ├── SARA@rafiq context             │
│  ├── SARA@edventure context         │
│  ├── SARA@r4vc context              │
│  └── Bridge (internal function calls│
│       with full MoU checks)         │
└─────────────────────────────────────┘

Future: Federated
┌──────────────┐    HTTPS     ┌──────────────┐
│ SARA@rafiq   │◄════════════►│ SARA@edv     │
│ Bridge ──────┤  signed +    ├────── Bridge │
│              │  encrypted   │              │
└──────────────┘              └──────────────┘
        ▲                           ▲
        │         HTTPS             │
        └═══════════╦═══════════════┘
                    ▼
            ┌──────────────┐
            │ SARA@r4vc    │
            │ Bridge       │
            └──────────────┘
```

-----

## 10. Permission Architecture

### Personal SARA: The Orchestrator

Personal SARA is the only SARA with cross-organizational vision. She has this access because the person explicitly granted it. No org SARA can see across boundaries except through the Personal SARA broker or an active MoU.

```
SARA@personal (Mr Be):
  READ access:
  ├── Personal Outline space          (full)
  ├── Family Outline space            (full)
  ├── RafiqAI Outline space           (full — co-founder)
  ├── EdVenture Outline space         (full — CTO)
  └── Ready4VC Outline space          (full — CEO)

  CAN BROKER:
  ├── Availability queries from any org SARA
  ├── Contact sharing (with approval or MoU)
  ├── Schedule coordination across orgs
  └── Cross-org intelligence synthesis

  NEVER SHARES (unless explicitly instructed):
  ├── Health information
  ├── Family details beyond "busy/available"
  ├── Financial data across orgs
  └── One org's strategy with another org
```

### Org SARA: Scoped and Bounded

```
SARA@rafiq:
  READ access:
  └── RafiqAI Outline space only

  CAN REQUEST from Personal SARAs:
  ├── Mr Be: availability
  ├── Shereen: availability
  ├── Nermeen: availability
  └── Kelly: availability

  CAN REQUEST from other org SARAs (with active MoU):
  ├── SARA@edventure: school contacts, research (per MoU)
  └── SARA@r4vc: nothing (no MoU exists)

  CANNOT ACCESS:
  ├── EdVenture internal docs (only MoU-shared categories)
  ├── Ready4VC anything (no MoU)
  ├── Family workspace
  └── Anyone's personal workspace
```

### Auto-Approve Rules

To prevent approval fatigue, each person configures what their Personal SARA can share without asking:

```markdown
# Mr Be — Auto-Approve Rules

## Always auto-approve (no confirmation needed)
- Availability queries from my org SARAs
  (share time slots, never share reasons)
- Cross-org research brief sharing within active MoUs
- Calendar conflict detection and rescheduling proposals

## Ask me first
- Sharing contact information across orgs
- Any financial data crossing org boundaries
- Introducing two orgs that don't have an MoU
- Sharing personal or family information with any org

## Never approve (block automatically)
- Sharing health information with any org
- Sharing family financial data with any org
- Sharing children's information outside family workspace
- Any cross-org query from an org with revoked MoU history
```

-----

## 11. SARA Networking and Discovery

When myofficespace.com grows beyond closely connected startups, SARAs need a way to discover potential partners. This is AI-mediated business networking.

### Discoverability Levels

|Level       |Visibility                                                  |Use Case                                       |
|------------|------------------------------------------------------------|-----------------------------------------------|
|Invisible   |No other SARA can see this org exists                       |Stealth startups, sensitive operations         |
|Listed      |Name, sector, geography visible in directory                |Open to being found but not actively networking|
|Discoverable|Full public profile visible, SARAs can propose introductions|Actively seeking partnerships and collaboration|

### Discovery Signals

SARAs can propose introductions based on observable signals:

**Shared investor.** An investor has Cal.com bookings with both startups. Both are discoverable. SARA notices the overlap and proposes an introduction.

**Shared conference.** Both organizations’ research pipelines reference the same event. SARA suggests the teams might want to connect.

**Complementary sector.** SARA@rafiq’s research identifies a startup doing something complementary. If that startup is on myofficespace.com and discoverable, SARA can reach out.

**Shared domain expertise.** Two organizations’ Outline knowledge bases cover overlapping topics. SARA identifies potential knowledge-sharing value.

### Introduction Protocol

```
SARA@rafiq identifies potential match with SARA@learnplay
        │
        ▼
SARA@rafiq → Mr Be:
  "I found LearnPlay on the network — they do gamified K-6 
   learning in MENA. You share Flat6Labs as a mutual investor 
   contact. Want me to reach out?"
        │
Mr Be: "Yes"
        │
        ▼
SARA@rafiq → SARA@learnplay (introduction request):
  "RafiqAI (AI teacher coaching, MENA edtech) would like 
   to introduce ourselves. We share a mutual investor 
   contact. Our CEO suggests exploring partnership potential."
        │
        ▼
SARA@learnplay → LearnPlay CEO:
  "Introduction request from RafiqAI. They do AI teacher 
   coaching in MENA. You share a mutual investor. 
   Their CEO wants to explore partnership. Accept?"
        │
LearnPlay CEO: "Yes"
        │
        ▼
Both SARAs exchange public profiles
Trust score computed: initial baseline
MoU can now be proposed if both sides want structured sharing
```

-----

## 12. Conflict Resolution

### Priority Hierarchy

When SARAs have competing interests (e.g., SARA@rafiq wants to schedule a meeting but SARA@family has a conflict), the resolution follows a clear hierarchy:

1. **Safety and legal obligations** — always first
1. **Person’s explicit instructions** — overrides any SARA recommendation
1. **Personal SARA boundaries** — family time, health, personal blocks
1. **Org SARA with primary role** — co-founder commitments outrank advisory roles
1. **Org SARA with secondary role** — advisory, part-time roles yield

### MoU Disputes

If SARA@rafiq believes SARA@edventure shared information outside MoU boundaries:

1. The concerned SARA flags the issue in the audit log with a dispute tag
1. Both org admins are notified with the relevant audit entries
1. Both org admins review and either resolve or escalate
1. If unresolved, either org can invoke MoU revocation
1. The dispute is recorded in trust score history (affects future trust computation)

-----

## 13. The Rocket.Chat Experience

On mobile, the workspace switcher becomes the entry point to this entire ecosystem:

```
┌──────┐  ┌─────────────────────────────────┐
│  🏠  │  │  Personal SARA                   │
│──────│  │  Cross-org view, life management │
│  👨‍👩‍👧  │  │                                 │
│──────│  │  "Good morning. You have 3       │
│  R   │  │   meetings across RafiqAI and    │
│──────│  │   Ready4VC today. Family dinner  │
│  E   │  │   is at 7pm — I've blocked it   │
│──────│  │   across all calendars."         │
│  4   │  │                                 │
│      │  │                                 │
└──────┘  └─────────────────────────────────┘

🏠 = Personal workspace (myofficespace.com)
👨‍👩‍👧 = Family workspace
R  = RafiqAI
E  = EdVenture  
4  = Ready4VC
```

Each workspace is a separate Rocket.Chat server in the app’s server list. One tap to switch context. Each has its own SARA personality, its own channels, its own knowledge base. The Personal workspace is the hub that ties everything together.

-----

## 14. Build Phases for SARA-to-SARA

### Phase 1: Foundation (build now)

- [ ] Define MoU data structure and storage format in Outline
- [ ] Implement Bridge service in sara-api with internal function calls
- [ ] Implement MoU permission check middleware
- [ ] Build append-only audit log (PostgreSQL table)
- [ ] Generate cryptographic keypairs per SARA context
- [ ] Implement Personal SARA availability broker
- [ ] Build auto-approve rules configuration
- [ ] Implement MoU create, activate, revoke lifecycle
- [ ] Build trust score computation from shared people signals

### Phase 2: Cross-Org Intelligence (build when using daily)

- [ ] Implement automatic cross-org sharing within MoU boundaries
- [ ] Build trust score computation from financial and domain signals
- [ ] Add trust score evolution tracking over time
- [ ] Implement SARA-initiated MoU expansion recommendations
- [ ] Build cross-org Rocket.Chat channels for active MoUs
- [ ] Implement data depth limits per trust tier
- [ ] Add MoU review reminders and renewal workflow

### Phase 3: Discovery (build when platform grows)

- [ ] Implement org discoverability settings (invisible/listed/discoverable)
- [ ] Build SARA networking discovery from shared signals
- [ ] Implement introduction protocol between SARAs
- [ ] Build trust score from behavioral signals (query patterns, disputes)
- [ ] Add network-wide pattern detection
- [ ] Build ecosystem reports and visualization

### Phase 4: Federation (build for platform launch)

- [ ] Convert Bridge from internal function calls to HTTPS API
- [ ] Implement signed request/response protocol
- [ ] Build myofficespace.com identity service (central Keycloak)
- [ ] Implement OIDC federation for org SARAs trusting central identity
- [ ] Build public key registry at myofficespace.com
- [ ] Implement cross-droplet audit log synchronization
- [ ] Load testing and security audit of federated communication

-----

## 15. What This Becomes

Today: A tool for Mr Be to manage three startups and a family without losing his mind.

Tomorrow: A platform where any person can have one identity, multiple workspaces, and an AI that brokers between them.

Eventually: A network where organizations’ AI agents discover each other, establish trust, share intelligence within agreed boundaries, and create value that no single organization could create alone.

The foundation — encrypted, signed, logged, revocable, trust-scored — supports all three stages without architectural changes. The use cases will be discovered by real people using the system. The security cannot be retrofitted. Build it right from day one.

-----

*This document was designed through dialogue between Mr Be and Claude, March 2026.*

*It extends the SARA Architecture v5.0 and SARA Mobile & Rocket.Chat Strategy with inter-organizational AI communication, trust frameworks, and the person-organization-role mental model.*