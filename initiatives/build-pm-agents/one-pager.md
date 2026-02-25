# One-Pager: Build PM Agents

February 2026 | PM: David Klappoth

---

## Problem

The product team has no shared standard for AI-assisted documentation. Each PM maintains their own prompts, their own style, and their own workflow — output quality varies, institutional knowledge doesn't accumulate, and every PM independently solves the same documentation problems. There is no mechanism to improve from one initiative to the next.

## Proposed Approach

Build a GitHub-hosted repository of AI agents, templates, and a shared style guide that every PM runs from Claude Code. Agents handle epics, one-pagers, PIDs, and Jira tickets using shared prompts and a feedback loop that improves the system after each use.

## Why Now

Claude Code and Jira/Confluence integrations are mature enough to support a team-wide workflow today. Q2 planning is approaching — if PMs onboard before planning starts, the first wave of epics and PIDs will be produced through the system, giving leadership a consistent, high-quality documentation baseline heading into planning.

## ICE Score

| Dimension | Score (1–10) | Rationale |
|-----------|-------------|-----------|
| Impact | 7 | Measurably reduces documentation time and improves consistency across PMs; compounding effect as adoption grows; not directly patient-facing |
| Confidence | 8 | Foundation is already built and working; mechanism is clear; low technical unknowns; remaining risk is adoption, not feasibility |
| Effort | 8 | Phase 1 complete; 2–3 weeks of validation and rollout remain; PM-led with minimal engineering dependency |
| **ICE Total** | **7.0** | 7 × 8 ÷ 8 |

*ICE scores are a starting point for roadmap conversation, not a final verdict. See ADR-001.*

## Who It Affects

**Primary users:** Product Managers — documentation drafting and Jira ticket creation

**Stakeholders:** Engineering (receives better-specified epics and tickets), Design (consistent documentation to review), Leadership (more consistent roadmap artifacts)

## Success Looks Like

- Each agent produces a draft document or Jira ticket set in under 5 minutes from a short briefing
- Documentation is consistent in structure and tone across all PMs, measured by template adherence
- At least 3 PM initiatives running through the repo by end of Q2 2026

## Risks / Open Questions

- Adoption: PMs may revert to ad hoc prompts if onboarding friction is high — mitigate with a demo and a clear first-use flow
- Shared file drift: if PMs update shared templates or agents without review, quality degrades — mitigate with PR review for shared file changes
- Quality bar: example documents are placeholders until replaced with real best-in-class artifacts

## The Ask

Approve team-wide adoption of this system and allocate one onboarding session before Q2 planning begins. Each PM needs approximately one hour to set up Claude Code and run their first agent against a real initiative. This positions the team to produce Q2 roadmap artifacts — epics, PIDs, and Jira tickets — through a shared system, giving leadership a consistent documentation baseline heading into planning.

---

*For questions: David Klappoth*
