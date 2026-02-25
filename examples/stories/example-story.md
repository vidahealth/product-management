# [Replace with a real best-in-class Story]

Drop a completed Story here that represents the quality bar for the team.

The Jira Task Agent will read this file before drafting Stories to calibrate tone, specificity, and AC format.

Guidelines for a good reference Story:
- User story is in "As a [user], I need to [do something], so that [reason]" format
- The user type is specific — not "user" but "Member," "Clinician," "Account Manager"
- Acceptance criteria use "When this work is complete, X will be true" format
- Each AC criterion is one testable outcome — not a bundle
- Background/context is brief but gives the engineer what they need
- Title starts with a verb and describes the outcome, not the task

Place holder story PLAT-367 used as a reference

Context / why this story exists
We agree the epic is directionally right, but there are enough unknowns (Airflow integration details, selector shape, validation approach, materialization impacts, and edge cases) that we should learn by implementing one domain first, then generate the rest of the stories from what we learn.

Medical claims is the best first domain because:

it has high blast-radius impact today (views cause implicit full reprocessing)

it’s a clear demonstration of the value of isolation + materialization

it will surface real CI/CD + orchestration nuances early

Objective
Create a working end-to-end proof that we can:

run a single staging domain independently via dbt selector

trigger it from Airflow (staging environment)

validate outputs match the current pipeline (equivalence check)

document learnings and required standards so we can scale to other domains

Scope (what this story does)
In dbt repo
Define tagging + selector for the medical claims staging domain (start narrowly if needed).

Convert/ensure the target staging objects are materialized as stable objects (tables/incrementals) where needed to avoid “view rebuilds everything” behavior.

Produce a single command that represents the domain run, e.g.

dbt build --selector stage_claim_medical (or an equivalent selector name aligned to your naming convention)

In Airflow repo (staging env)
Add or modify a staging DAG/task to run the new selector command (parameterized if possible).

Ensure it can be triggered independently without running the full monolithic staging build.

Validation
Implement a lightweight equivalence check between:

current “existing” outputs and

the new “domain-isolated” outputs

Minimum acceptable equivalence checks:

row count parity

schema/column parity (names + types where feasible)

optional: checksum/hash on a stable key (if cheap enough)

Documentation / learnings
Capture “what we learned” (edge cases, required conventions, what broke, how to structure selectors, how orchestration should work).

Produce the template we’ll reuse for subsequent domains.

Out of scope (explicit)
Event-driven triggers (Pub/Sub / ingestion events) — included only as forward-looking design notes

Refactoring all staging domains

Full CI/CD redesign (we are not changing CI strategy yet beyond documenting implications)

Full claims ingestion redesign / removing seeds/config dependencies

Deliverables
Selectors + tags supporting a medical claims staging domain run

Airflow staging DAG/task that runs the selector

Equivalence validation queries/checks recorded somewhere durable (Jira, doc, or repo script)

Runbook snippet:

how to run it locally

how to trigger via Airflow

how to interpret failures

Lessons learned section added to tech spec (or a short addendum)

Acceptance criteria
A single command exists and succeeds:

dbt build --selector <medical_claims_selector>

Airflow staging environment can trigger the selector-run independently.

The isolated run produces outputs that match the current pipeline outputs by the agreed checks:

schema parity (at least column names)

row count parity (within expected tolerance, ideally exact)

and either: stable key hash parity or a sampled record parity check

Failures in this domain run do not require running unrelated staging domains to diagnose (i.e., isolation is operationally meaningful).

A short “POC findings” section is written and includes:

what changed

what assumptions were wrong

what conventions we need for rollout

what follow-up work is required to scale

Implementation notes / suggested tasks (subtasks)
Selector + tagging design (medical claims only)

Materialization audit

identify which staging models are views and would cause implicit full-history rebuild

propose materialization changes needed for the POC

Airflow staging DAG: run selector

Equivalence checks

define which tables are compared

write and run validation queries

Document learnings + propose “standard pattern”

recommended selector naming

how to structure chains / dependencies

how to validate future domains fast

Open questions (to resolve inside this story)
Which specific medical claims slice do we start with (entire folder vs one representative subset)?

Where do we write “new outputs” for comparison (suffix schema, dataset, table suffix like _new)?

What is the cheapest equivalence standard we’ll accept for this POC?

Which materialization changes are required vs “nice to have”?