# Market Research Subagent Prompt Template

This document contains the master prompt for launching 6 parallel research agents to investigate markets for Rafiq AI.

---

## How to Use This Prompt

1. Copy the **Agent Prompt Template** below
2. Replace `{COUNTRY}`, `{REGION}`, and `{FOLDER}` with the specific market details
3. Launch 6 agents in parallel using the Task tool with `subagent_type: "general-purpose"`

---

## Market Assignments

| Agent | Country | Region | Folder | Currency | Language |
|-------|---------|--------|--------|----------|----------|
| 1 | USA | North America | `/USA/` | USD | English |
| 2 | Saudi Arabia | MENA | `/Saudi-Arabia/` | SAR | Arabic |
| 3 | Qatar | MENA | `/Qatar/` | QAR | Arabic |
| 4 | Turkey | Europe/MENA | `/Turkey/` | TRY | Turkish |
| 5 | United Arab Emirates | MENA | `/UAE/` | AED | Arabic/English |
| 6 | United Kingdom | Europe | `/UK/` | GBP | English |

---

## Agent Prompt Template

```
# MARKET RESEARCH TASK: {COUNTRY}

## Context

You are conducting comprehensive market research for **Rafiq AI**, an AI-powered instructional coaching platform for teachers. The founder is Egyptian and is evaluating 7 markets to determine the best entry point.

**Product:** AI instructional coach that provides personalized feedback to help teachers improve their teaching practice.

**Target Customers:**
- Primary: Private and international schools (B2B)
- Secondary: Individual teachers (B2C)

**Founder Context:**
- Egyptian founder (no existing network in {COUNTRY})
- Pre-seed stage, limited budget ($20-30K for Year 1)
- Needs to validate market before significant investment

---

## Your Task

Conduct deep desk research on **{COUNTRY}** following the 6-section framework below. Use extensive web searches to gather data. Create 8 output files with comprehensive findings.

---

## Research Sections

### SECTION 1: Legal & Regulatory Environment

**Web Searches to Conduct:**
1. "{COUNTRY} foreign company business registration requirements 2024 2025"
2. "{COUNTRY} data protection privacy law GDPR 2024"
3. "{COUNTRY} AI regulations artificial intelligence policy 2024 2025"
4. "{COUNTRY} EdTech education technology regulations approval"
5. "{COUNTRY} school procurement government contracts requirements"

**Questions to Answer:**
- What are business registration requirements for foreign companies?
- What are the data protection/privacy laws (GDPR, local equivalents)?
- What AI regulations exist or are being developed?
- What approvals are needed to sell EdTech to schools?
- What are procurement requirements for educational technology?
- What are the tax obligations (corporate, VAT)?

**Output:** Create `legal-regulatory.md` with findings organized by subsection.

---

### SECTION 2: Desirability - Market Demand

**Web Searches to Conduct:**
1. "{COUNTRY} number of teachers 2024 statistics UNESCO World Bank"
2. "{COUNTRY} public private schools statistics market size education"
3. "{COUNTRY} teacher professional development programs instructional coaching"
4. "{COUNTRY} education budget spending professional development"
5. "{COUNTRY} academic year calendar school dates"
6. "{COUNTRY} EdTech startups companies competitors education apps 2024"
7. "{COUNTRY} teacher salary average income 2024"

**Questions to Answer:**
- What is the total number of teachers? (K-12, by level)
- What is the public vs private school breakdown?
- Is instructional coaching adopted in this market?
- How mature is the coaching culture in education?
- Do schools invest in professional development?
- What is the average/typical PD budget per teacher?
- When is the academic year? (start/end dates)
- Who are the EdTech competitors?
- What is the average teacher salary?

**Output:** Create `desirability.md` with market size tables, competitor analysis, and demand signals.

---

### SECTION 3: Feasibility - Can We Execute?

**Web Searches to Conduct:**
1. "{COUNTRY} EdTech investment funding venture capital 2024"
2. "{COUNTRY} startup accelerators incubators EdTech education"
3. "{COUNTRY} startup costs business formation expenses 2024"
4. "{COUNTRY} education ministry decision makers procurement"

**Questions to Answer:**
- Do we need local presence for sales?
- What language/localization is required?
- What is the EdTech investment activity (funding rounds, M&A)?
- What accelerators/incubators support EdTech?
- What government grants or programs exist?
- What are market entry costs?
- What are ongoing operational costs?

**Output:** Create `feasibility.md` with execution assessment and cost estimates.

---

### SECTION 4: Viability - Profit Potential

**Web Searches to Conduct:**
1. "{COUNTRY} EdTech pricing SaaS education subscription model"
2. "{COUNTRY} school technology budget spending"
3. "{COUNTRY} private school fees tuition international schools"

**Questions to Answer:**
- What are customers willing to pay? (by segment)
- What pricing models work best? (per teacher, per school, subscription)
- What are the unit economics? (CAC, LTV estimates)
- How many customers needed to break even?
- What is realistic customer acquisition timeline?

**Output:** Create `viability.md` with pricing strategy, unit economics, and break-even analysis.

---

### SECTION 5: Strategic Validation

**Web Searches to Conduct:**
1. "{COUNTRY} teacher problems challenges pain points education"
2. "{COUNTRY} education reform initiatives 2024"
3. "{COUNTRY} EdTech adoption rates schools"

**Questions to Answer:**
- What are key assumptions to validate?
- What are teacher pain points in this market?
- How should we position Rafiq AI here?
- What outreach methods work best for reaching educators?
- What drives willingness to pay?

**Output:** Create `validation.md` with hypotheses, validation plan, and go/no-go criteria.

---

### SECTION 6: Ecosystem - Network & Support

**Web Searches to Conduct:**
1. "{COUNTRY} EdTech VC investors venture capital education"
2. "{COUNTRY} EdTech accelerator incubator programs"
3. "{COUNTRY} angel investors education technology network"
4. "{COUNTRY} private school chains networks"
5. "{COUNTRY} education conferences events EdTech"
6. "{COUNTRY} teacher associations unions professional"
7. "{COUNTRY} EdTech founders startups LinkedIn"

**Questions to Answer:**
- What VCs invest in EdTech? (names, check sizes, contacts)
- What accelerators/incubators exist? (application links, terms)
- What angel networks operate here?
- Who are the EdTech "godmothers/godfathers"?
- What school chains could be B2B partners?
- What events/conferences should we attend?
- What EdTech founders could provide advice?
- What media covers EdTech news?

**Output:** Create `Ecosystem.md` with:
- VC table (name, focus, check size, key contact)
- Accelerator table (program, investment, equity, application link)
- Angel networks list
- Key contacts table (name, org, role, LinkedIn)
- B2B partners (school chains, platforms)
- Events calendar
- Media/PR contacts

---

## Output Files to Create

Create all files in: `Market-Disk-Research/{FOLDER}/`

| File | Content |
|------|---------|
| `summary.md` | Executive summary with Quick Facts table, Overall Score, Key Insights, Recommendation |
| `legal-regulatory.md` | Section 1 findings |
| `desirability.md` | Section 2 findings |
| `feasibility.md` | Section 3 findings |
| `viability.md` | Section 4 findings |
| `validation.md` | Section 5 findings |
| `Ecosystem.md` | Section 6 findings (VCs, accelerators, contacts) |
| `sources.md` | All URLs and references used |

---

## Summary.md Template

The summary.md should include:

```markdown
# {COUNTRY} - Market Research Summary

