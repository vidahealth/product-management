> Confluence source of truth: https://vidahealth.atlassian.net/wiki/spaces/DATA/pages/4312006677

# PID: Build PM Agents

February 2026

**PM:** David Klappoth
**Tech Lead:** TBD
**Designer:** N/A
**Status:** Draft

---

## Problem Statement

How might we reduce the time and cognitive load required to produce PM documentation so that the product team can spend more time on discovery, stakeholder alignment, and shipping?

---

## Background

Today, each PM maintains their own prompts, style, and AI workflow. There is no shared standard. The result: documentation quality varies across initiatives, institutional knowledge doesn't accumulate between projects, and every PM independently solves the same documentation problems. When a new initiative begins, the PM starts from scratch.

Key observations:
- There is no mechanism to improve documentation standards from one initiative to the next
- A PM who writes a well-crafted epic has no way to share what made it good in a form the team can reuse
- The tooling to support a shared, agent-driven workflow (Claude Code, Jira MCP, Confluence MCP) is available today

---

## Proposed Approach

Build a GitHub-hosted repository of AI agents, templates, a shared style guide, and a feedback loop that improves the system after every use. Every PM runs the same agents from Claude Code. The agents enforce structure and style automatically — PMs provide initiative-specific context, agents produce consistent output.

**What we are building:**
- 4 agents: Epic, Jira Task, One-Pager, PID
- Shared style guide and document templates
- Initiative folder structure (game-plan, references, meeting-notes, transcripts, analytics, learnings)
- Best-in-class example documents agents reference before drafting
- A feedback loop that routes learnings back into shared files after each session

**What we are NOT building:**
- Automated sprint planning or Jira workflow automation
- Slack or email integrations
- PR/commit documentation agents
- Custom Jira fields or project configuration

---

## Business Case

**Why do this?**

**Operational efficiency:**
Every PM documentation artifact — epic, one-pager, PID, ticket set — currently requires the PM to recall structure, apply a style, and format from scratch. Agents reduce first-draft time to under 5 minutes and eliminate formatting overhead entirely.

**Strategic value:**
Consistent documentation lowers the cost of cross-functional collaboration. Engineers who receive well-specified epics and tickets ask fewer clarifying questions. Leadership reviewing PIDs with consistent structure makes faster decisions. The compounding effect grows as more initiatives run through the system.

**Why now?**
Q2 planning is approaching. Onboarding PMs before planning begins means the first wave of roadmap artifacts — epics and PIDs — are produced through the shared system, giving the team a clean baseline to evaluate quality and time savings.

---

## Success Metrics

**Business KPIs:**
- At least 3 PM initiatives running through the repo by end of Q2 2026
- Reduction in time to produce a first-draft epic or PID (target: under 5 minutes from briefing)

**Usage metrics:**
- Agent invocations per PM per week
- First-draft acceptance rate (how often the agent draft is used with minimal revision)
- Number of learnings promoted from initiative-specific to shared files

---

## Stakeholders

| Role | Name | Involvement |
|------|------|-------------|
| PM | David Klappoth | Owner |
| Tech Lead | TBD | Setup support, MCP configuration |
| Engineering | All teams | Receives epics and tickets produced by agents |
| Leadership | — | Approve adoption; primary consumers of one-pagers and PIDs |

---

## Dependencies

| Team / System | What we need | Owner | Date notified |
|---------------|-------------|-------|--------------|
| Each PM | Claude Code installed, repo access granted | David Klappoth | TBD |
| Jira | MCP integration configured | TBD | Already active |
| Confluence | MCP integration configured | TBD | Already active |

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| PMs revert to ad hoc prompts after initial use | Med | High | Demo session + clear first-use flow in README |
| Shared templates or agents updated without review, degrading quality | Low | High | PR review required for changes to shared files |
| Example documents remain placeholders, reducing agent output quality | High | Med | PM team prioritizes adding one real example per document type before Q2 planning |

---

## Work Breakdown

High-level phases of work. Each phase maps to one or more Epics. Use this section as input when running the Epic Agent.

| Phase | Description | Key deliverables | Dependencies |
|-------|-------------|-----------------|--------------|
| Phase 1: Foundation | Build the repo structure, agents, templates, and style guide | Agents (Epic, Jira Task, One-Pager, PID), templates, style guide, CLAUDE.md, initiative folder structure | None — this is the starting point |
| Phase 2: Quality Calibration | Validate agent output against real PM scenarios; populate example documents | Best-in-class examples for all 5 document types; agent prompt revisions based on output review | Phase 1 complete |
| Phase 3: Team Rollout | Onboard the PM team before Q2 planning; run a demo session | Each PM set up with Claude Code and repo access; first real initiative folder beyond build-pm-agents | Phase 2 complete; Q2 planning date confirmed |
| Phase 4: Expand | Add agents for adjacent PM tasks based on team feedback | Candidates: ADR agent, meeting-notes-to-tickets agent | Phase 3 complete; team has used the system on at least 2 real initiatives |

**Notes:**
- Phases 1 and 2 can overlap — validation can begin on completed agents while remaining agents are still being refined
- Phase 4 is discretionary; the system is fully usable without it
- Phase 3 is the critical path dependency for Q2 planning value

---

## Sizing

- [x] Medium (2–6 weeks)

**Rationale:** Foundation (Phase 1) is complete. Phases 2–3 are estimated at 2–3 weeks with one PM running validation and one team onboarding session before Q2 planning.

---

## Open Questions

1. Who owns the tech lead role for this initiative, or is it PM-only? *(Owner: David Klappoth — resolve before kickoff)*
2. What is the Q2 planning kickoff date? *(Determines onboarding deadline — Owner: David Klappoth)*
3. What is the PR review process for changes to shared agent/template files? *(Needs a decision before team-wide adoption — Owner: David Klappoth)*

---

## Links

- Epic: [PLAT-533](https://vidahealth.atlassian.net/browse/PLAT-533)
- One-Pager: `initiatives/build-pm-agents/one-pager.md`
- Related: `initiatives/build-pm-agents/game-plan.md`
