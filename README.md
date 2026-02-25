# Product Management Agents

A repository of AI-powered agents, templates, and standards for the product management team. This is your instruction manual.

---

## Prerequisites

Before using this repo you need:

1. **Claude Code** installed — [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code)
2. **This repo cloned** and open as your working directory in Claude Code
3. **Jira and Confluence MCP** configured (one-time setup — ask your tech lead if not already active)

---

## How It Works

You talk to Claude Code. Claude reads the agent files in this repo to know how to behave — what to ask you, what structure to follow, what to write, and what to create in Jira or Confluence.

Every agent:
1. Reads your initiative folder for context
2. Asks you for anything missing
3. Drafts the document and shows it to you for review
4. Creates it in Jira or Confluence when you approve
5. Writes back to your initiative folder so the next session has context

---

## First-Time Setup

### 1. Add your team to `teams.md`

Open [teams.md](teams.md). If your team isn't listed, add a row to both tables:

```
Jira:       | Your Team Name | PROJ_KEY | your-label |
Confluence: | Your Team Name | SPACE    | Parent Page Title |
```

Open a PR so other PMs on your team benefit automatically.

### 2. Create your initiative folder

Initiative folders live in two places:

**Shared initiatives** — committed to the repo and visible to the whole team:
```
initiatives/
  your-initiative-name/
    game-plan.md       ← fill this in first
    references.md      ← set Team, Jira project key
    meeting-notes.md   ← add as you go
    transcripts.md     ← paste raw transcripts here
    analytics.md       ← paste query results and metrics here
    learnings.md       ← agent and PM write to this over time
```

**Personal initiatives** — kept local, never committed:
```
initiatives/personal/
  your-initiative-name/
    ...same structure...
```

Use `initiatives/personal/` when you're exploring an early-stage idea, doing individual work, or not ready to share with the team. Everything inside it is gitignored. When the initiative is ready to share, move the folder out of `personal/` and open a PR.

Copy `initiatives/example/` into whichever location fits and rename it to your initiative.

### 3. Fill in `references.md`

Set at minimum:
- `Team:` — must match a row in `teams.md`
- `Project Key:` — your Jira project key (e.g., `PLAT`)

### 4. Fill in `game-plan.md`

At minimum fill in **Objective** and **Problem Statement**. These are required before any agent will proceed.

---

## Running an Agent

Tell Claude Code which initiative you're working on, then which agent to run:

> "I'm working on **patient-portal-redesign**. Engage the PID agent."

Claude will confirm the initiative, load context from your folder, and walk you through the rest.

### Confirming your initiative

The first thing any agent does is confirm which initiative you're working on. It will list the folders under `initiatives/` and ask you to confirm. Type the folder name exactly:

> `patient-portal-redesign`

---

## Documentation Chain

Work in this order. Each document builds on the previous one — agents check for prerequisites and will stop if something is missing.

```
1. game-plan.md          Fill this in manually — sets the Objective and Problem Statement
         ↓
2. One-Pager             Early alignment for leadership (optional but recommended)
         ↓
3. PID                   Full initiative brief for roadmapping and leadership sign-off
         ↓
4. Epic                  Jira epic scoped from the PID's Work Breakdown
         ↓
5. Stories               Jira stories parented to the epic
         ↓
6. Sub-Tasks             Jira sub-tasks parented to stories
```

You don't have to produce every document for every initiative. A small initiative may go straight from game-plan → epic → stories. A larger one runs the full chain.

---

## The Agents

### One-Pager Agent
**When:** Early stage — idea needs a single page to align stakeholders before committing to a full PID.
**Output:** `initiatives/<name>/one-pager.md` + optional Confluence page
**To run:** "Engage the one-pager agent"

### PID Agent (Product Initiative Document)
**When:** Initiative has leadership buy-in and needs a formal brief for roadmapping.
**Output:** `initiatives/<name>/pid.md` + Confluence page (the collaborative source of truth)
**To run:** "Engage the PID agent"

After the PID is saved, the agent will ask: *"Should I create the Confluence page now?"* — say yes. The Confluence page is where the team collaborates and keeps the PID current. The local `pid.md` is a snapshot.

### Epic Agent
**When:** PID is approved and the team is ready to scope the work into Jira.
**Output:** Jira epic
**To run:** "Engage the epic agent"

The agent reads the latest PID from Confluence (if published) or from `pid.md`. It uses the PID's Work Breakdown section to scope the epic.

### Jira Task Agent
**When:** Epic exists in Jira and you're ready to write stories or break a story into sub-tasks.
**Output:** Jira stories and/or sub-tasks
**To run:** "Engage the jira task agent" or "Create stories for epic PLAT-12"