## Quick Facts

| Attribute | Value | Source |
|-----------|-------|--------|
| Population | X million | World Bank |
| Number of Teachers | X | UNESCO/Local |
| Number of Schools | X | Ministry |
| Public/Private Ratio | X% / X% | Research |
| Academic Year | Month - Month | Ministry |
| Currency | XXX | - |
| Language | XXX | - |
| Avg Teacher Salary | XXX/month | Research |

## Overall Score

| Dimension | Score (1-5) | Notes |
|-----------|-------------|-------|
| Desirability | X/5 | [brief note] |
| Feasibility | X/5 | [brief note] |
| Viability | X/5 | [brief note] |
| Ecosystem | X/5 | [brief note] |
| **Total Weighted** | **X.X/5** | |

## Key Insights

### Opportunities
1. [opportunity 1]
2. [opportunity 2]
3. [opportunity 3]

### Challenges
1. [challenge 1]
2. [challenge 2]
3. [challenge 3]

## Ecosystem Overview
[Key VCs, accelerators, contacts summary]

## Research Status
- [x] Legal & Regulatory (Section 1)
- [x] Desirability (Section 2)
- [x] Feasibility (Section 3)
- [x] Viability (Section 4)
- [x] Strategic Validation (Section 5)
- [x] Ecosystem (Section 6)

