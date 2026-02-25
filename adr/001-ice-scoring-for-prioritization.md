# ADR-001: Use ICE Scoring for Roadmap Prioritization

**Date:** February 2026
**Status:** Accepted
**Owner:** PM Team

---

## Context

As the team scales and the volume of incoming feature requests, bug reports, and strategic initiatives grows, we need a consistent, defensible framework for deciding what gets prioritized each quarter.

Without a shared framework, prioritization decisions are made inconsistently — varying by who advocates loudest, which team submitted the request, or which PM has the most context in a given planning meeting. This creates frustration, distrust in the roadmap, and difficulty explaining decisions to stakeholders.

---

## Decision

We will use **ICE scoring** to evaluate and rank roadmap candidates during quarterly planning.

**ICE = Impact × Confidence ÷ Effort**

Each dimension is scored 1–10:
- **Impact**: How significantly will this move a key business or user metric if it works?
- **Confidence**: How certain are we in our Impact and Effort estimates? (Higher with more research/data)
- **Effort**: How much work is required? (1 = very high effort, 10 = very low effort — inverse scale)

ICE scores are a **starting point for conversation**, not a final verdict. The PM and leadership use scores to structure discussion, not replace judgment.

---

## Rationale

| Option | Why not chosen |
|--------|---------------|
| RICE (Reach × Impact × Confidence ÷ Effort) | Reach is difficult to estimate accurately for internal tools and clinical workflows; adds false precision |
| Gut feel / HiPPO (Highest Paid Person's Opinion) | Not defensible, not scalable, erodes PM credibility |
| Weighted scoring with custom criteria | Too much upfront calibration; criteria drift over time |
| MoSCoW (Must/Should/Could/Won't) | Useful for sprint-level decisions, not quarterly roadmap trade-offs |

ICE is simple enough to apply consistently across different PMs and initiative types while still capturing the three most important variables in a prioritization decision.

---

## Consequences

**Positive:**
- Roadmap decisions are explainable and auditable
- Cross-functional stakeholders can see why one initiative ranked above another
- New PMs can ramp up quickly on how prioritization works
- Encourages PMs to de-risk low-confidence items through research before roadmapping

**Negative / trade-offs:**
- ICE scores can be gamed if PMs inflate Impact or deflate Effort
- Does not account for strategic dependencies (e.g., "we must build X before Y")
- Initiatives that are strategically mandatory still need to be scheduled regardless of ICE score

**What this decision does NOT cover:**
- How to handle regulatory or compliance-mandated work (always prioritized separately)
- Tie-breaking when two initiatives have the same ICE score
- How ICE integrates with OKR alignment (a separate ADR if needed)

---

## Related

- [Epic Template](../templates/epic_template.md) — ICE scoring box referenced in roadmapping section
- [PID Template](../templates/pid_template.md) — Sizing and business case sections inform ICE scores
