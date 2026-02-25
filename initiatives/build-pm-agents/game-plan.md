# Game Plan: Build PM Agents

February 2026

PM: David Klappoth

---

## Objective

Build a set of AI-powered agents and documentation standards that help the product management team produce consistent, high-quality documentation and manage Jira tickets more efficiently.

This initiative also serves as a working example for how all future initiatives should be structured in this repository.

---

## Why This Approach

Most PMs default to ad hoc AI usage — individual chat sessions, personal prompts, privately refined workflows. That works for one person but doesn't scale: outputs vary, institutional knowledge doesn't accumulate, and every PM is solving the same problems in isolation.

Storing agents in a GitHub repo changes the dynamic:

- **Shared prompts, consistent outputs.** Every PM runs the same style guide, the same rules, the same templates. The output one PM gets from the Epic Agent is comparable to what any other PM gets — not because they're following a style doc, but because the agent enforces it automatically.
- **Collaborative refinement.** When someone finds a gap in a template or a prompt that produces bad output, they fix it in the repo and everyone benefits immediately. Git history tracks what changed and why.
- **Documentation as infrastructure.** The repo is not just for generating docs — it *is* the documentation system. Meeting notes, analytics, learnings, and finalized artifacts all live alongside the agents that produce them.
- **Flexibility as needs change.** New document types, new agents, new process rules — all added as files without rebuilding anything. The system grows incrementally rather than requiring a platform migration.

---

## Problem Statement

How might we reduce the time and cognitive load required to produce PM documentation so that the product team can spend more time on discovery, stakeholder alignment, and shipping?

---

## Success Metrics

- Each agent can produce a draft document or set of Jira tickets from a short briefing in under 5 minutes
- Documentation quality is consistent across PMs (measured by template adherence)
- At least 3 other PM initiatives use this repo structure by end of Q2 2026

---

## Milestones

### Milestone 1: Foundation
- [x] Repository created with basic structure
- [x] Epic template populated

### Milestone 2: Standards and Configuration
- [x] Style guide written
- [x] CLAUDE.md created (Claude Code project configuration)
- [x] skill.md populated
- [x] README updated

### Milestone 3: Agent Definitions
- [x] Epic agent defined
- [x] Jira task agent defined
- [x] One-pager agent defined
- [x] PID agent defined

### Milestone 4: Templates
- [x] One-pager template created
- [x] PID template created

### Milestone 5: Validation
- [ ] Run each agent against a real PM scenario
- [ ] Iterate on agent prompts based on output quality
- [ ] Document any edge cases or known limitations

### Milestone 6: Expand
- [ ] Share with PM team and run a walkthrough
- [ ] Add first real initiative folder as a second example
- [ ] Consider: ADR agent
- [ ] Consider: meeting-notes-to-tickets agent

---

## Risks

| Risk | Mitigation |
|------|-----------|
| Agents produce inconsistent output | Tighten templates and style guide; add examples |
| Jira project keys vary by initiative | Require `references.md` with project key for all initiatives |
| Team adoption is low | Create clear README and run a demo session |

---

## Out of Scope (for now)

- Automated sprint planning
- Integrations with Slack or email
- Custom Jira workflow automation
- PR/commit documentation agents

---

## Links

See `references.md` for Jira project key and related links.
