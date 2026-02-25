# [Replace with a real best-in-class Sub-Task]

Drop a completed Sub-Task here that represents the quality bar for the team.

The Jira Task Agent will read this file before drafting Sub-Tasks to calibrate technical specificity and done criteria.

Guidelines for a good reference Sub-Task:
- Title starts with a verb and names the specific technical work — "Write migration script for legacy encounter IDs" not "Migration"
- Description is 1–3 sentences: what to build or change, and where
- No user story needed — this is technical execution, not a user outcome
- "Done when" criteria are technical and verifiable — not subjective
- Scoped to 1–3 days of work; if it's bigger, it should be a Story

Place holder sub-task PLAT-371 used as a reference

Why: LLM tools (Claude Code, Codex) need context files from day one. Create these FIRST, then update them continuously as patterns emerge throughout the POC.

Checklist:

Create docs/style_guide.md: Initial naming conventions, classification rules, placeholder sections

Create docs/references.md: Link to epic, Jira checklist, architectural guidelines, placeholder for ADRs

Create docs/skill.md: Project context summary, first-wave domain list, example prompts

Acceptance Criteria: All 3 files created before other Phase 1 tasks begin. Files are functional (can be rudimentary, but not empty placeholders).

Estimated Effort: 0.5 days (initial creation)

THIS IS A CONTINUOUS RESPONSIBILITY, NOT A ONE-TIME TASK

After every task in every phase, ask yourself:

Did I discover a new pattern? → Add to style_guide.md

Did I find a useful reference? → Add to references.md

Did I learn what works/doesn't for agents? → Update skill.md

TASK-6.5 is an AUDIT — if the files aren't comprehensive by then, the team wasn't doing continuous updates.