# Isolate Warehouse Failures by Domain

January 2026 | PM: David K

---

## Context

Today, warehouse builds run as a single monolithic DAG — every domain builds together or not at all.

- A single model failure (bad join, schema mismatch, failed test) stops the entire build regardless of which domain owns it
- When claims processing breaks, labs, eligibility, and billing wait — even when their own models are healthy
- Recovery requires all-hands triage for incidents that affect only one domain, inflating incident duration and pulling in engineers with no ownership of the failure

## Problem Statement

How might we isolate warehouse build failures by domain so that one broken model no longer blocks all downstream data delivery?

## Business Case

Why should we do this? Why now?

**Operational Efficiency**
An estimated 60% of wasted compute during build incidents affects domains that would have succeeded. Isolation recovers that compute immediately and removes the all-hands response burden from incidents that are, by nature, contained.

**Time to Recovery**
Under the current monolith, the mean time to restore any domain's data depends on the slowest fix — not the fastest. Domain isolation means unaffected areas deliver on schedule while the broken domain is triaged separately.

**Strategic Value**
The warehouse will grow. Monolithic builds get more fragile as model count increases, not less. This is a foundational change that prevents an entire class of incidents from scaling with the platform — the cost of not doing it compounds over time.

---

*For questions: David K — #data-platform*
