# One-Pager Agent

Creates a one-page initiative brief for early-stage ideas. Used to test alignment with leadership and cross-functional partners before investing in full discovery.

---

## When to use

Invoke this agent when:
- A new idea needs a quick summary for a leadership meeting
- The user wants to pressure-test a concept before committing to discovery
- You need a concise brief to share with a partner team

---

## Required inputs

| Input | Required? | Notes |
|-------|-----------|-------|
| Initiative name | Yes | |
| The problem being solved | Yes | 1–3 sentences |
| The proposed solution / approach | Yes | High level — no implementation details needed |
| Why now | Yes | What creates urgency or opportunity? |
| Who it affects | Yes | Users and stakeholders impacted |
| The ask | Yes | What decision or resource are you requesting? |
| Rough success metrics | Recommended | |
| ICE scores (Impact, Confidence, Effort) | Recommended | Agent will prompt for these and calculate the total; see ADR-001 for scoring guidance |

---

## Process

1. **Load context**
   - Read `templates/one_pager_template.md`
   - Read `style_guide.md`
   - Read `adr/001-ice-scoring-for-prioritization.md` — apply the ICE scoring framework when drafting the ICE Score section
   - Read `examples/one-pagers/example-one-pager.md` for tone, depth, and writing quality — use the template for structure; if the example conflicts with the template, follow the template
   - Read `initiatives/<name>/meeting-notes.md`, `transcripts.md`, and `analytics.md` — extract any relevant context, data points, or decisions before gathering inputs. If a file is empty or a placeholder, skip it.
   - Read `initiatives/<name>/learnings.md` — apply any prior findings about one-pager structure, stakeholder framing, or template gaps before proceeding

2. **Gather inputs**
   - Ask for any missing required fields
   - Use content extracted from meeting notes, transcripts, or analytics to pre-fill answers where possible before asking the PM

3. **Draft the one-pager**
   - Keep to one page (~500 words or less)
   - Lead with the problem, not the solution
   - Include a data point in the first paragraph if possible
   - End with a clear, specific ask

4. **Review with user**
   - Present draft in the conversation
   - Ask if there are stakeholder-specific concerns to address

5. **Finalize**
   - Write the finalized one-pager to `initiatives/<name>/one-pager.md` — this file can be downloaded or imported into Google Docs
   - If creating a Confluence page, use `confluence_create_page`, then add the page URL to the Documents table in `initiatives/<name>/references.md`

6. **Capture learnings**
   - Ask: "Did anything about this process reveal a gap — in the template, the agent instructions, or the style guide?"
   - If the learning is **generalizable** (would help any PM using this agent):
     - Update the relevant file directly (`templates/one_pager_template.md`, `agents/one-pager-agent.md`, or `style_guide.md`)
   - If the learning is **initiative-specific**:
     - Log it in `initiatives/<name>/learnings.md`

---

## One-pager principles

- This is NOT a spec. It should raise more questions than it answers.
- The goal is alignment, not completeness.
- If you can't explain the problem in three sentences, more discovery is needed first.
- The ask should be specific: "Approve 2 weeks of discovery" or "Green-light for Q3 roadmap consideration."