## Recommendation
**Verdict: [GO / NO-GO / PROCEED WITH CAUTION]**
[Reasoning]
```

---

## Scoring Criteria

Score each dimension 1-5 based on:

| Criteria | 1 (Poor) | 3 (Average) | 5 (Excellent) |
|----------|----------|-------------|---------------|
| Market Size | <100K teachers | 100K-500K | >500K teachers |
| Coaching Adoption | None | Emerging | Mature culture |
| PD Investment | <1% budget | 1-3% | >3% budget |
| Network Strength | No contacts | Some access | Strong network |
| Regulatory Ease | Complex barriers | Navigable | Simple/supportive |
| Investor Access | No EdTech VCs | Some activity | Active ecosystem |
| Willingness to Pay | Very low | Moderate | High budgets |
| Break-even | >1000 users | 300-1000 | <300 users |
| Ecosystem | No support | Some programs | Rich ecosystem |

---

## Quality Standards

1. **Use multiple web searches** - At least 15-20 searches per market
2. **Include specific numbers** - Teacher counts, funding amounts, pricing
3. **Name specific contacts** - Real people with LinkedIn profiles
4. **Cite sources** - Include URLs in sources.md
5. **Create actionable insights** - Not just data, but recommendations
6. **Use tables** - For easy comparison across markets
7. **Be thorough** - Cover all subsections in each section

---

## Country-Specific Considerations

### USA
- Focus on district-level purchasing (not just schools)
- Research state-by-state variations
- Look for instructional coaching platforms (BetterLesson, Edthena, etc.)
- Large market but competitive

### Saudi Arabia
- Consider Vision 2030 education initiatives
- Arabic localization essential
- Ministry of Education centralized purchasing
- Growing EdTech investment

### Qatar
- Small but wealthy market
- Qatar Foundation ecosystem
- High per-student spending
- English widely used in private sector

### Turkey
- Large teacher population
- Turkish localization required
- Growing startup ecosystem
- Currency volatility consideration

### UAE
- Hub for regional EdTech
- GEMS, Taaleem school chains
- English widely used
- High willingness to pay in private schools

### United Kingdom
- Mature coaching culture (IRIS Connect, etc.)
- MAT (Multi-Academy Trust) purchasing
- Competitive market
- Strong EdTech ecosystem

---

## Final Deliverable

After completing all research, the agent should have created 8 files totaling approximately:
- summary.md: ~200 lines
- legal-regulatory.md: ~150 lines
- desirability.md: ~200 lines
- feasibility.md: ~150 lines
- viability.md: ~150 lines
- validation.md: ~150 lines
- Ecosystem.md: ~400 lines
- sources.md: ~100 lines

Total: ~1,500 lines of comprehensive market research per country.
```

---

## Launch Command Example

To launch all 6 agents in parallel, use:

```
I need you to launch 6 research agents in parallel to investigate markets for Rafiq AI.

Use the prompt template from Market-Disk-Research/subagent-research-prompt.md for each agent.

Launch these 6 agents simultaneously:
1. USA market research
2. Saudi Arabia market research
3. Qatar market research
4. Turkey market research
5. UAE market research
6. UK market research

Each agent should:
- Conduct extensive web searches (15-20 per market)
- Create 8 output files in the appropriate folder
- Follow the research framework from research-guide.md
- Score the market on all criteria
- Provide a GO/NO-GO/CAUTION recommendation

Run all agents in parallel for maximum efficiency.
```

---

## Individual Agent Prompts (Ready to Copy)

### Agent 1: USA

```
Conduct comprehensive market research on the USA for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/USA/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for USA:
- District-level purchasing power
- State-by-state regulatory variations
- Existing instructional coaching platforms (BetterLesson, Edthena, Swivl)
- Large but competitive market
- Teacher salary context (~$60K average)
```

### Agent 2: Saudi Arabia

```
Conduct comprehensive market research on Saudi Arabia for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/Saudi-Arabia/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for Saudi Arabia:
- Vision 2030 education transformation
- Arabic localization essential
- Ministry of Education centralized purchasing
- Growing EdTech investment (MISK, STV)
- High per-student spending potential
```

### Agent 3: Qatar

```
Conduct comprehensive market research on Qatar for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/Qatar/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for Qatar:
- Small but wealthy market
- Qatar Foundation ecosystem
- High per-student spending
- English widely used in private education
- Qatar National Vision 2030
- Limited local VC but regional access
```

### Agent 4: Turkey

```
Conduct comprehensive market research on Turkey for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/Turkey/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for Turkey:
- Large teacher population (1M+)
- Turkish localization required
- Growing startup ecosystem (Istanbul hub)
- Currency volatility (TRY/USD)
- Bridge between Europe and MENA
- KVKK data protection law
```

### Agent 5: UAE

```
Conduct comprehensive market research on the United Arab Emirates (UAE) for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/UAE/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for UAE:
- Regional EdTech hub
- GEMS, Taaleem, ADEK school networks
- English widely used
- High willingness to pay in private schools
- Dubai/Abu Dhabi focus
- Strong government EdTech initiatives
- Hub for MENA expansion
```

### Agent 6: UK

```
Conduct comprehensive market research on the United Kingdom (UK) for Rafiq AI (AI instructional coaching for teachers).

Follow the research framework in Market-Disk-Research/research-guide.md covering all 6 sections:
1. Legal & Regulatory
2. Desirability (Market Demand)
3. Feasibility (Execution)
4. Viability (Profit Potential)
5. Strategic Validation
6. Ecosystem (VCs, accelerators, contacts)

Create 8 files in Market-Disk-Research/UK/:
- summary.md, legal-regulatory.md, desirability.md, feasibility.md, viability.md, validation.md, Ecosystem.md, sources.md

Use extensive web searches (15-20+). Include specific numbers, contacts with LinkedIn profiles, and actionable insights. Score the market 1-5 on all criteria and provide a GO/NO-GO recommendation.

Key considerations for UK:
- Mature instructional coaching culture
- Existing competitors (IRIS Connect, Edthena UK)
- MAT (Multi-Academy Trust) purchasing model
- Strong EdTech ecosystem (London hub)
- BETT conference
- UK GDPR compliance required
- Teacher recruitment crisis context
```

---

*Template Version 1.0 | Created: December 2024*
