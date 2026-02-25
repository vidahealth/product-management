# PM Style Guide

Writing standards for all product management documentation in this repository.

---

## Principles

1. **Start with the problem, not the solution** — Always define what's broken or missing before describing what to build.
2. **Data first** — Support every claim with a metric, user quote, or business observation.
3. **Say it once, clearly** — Avoid repetition. If something needs repeating, it belongs higher in the document.
4. **Assume a smart reader with no context** — Write as if the reader is capable but unfamiliar with your initiative.

---

## Document Hierarchy

| Document | When to create | Audience | Length |
|----------|---------------|----------|--------|
| One-Pager | Early idea stage | Leadership, cross-functional partners | ~1 page |
| PID (Product Initiative Document) | Before roadmapping | Product, Eng, Design, Leadership | 2–4 pages |
| Epic | Before discovery kicks off | Eng, Design, cross-functional teams | Full template |
| Jira Story | During planning | Engineering team | User story + AC |
| Jira Sub-Task | During sprint | Individual contributor | Technical steps only |

## Jira Issue Hierarchy

Issues nest strictly. Each level has a distinct purpose and is never substituted for another.

```
Epic
└── Story
    └── Sub-Task
```

| Level | Size | Purpose | Used for metrics? |
|-------|------|---------|------------------|
| **Epic** | 4–8 weeks | Exec-facing outcome. Roadmap planning and leadership communication. Owned jointly by Product and Engineering. | No — not used for fine-grained performance metrics |
| **Story** | 1–3 weeks | User or system outcome. Always belongs to exactly one Epic. Has clear AC and can be demoed. | Yes — primary unit for cycle time and throughput |
| **Sub-Task** | 1–3 days | Concrete execution step to complete a Story. Technical detail only (by system, component, or discipline). Never represents a standalone user outcome. | No — exists to help engineers execute, not to represent progress |

**Nesting rules:**
- Stories are created with `parent = <epic-key>`
- Sub-Tasks are created with `parent = <story-key>`
- Sub-Tasks are never parented directly to an Epic

---

## Tone and Voice

- **Active voice**: "The feature will enable X" → "X becomes possible."
- **Direct and specific**: Avoid hedging language like "potentially," "might," or "could possibly."
- **User-centered**: Name your users ("Members," "Clinicians," "Account Managers") rather than writing abstractly.
- **Jargon**: OK to use established team terms; always spell out acronyms on first use.

---

## Formatting Standards

### Headers
- Use `#` for document title
- Use `##` for major sections
- Use `###` for sub-sections
- Never skip levels

### Lists
- Use bullet points for non-sequential items
- Use numbered lists only for ordered steps or ranked items
- Keep bullets parallel — same grammatical structure, same level of detail

### Problem Statements
Use the **How Might We** format at the epic level:
> How might we [achieve outcome] for [user/stakeholder] so that [business result]?

Example:
> How might we surface billing errors before claim submission so that clinicians spend less time on administrative rework?

### User Stories
Use this format at the **Story** level:
> As a [type of user], I need to [do something], so that [reason/outcome].

Sub-Tasks do not need a user story — they are technical steps. Their title and a brief description of what to build/change is sufficient.

### Acceptance Criteria
Use this format at the bottom of every **Story**:
> When this work is complete, [outcome] will be true.

Write one criterion per line. Anyone should be able to read it and understand what was built.

### Metrics
Split metrics into two categories:
- **Business KPIs**: How will this affect revenue, retention, or efficiency at the business level?
- **Usage metrics**: How will users interact with this feature? Are they successful? Getting stuck?

### Quantifying impact under uncertainty
When an estimate has a wide range, frame it as a floor with modest upside acknowledgment — not as a midpoint or ceiling. This preserves a credible business case without overpromising.

- Too optimistic: "Reduces queue times by 30–120 minutes"
- Correct: "Reduces queue times by at least 30 minutes; upside of 60–120 minutes as adoption grows"

Use this framing in business cases, KPIs, and any section where estimates are uncertain.

---

## Common Terms

| Term | Definition |
|------|-----------|
| Epic | 4–8 week exec-facing outcome. Unit of roadmap planning. Broken into Stories. |
| Story | 1–3 week user or system outcome. Always belongs to one Epic. Has AC, can be demoed. Primary cycle time metric unit. |
| Sub-Task | 1–3 day execution step. Always belongs to one Story. Technical detail only — not used for roadmap or metrics. |
| Milestone | Term used in the epic template to describe the Stories that will be created under an Epic. "Milestones" in an epic = Stories in Jira. |
| ICE Score | Impact × Confidence ÷ Effort — used to prioritize epics during roadmapping |
| PID | Product Initiative Document — a pre-epic summary used for leadership alignment |
| One-Pager | A short brief for a very early-stage idea; used to test alignment before investing in discovery |
| ADR | Architecture Decision Record — a short doc capturing a key technical decision |
| GTM | Go-to-Market — the plan for how a feature gets launched to customers |

---

## Document Title Conventions

- Epic titles: start with a verb — "Modernize the billing dashboard" not "Billing dashboard modernization"
- Story titles: start with a verb — "Add date filter to encounter list" not "Date filter"
- Sub-Task titles: start with a verb, be specific — "Write migration script for legacy encounter IDs"
- One-pager titles: describe the outcome — "One-Pager: Reducing Prior Authorization Lag"

---

## What to Avoid

- Passive voice: "It was decided that..." → "We decided..."
- Vague metrics: "improve performance" → "reduce p95 load time from 4s to <1s"
- Solution-first thinking: Don't write the approach before the problem
- Overloaded bullet points: If a bullet needs more than two lines, break it into its own section
- Placeholder text left in: Search for "xyz" and "TBD" before sharing any document
- Verbosity in Jira content: Engineers read tickets at speed. Condense output, limit blank lines, use plain speech. Cut preamble ("This epic covers..." → just start with the content).
- Overstating uncertain impact: Wide estimate ranges should be presented as conservative floors, not midpoints or best cases
