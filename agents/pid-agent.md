# PID Agent (Product Initiative Document)

Creates a Product Initiative Document — a deeper initiative brief used for roadmapping alignment between product, engineering, design, and leadership.

---

## When to use

Invoke this agent when:
- An idea has passed the one-pager stage and needs a formal brief for roadmapping
- The team is preparing for a quarterly planning cycle
- Leadership has approved further discovery and needs a structured summary

---

## Relationship to other documents

```
One-Pager → PID → Epic → Tickets
```

A PID answers "should we do this?" — the epic answers "how will we do this?" A PID is more complete than a one-pager but less detailed than a full epic.

---

## Required inputs

| Input | Required? | Notes |
|-------|-----------|-------|
| Initiative name | Yes | |
| Problem statement (How Might We) | Yes | |
| Background / context | Yes | What's true today |
| Proposed approach | Yes | High-level direction |
| Business case | Yes | Financial, strategic, or clinical value |
| Success metrics | Yes | KPIs and usage metrics |
| Stakeholders | Yes | PM, Tech Lead, Design, cross-functional |
| Dependencies | Recommended | Which teams need to be involved |
| Risks | Recommended | |
| Rough timeline / sizing | Recommended | T-shirt sizing: S / M / L / XL |
| Out of scope | Optional | What we're explicitly NOT doing |

---

## Process

1. **Load context**
   - Read `templates/pid_template.md`
   - Read `style_guide.md`
   - Read `examples/pids/example-pid.md` for tone, depth, and writing quality — use the template for structure; if the example conflicts with the template, follow the template
   - Read the initiative's `game-plan.md` — confirm it has an Objective filled in. If missing or still a placeholder, stop and ask the PM to complete it before proceeding.
   - Read the initiative's `references.md`
   - Read `initiatives/<name>/meeting-notes.md`, `transcripts.md`, and `analytics.md` — extract any relevant context, data points, decisions, or business case evidence before gathering inputs. If a file is empty or a placeholder, skip it.
   - Read `initiatives/<name>/learnings.md` — apply any prior findings about PID structure, business case framing, or stakeholder concerns before proceeding

2. **Gather inputs**
   - Ask for any missing required fields

3. **Draft the PID**
   - Lead with the problem statement
   - Be specific about the business case — vague value statements get deprioritized
   - Include risks with mitigations
   - Keep it under 4 pages

4. **Review with user**
   - Present full draft
   - Ask: "Is this ready for leadership review or do you want to iterate?"

5. **Save and publish**
   - Write the finalized PID to `initiatives/<name>/pid.md`
   - Then ask: **"Should I create the Confluence page now? This will be the collaborative source of truth — where the team reviews, comments, and keeps the PID current. (Yes / No)"**
   - If yes:
     - Check `initiatives/<name>/references.md` for the **Team** field. If set, look it up in `teams.md` automatically.
     - If Team is not set in `references.md`, read `teams.md`, list the known teams, and ask: **"Which team should this be created under?"**
     - If the team is **not** in `teams.md`: ask for Space Key and Parent Page manually, then ask: "Should I add this team to `teams.md` for future use? (Yes / No)" — if yes, append rows to both the Jira and Confluence tables
     - If Parent Page is a title (not a numeric ID), use `confluence_search` with `title = "<Parent Page>"` to resolve it to a page ID before creating
     - Use `confluence_create_page` with the full PID content converted to markdown, passing the resolved page ID as `parent_id`
     - Set the page title to: `PID: <Initiative Name>`
     - After creation, write the Confluence page URL and Space Key back to `initiatives/<name>/references.md` (Initiative Page and Space Key fields)
     - Update `initiatives/<name>/pid.md` — add a note at the top: `> Confluence source of truth: <page URL>`
   - If no: skip — PM can create the page manually later
   - Link back from the Jira epic when it is created

6. **Capture learnings**
   - Ask: "Did anything about this process reveal a gap — in the template, the agent instructions, or the style guide?"
   - If the learning is **generalizable** (would help any PM using this agent):
     - Update the relevant file directly (`templates/pid_template.md`, `agents/pid-agent.md`, or `style_guide.md`)
   - If the learning is **initiative-specific**:
     - Log it in `initiatives/<name>/learnings.md`

---

## Key differences from an epic

| | PID | Epic |
|--|-----|------|
| Written before | Roadmap prioritization | Team kickoff |
| Who reviews | Leadership, cross-functional leads | Full product + eng team |
| Design needed | No | Yes (before kickoff) |
| Milestones | Not required | Required |
| Tickets | Not required | Required |
| Lives in | Confluence | Jira |
