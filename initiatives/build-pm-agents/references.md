# References: Build PM Agents

---

## Team

**Team:** Data Platform

---

## Jira

**Project Key:** PLAT

**Board:** [Link TBD]

### Key Issues

| Type | Key | Summary | Parent |
|------|-----|---------|--------|
| Epic | PLAT-533 | Launch shared PM documentation agents across the product team | — |
| Story | PLAT-534 | Validate agent quality against real PM scenarios | PLAT-533 |
| Sub-Task | PLAT-536 | Update README and CLAUDE.md to specify Opus 4.6 as the recommended model | PLAT-534 |
| Sub-Task | PLAT-537 | Run all 4 agents against a real initiative beyond build-pm-agents | PLAT-534 |
| Sub-Task | PLAT-538 | Populate examples/ folders with real completed artifacts | PLAT-534 |
| Sub-Task | PLAT-539 | Apply agent prompt revisions from validation findings | PLAT-534 |
| Story | PLAT-535 | Onboard the PM team before Q2 planning | PLAT-533 |
| Sub-Task | PLAT-540 | Confirm Q2 planning kickoff date and update game-plan.md | PLAT-535 |
| Sub-Task | PLAT-541 | Grant repo access and verify Claude Code installation for all PMs | PLAT-535 |
| Sub-Task | PLAT-542 | Review Max Pederson's story granularity structure and incorporate into demo | PLAT-535 |
| Sub-Task | PLAT-543 | Run live onboarding session with PM team | PLAT-535 |

---

## Confluence

**Space Key:** DATA

**Parent Page:** Data Platform Team

**Initiative Page:** https://vidahealth.atlassian.net/wiki/spaces/DATA/pages/4312006677

---

## Related Resources

- [Epic Template](../../templates/epic_template.md)
- [One-Pager Template](../../templates/one_pager_template.md)
- [PID Template](../../templates/pid_template.md)
- [Style Guide](../../style_guide.md)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)

---

## Stakeholders

| Name | Role | Contact |
|------|------|---------|
| David Klappoth | PM | |
| [Tech Lead] | Tech Lead | |

---

## Decision Log

| Date | Decision | Rationale |
|------|---------|-----------|
| Feb 2026 | Use Markdown for all agent definitions | Portable, readable, works natively with Claude Code |
| Feb 2026 | Store Jira project key in references.md | Prevents hardcoding; one source of truth per initiative |
| Feb 2026 | CLAUDE.md as project config entry point | Loads automatically in every Claude Code session |
