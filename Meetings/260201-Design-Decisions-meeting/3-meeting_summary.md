# Rafiq Product Design Meeting - Summary
**Date:** February 1, 2026
**Duration:** 2 hours 25 minutes
**Participants:** Ahmed Youssry (Product/Technical Lead), Shereen Sayed (Coaching Expert / Domain Lead), Nermeen Ahmed (Coaching Expert / Curriculum)

---

## Context

This was a product design session for **Rafiq**, an EdTech coaching platform designed to support instructional coaches and teachers in K-12 schools, initially targeting the US market (Georgia). The meeting focused on resolving open design questions around data models, reporting, privacy, and terminology before Ahmed builds the underlying architecture.

---

## Key Decisions

### 1. "Cohort" Replaced with "Community"
- The term **cohort** was dropped for groups of teachers under the same coach
- Teachers grouped by a shared coach are now called a **community** (not a cohort)
- Communities can form around: same coach, same school, same grade level, or same topic
- A cohort implies shared timelines and shared journeys, which doesn't match reality -- teachers join at different points and progress individually

### 2. Core Unit: The Coaching Journey
- The fundamental unit is a **one-to-one journey** between one coach and one teacher (one-to-many at the coach level, but each journey is 1:1)
- The journey has **three phases**:
  1. **Contracting** -- setting goals, agreements, and expectations
  2. **Repeating observation cycles** -- the core coaching work (lesson plans, observations, feedback)
  3. **Closing cycle** -- documentation and performance reporting
- Journey duration defaults to **one semester**, configurable by school admin
- Journeys can be extended or renewed across semesters

### 3. Stay Out of Teacher Evaluation
- **Critical guardrail:** The app must NOT enter formal teacher evaluation territory
- Evaluation in US schools is politically charged -- tied to salaries, strikes, and careers
- The performance report should **contribute to** evaluation but never **replace or replicate** it
- No pre/post assessments or quantitative scoring of teacher performance
- Student performance data is **excluded entirely** from the MVP

### 4. Performance Report Design
The performance report is a low-stakes, positive-framed document with these sections:

| Section | Description |
|---------|-------------|
| **"Now I Can Do"** | Teacher self-reports skills they've developed |
| **"Next I Want to Explore"** | Teacher states future development goals |
| **Coach Testimonial** | Coach adds a narrative testimonial (not an evaluation) |
| **Evidence Attachments** | Links to observation data, lesson plans as supporting evidence |

- The AI assistant (Rafiq) helps generate the report using sentence starters and pulling from observation data
- The report is generated at midterm and end-of-term

### 5. Data Privacy Matrix
Data access levels need to be defined for every data point:

| Data Level | Who Sees It |
|-----------|-------------|
| **Private** | Teacher only (self-reflection, personal notes) |
| **Shared** | Teacher + Coach (observations, lesson plans, journey details) |
| **School** | + Principal (attendance, session counts, plan completion %) |
| **District** | + District admin (aggregate reports -- TBD what exactly) |

- Observation reports and lesson plans are **too detailed** for school/district level -- those stay between teacher and coach
- Session attendance and plan completion can be surfaced to school leadership on request (not by default)
- What the district receives is still **an open question** requiring stakeholder input

### 6. Lesson Plan Review via AI
- AI bot reviews lesson plans with customizable focus:
  - General lesson plan quality
  - Specific criteria (e.g., learner-centered activities, teaching strategies, alignment with development goal)
- Uses a sandwich feedback approach
- Could track engagement (how often teachers return for review) as a non-evaluative performance indicator

### 7. Market Entry: Georgia First
- Despite research suggesting California might rank higher on potential, the team chose **Georgia** as the entry market
- Rationale:
  - **Accessibility** -- Shereen has relationships at the university and can reach the district/schools
  - **Tester access** -- Karen (current user) is based there and provides direct feedback
  - Buyer access can be obtained through Shereen's other channels
- **Saudi Arabia** is a priority secondary market due to education reform momentum, AI/digitization vision, and being the largest regional market
- Nermeen has a new Saudi contact (via Mariam) connected to the Ministry of Education

---

## Open Questions (Deferred)

1. **What does the district want to see?** -- Need stakeholder interviews to determine what reports the district expects
2. **Closing session flow** -- Should the app facilitate a closing conversation between coach and teacher? Deferred to testing phase
3. **Video upload limits** -- Per-coach and per-self limits needed (direct cost impact). Not yet decided
4. **Coach performance report** -- New requirement surfaced in this meeting. Details TBD
5. **Questionnaire for stakeholders** -- To be created and circulated through Karen to other coaches in her school/district

---

## Action Items

| Owner | Action | Priority |
|-------|--------|----------|
| **Ahmed** | Update data models based on today's decisions (community, journey phases, privacy matrix) | High |
| **Ahmed** | Document the Georgia market entry decision with rationale | High |
| **Ahmed** | Update the feature base with insights from this session | High |
| **Shereen & Nermeen** | Finalize the contracting phase document (coaching plan template) | High |
| **Shereen** | Send Zoom meeting summary to Ahmed | Medium |
| **Nermeen** | Connect with Saudi Arabia ministry contact (from Mariam) | Medium |
| **Team** | Create stakeholder questionnaire for Karen's school/district | Medium |
| **Team** | Reschedule Cali's meeting (she can't make the 8th -- move to midweek or following week) | Admin |

---

## Scheduling Notes
- Cali cannot attend the meeting on February 8th -- needs to be rescheduled
- Shereen's availability on the 15th depends on her academic submission deadline (February 17th)
- Next tactical meeting is on the regular schedule; Nermeen and Shereen have a follow-up meeting after that
