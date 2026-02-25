# [Replace with a real best-in-class epic]

Drop a completed epic here that represents the quality bar for the team.

The Epic Agent will read this file before drafting to calibrate tone, structure, and level of detail.

Guidelines for a good reference epic:
- Problem statement is crisp and in How Might We format
- Business case is specific — numbers, not adjectives
- Context leads with what already exists before describing what's broken
- Milestones are written as user stories with clear AC
- No placeholder text remains

-- Place holder Epic for Reference PLAT-354
Executive summary

What: Split warehouse builds into independent domain runs using dbt tags/selectors so a failure in one area doesn’t block other domains. 

Why: Today the warehouse runs as a monolithic DAG—one model/test failure stops everything, wasting compute and delaying critical workflows. 

Unlocks Q2: Enables safe partial builds, event-driven runs by domain, and stronger reliability controls (SLOs/ownership) without global instability.

Problem statement
The warehouse currently behaves like a single blast radius. Any code/model failure (bad join, schema mismatch, failed test) blocks unrelated domains from delivering data. 

Goal / outcome
Implement a domain-based tagging taxonomy + selectors and update orchestration so scheduled builds run by domain rather than “everything,” containing failures to the affected area. 

Success criteria (acceptance)

Scheduled builds run using domain selectors, not full builds. 

If a domain fails, only that domain’s downstream dependencies are skipped; other domains continue on schedule. 

Tier-1 domain build time < 15 minutes (or an agreed target) and is reproducible via selectors. 

Metrics

Leading: Tier-1 domain build runtime; % of incidents where other domains still succeed; number of “global” failures.

Lagging: reduced time-to-recovery; reduced compute waste; fewer all-hands incidents. 

Measurement: Airflow task logs; BigQuery audit logs; dbt artifacts. 

Scope
In scope:

Define domain boundaries + tagging taxonomy (domain + tier + schedule)

Create/extend selectors.yml + run groups

Update Airflow DAG(s) to execute isolated selectors instead of monolithic build

Out of scope (not now):

Changing model SQL/business logic

Full repo restructure

Event-driven triggers (post-MVP) 

Plan / milestones

Week 1: agree domain boundaries + tag schema; baseline current runtimes

Week 2: implement tags/selectors + run groups

Week 3: update Airflow orchestration + test in non-prod

Week 4: cutover schedules + document runbook

Dependencies

Airflow DAG access

Ownership of dbt_project.yml

Agreement on domain boundaries 

Risks / open questions

Cross-domain models: do they live in platform/core selector or duplicated dependencies?

How we classify Tier-1 assets per domain (alignment with SLO/ownership epic)

Definition of Done

Selectors are live in production schedules

Failure isolation behavior confirmed with a real incident or simulated failure

Runbook exists: “How to run domain builds / how to triage failures”

Metrics visible (even if minimal: runtime + success rate by domain)
