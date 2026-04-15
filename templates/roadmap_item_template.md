# [Title — start with a verb, short description of the work]

[Month Year] | PM: [Name] | Tech Lead: [Name]

---

## Opportunity Doc

> **Required for roadmapping consideration.** Complete Context, Problem Statement, Business Case, and ICE Score before submitting for roadmap prioritization.

### Context

Today, [current state is true — describe the world as it is, not the problem you want to solve].

- [Data point or observable fact that illustrates this reality]
- [A second data point or consequence of the current state]
- [A third data point — optional, only if it adds distinct evidence]

### Problem Statement

How might we [achieve outcome] so that [business result]?

### Business Case

Why should we do this? Why now?

**[Category — e.g., Financial Impact]**
[One concrete claim backed by data or a well-reasoned estimate. Use conservative floors, not midpoints.]

**[Category — e.g., Clinical / Operational Efficiency]**
[One concrete claim about time saved, errors reduced, or capacity freed.]

**[Category — e.g., Strategic Value]**
[Why this matters beyond cost savings — platform positioning, risk reduction, competitive necessity, etc.]

### Expected Outcome

What does success look like? Align on this during roadmapping.

- Goals: [Increase X by Y%]
- Guardrails: [No increase in support tickets, no drop in consult completion rate]

### ICE Score

| Dimension | Score (1–10) | Notes |
|-----------|-------------|-------|
| Impact | | How much value does this deliver if it works? |
| Confidence | | How confident are we in impact and approach? |
| Effort | | How much work is required? (10 = low effort) |
| **Total** | | Impact × Confidence ÷ Effort |

See `adr/001-ice-scoring-for-prioritization.md` for scoring guidance.

---

## High Level Approach

> **Required before team kick off / alignment meeting.**

[One sentence overview of what you're building — new process, new user-facing UI, backend service, etc.]

[Diagram: use boxes and arrows to describe the expected flow. Link to a Miro, Figma, or embed ASCII art.]

---

## Business Guidelines

> **ACTION REQUIRED:** For each cross-functional partner below, describe what must be true in their world for this initiative to succeed. Remove partners that are not relevant. Add partners that are missing.

Have you collaborated with the following teams? What do they need for this epic to succeed?

- **Clinical:** [What must be true]
- **Interventions:** [What must be true]
- **Marketing:** [What must be true / Braze attributes and events needed]
- **Strategy:** [What must be true]
- **Member Services:** [What must be true]
- **Account Management:** [What must be true]
- **Finance:** [What must be true]
- **Legal + Compliance:** [What must be true]
- **Billing:** [What must be true]
- **BIHA (Aarathi's team):** [What must be true]

---

## Designs

> **ACTION REQUIRED:** Link to Figma files and embed at least one screenshot before kick off. Designs can evolve during development but should be solid enough to build against.

- Figma: [link]
- [Screenshot or description of key screens]

---

## Risks / Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| [Risk] | High / Med / Low | High / Med / Low | [How we reduce or accept it] |

---

## Metrics

### KPIs

How will we know this is affecting the overall business?

- [Business-level KPI — e.g., reduce member escalations related to X by Y%]

### Usage Metrics

How will we know people are successfully using this feature? Are they getting stuck?

- [Feature-level usage metric — e.g., % of eligible members who complete X flow]

---

## Logistics

- **A/B Test:** Yes / No
- **Feature Flag:** Yes / No
- **Available for all current Clients/Members:** Yes / No
- **Technical Documentation:** [Link]
- **Launch + GTM Planning:** [Link — not a blocker to kick off]

---

## [After Kick Off] Jira Stories + Tasks

> **ACTION REQUIRED:** Break this epic into Stories with your Tech Lead after kick off. Each Story needs a user story format ("As an X, I need to Y, so that Z") and acceptance criteria ("When this work is complete, XYZ will be true.").

Epic: [Epic Jira Key]

| Story | Priority | AC Summary |
|-------|----------|-----------|
| [Story title] | P0 / P1 / P2 / P3 | [When complete, X will be true] |

---

## Dependencies

| Team | What's needed | Owner | Date notified |
|------|-------------|-------|--------------|
| [Team Name] | [What you need them to do] | [Name] | [Date] |

---

## Future Iterations

> Things we are NOT doing right now — captured for visibility so we don't have to re-remember later.

-

---

## Productboard Copy (Paste-Ready)

> Use this section to maintain a Productboard-friendly version of this roadmap item. Keep it concise and table-free.

## Problem
[2-4 bullets describing what is broken today and who is affected]

## Business Impact
- [Impact statement 1]
- [Impact statement 2]

## Proposed Approach
- [One sentence on what will be built]
- [Architecture diagram link or doc link if backend-heavy]
- [UI design link if applicable; otherwise `N/A`]

## Metrics
- [KPI name] — Baseline: [X] | Target: [Y]
- [KPI name] — Baseline: [X] | Target: [Y]
- [Usage metric] — Baseline: [X] | Target: [Y]

## Risks
- Risk: [risk] | Mitigation: [mitigation]
- Risk: [risk] | Mitigation: [mitigation]

## Dependencies
- Team: [team] | Needed: [what is needed] | Owner: [owner]
- Team: [team] | Needed: [what is needed] | Owner: [owner]