---

## Initiative Folder Files

| File | Role | Who provides it |
|------|------|----------------|
| `game-plan.md` | Input | PM fills in objectives and milestones |
| `references.md` | Input | PM fills in team, Jira key, Confluence links, stakeholders |
| `meeting-notes.md` | Input | PM adds running notes |
| `transcripts.md` | Input | PM pastes raw interview or meeting transcripts |
| `analytics.md` | Input | PM pastes query results, dashboard links, metrics |
| `learnings.md` | Both | PM or agent appends findings; agents read this at the start of every session |
| `one-pager.md` | Output | Written by the One-Pager Agent |
| `pid.md` | Output | Written by the PID Agent — snapshot, Confluence is the live version |

Jira epics, stories, and sub-tasks are agent outputs that live in Jira. Their keys are written back to `references.md` so the next session has them.

---

## Team Configuration (`teams.md`)

[teams.md](teams.md) maps team names to Jira and Confluence defaults. Agents read this automatically — PMs never need to look up project keys or space keys.

| Column | What it is |
|--------|-----------|
| Jira Space (Project Key) | Prefix of Jira issue keys, e.g., `PLAT` → `PLAT-123` |
| Default Labels | Applied to every Epic, Story, and Sub-Task for this team |
| Confluence Space Key | Short code for the team's Confluence space |
| Confluence Parent Page | Title of the page where initiative docs are nested |

To add your team: add a row to both tables in `teams.md` and open a PR.

---

## Examples (`examples/`)

Best-in-class reference documents agents read before drafting. They calibrate tone and quality — not structure (templates own structure).

| File | Used by |
|------|---------|
| `examples/epics/example-epic.md` | Epic Agent |
| `examples/one-pagers/example-one-pager.md` | One-Pager Agent |
| `examples/pids/example-pid.md` | PID Agent |
| `examples/stories/example-story.md` | Jira Task Agent |
| `examples/sub-tasks/example-sub-task.md` | Jira Task Agent |

**The better the example, the better the agent output.** Replace placeholders with real completed documents from your team.

---

## The Feedback Loop

After every agent session, the agent asks: *"Did anything reveal a gap — in the template, the agent instructions, or the style guide?"*

| What happened | Where it goes |
|--------------|--------------|
| Agent prompt was missing a step | Update `agents/<agent>.md` directly |
| Writing standard was unclear | Update `style_guide.md` |
| Template was missing a section | Update `templates/<template>.md` |
| Initiative-specific finding | Log in `initiatives/<name>/learnings.md` |
| Process decision needs to be official | Create a new ADR in `adr/` |

Git history tracks every change. You don't need a changelog.

---

## Repository Structure

```
agents/          # Agent prompt definitions — one file per agent
templates/       # Document templates — agents follow these for structure
examples/        # Best-in-class reference documents — agents read for quality
initiatives/     # One folder per initiative (initiatives/personal/ is gitignored)
teams.md         # Team → Jira and Confluence config
style_guide.md   # Writing standards for all PM documents
skill.md         # Orchestrator — routes requests to the right agent
CLAUDE.md        # Claude Code project config — loaded automatically every session
adr/             # Architecture Decision Records for process decisions
```

---

## Reference: All Agents

| Agent | File | Output |
|-------|------|--------|
| One-Pager Agent | `agents/one-pager-agent.md` | `one-pager.md` + optional Confluence page |
| PID Agent | `agents/pid-agent.md` | `pid.md` + Confluence page |
| Epic Agent | `agents/epic-agent.md` | Jira epic |
| Jira Task Agent | `agents/jira-task-agent.md` | Jira stories and sub-tasks |

---

## Reference: Templates

| Template | File |
|----------|------|
| Epic | `templates/epic_template.md` |
| One-Pager | `templates/one_pager_template.md` |
| PID | `templates/pid_template.md` |

---

## Reference: Architecture Decision Records

ADRs capture stable, cross-cutting product and process decisions — not software architecture.

| File | Decision |
|------|---------|
| `adr/001-ice-scoring-for-prioritization.md` | Why we use ICE scoring |

To add a new ADR: copy `adr/adr_template.md`, number it sequentially, write the decision.

---

## First Initiative

`initiatives/build-pm-agents/` is the first real initiative in this repo — it documents the work of building this agent system. Use it as the reference example for how a fully set-up initiative looks.

---

## Improving the System

To improve an agent, edit `agents/<agent>.md` directly.

To add a new template, add it to `templates/` and reference it from the relevant agent.

To add a new agent, create a file in `agents/` following the existing format and add it to `skill.md`.
