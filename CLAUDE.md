# Product Management Agents

This repository provides AI-powered agents for product management documentation and Jira workflows.

## Quick Start

Tell me which initiative you're working on (folder name under `initiatives/`). I'll load the relevant context automatically.

To use an agent, ask me to run one:
- **Epic Agent** — create or update a Jira epic
- **Jira Task Agent** — create or update Jira tickets
- **One-Pager Agent** — draft a one-page initiative summary
- **PID Agent** — create a full Product Initiative Document

## Repository Structure

```
agents/          # Agent instructions — one file per agent type
initiatives/     # One folder per initiative (active + reference)
templates/       # Document templates
style_guide.md   # Writing standards for all PM docs
skill.md         # Main PM skill definition
adr/             # Architecture Decision Records
scripts/         # Automation scripts
```

## How Agents Work

Each file in `agents/` is a prompt definition for a specific workflow. When you ask me to run an agent, I will:

1. Read the agent file for instructions
2. Read the relevant initiative's `references.md` for Jira project key and context
3. Read the appropriate template from `templates/`
4. Follow the style guide in `style_guide.md`
5. Produce output or create/update Jira tickets using the available MCP tools

## Initiative Folders

Each initiative lives in `initiatives/<name>/`:

| File | Purpose |
|------|---------|
| `game-plan.md` | Objectives, milestones, success criteria |
| `references.md` | Jira project key, Confluence links, stakeholders |
| `meeting-notes.md` | Running meeting notes |
| `transcripts.md` | Raw interview/meeting transcripts |
| `analytics.md` | Query results, dashboard links, key metrics, experiment results |
| `learnings.md` | What changed and why — feeds back into shared resources |
| `one-pager.md` | _(optional)_ Finalized one-pager — written by the One-Pager Agent |
| `pid.md` | _(optional)_ Finalized PID — written by the PID Agent |

## Examples

`examples/` contains best-in-class reference documents. Agents read these before drafting to calibrate quality.

| File | Used by |
|------|---------|
| `examples/epics/example-epic.md` | Epic Agent |
| `examples/one-pagers/example-one-pager.md` | One-Pager Agent |
| `examples/pids/example-pid.md` | PID Agent |
| `examples/stories/example-story.md` | Jira Task Agent |
| `examples/sub-tasks/example-sub-task.md` | Jira Task Agent |

When a placeholder is replaced with a real document, agent output quality improves immediately.

## Tools Available

This project has access to Jira and Confluence via MCP.

**Jira:** create issues, update issues, search (JQL), transition issues, add comments, link issues to epics

**Confluence:** search, create pages, update pages

Always retrieve the Jira project key from the initiative's `references.md` before creating tickets.

`teams.md` maps team names to their Confluence space and parent page. The PID agent reads this when creating a Confluence page — if the team isn't listed, it asks and offers to add the row.

## Architecture Decision Records (ADRs)

ADRs in this repo capture **product and process decisions** — the stable, cross-cutting business precepts that govern how all initiatives are built and prioritized. They are not software architecture docs.

Good candidates: scoring frameworks, required process gates, platform/audience decisions, non-negotiable standards (accessibility, feature flags, etc.).

- Template: `adr/adr_template.md`
- Naming: `adr/[number]-[short-title].md`
- If someone asks "why do we do it this way?", the ADR is the answer.

## Feedback Loop — Updating the System

When agents produce poor output or a template has a gap, here's how learning flows back in:

| What happened | Action |
|--------------|--------|
| Agent prompt was missing a step | Update `agents/<agent>.md` directly |
| Writing standard was unclear | Update `style_guide.md` |
| A process decision needs to be official | Create a new ADR |
| Initiative-specific finding | Log in `initiatives/<name>/learnings.md` |

The agent files are living documents. When you learn something, update the file. Git history tracks what changed and when.

## Adding a New Initiative

1. Create `initiatives/<initiative-name>/` folder
2. Create `game-plan.md`, `references.md`, `meeting-notes.md`, `transcripts.md`, `analytics.md`, `learnings.md`
3. Add the Jira project key and relevant links to `references.md`
4. Start using the agents
