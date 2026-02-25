# [Replace with a real best-in-class PID]

Drop a completed PID here that represents the quality bar for the team.

The PID Agent will read this file before drafting to calibrate depth, business case framing, and stakeholder communication style.

Guidelines for a good reference PID:
- Problem statement is in How Might We format
- Business case quantifies value — revenue, efficiency, or clinical impact
- Risks include mitigations, not just a list of concerns
- Stays under 4 pages
- Metrics are split into business KPIs and usage metrics


Architecture Context (Read First)
Classification Rules
| Domain Type | Materialization | Snapshot Source? | Rationale |
|-------------|----------------|-----------------|-----------|
| `config` | table | Yes, always | Config changes affect business logic; audit trails required |
| `raw_ingestion` | table | Only if consumer needs history | Most raw data is append-only (claims, events) |
| `live_compute` | view | N/A (no external source) | Consumer requires fresh data |
| `snapshot_candidate` | table | Already SCD-tracked | Changelog structure built-in |


Consumer-Driven Discovery (for non-config decisions)
For decisions beyond the defaults above, justify by what consumers REQUIRE:

WRONG: "This is a view, so it's live_compute"
RIGHT: "Consumer X requires fresh data for real-time dashboard, therefore this must remain a view"
WRONG: "This raw_ingestion source changes, so snapshot it"
RIGHT: "Consumer Y needs point-in-time claims data for audit, therefore source needs snapshot"

This matters because:

It connects staging to business requirements

It prevents over-engineering (don't snapshot raw_ingestion if no one needs history)

It prevents under-engineering (don't materialize as table if consumer needs live data)

It creates accountability (we know WHO breaks if contract changes)

Exception: Config data is ALWAYS snapshotted. Config changes are business logic changes—history is non-negotiable.

Separation of Concerns
┌─────────────────────────────────────────────┐
│             INGESTION LAYER                 │
│        (NOT dbt's responsibility)           │
│                                             │
│  - Schema and mappings applied              │
│  - File uploads → BigQuery                  │
│  - Seeds (TECH DEBT - migrating here)       │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│         STAGING DOMAIN (dbt)                │
│        (transformation only)                │
│                                             │
│  - References sources via source()          │
│  - Transforms to satisfy consumer contracts │
│  - NO file loading                          │
└─────────────────────────────────────────────┘

Seeds are tech debt. They mix ingestion with transformation. The tmp_seeds pattern is a temporary workaround while the ingestion service is being developed. All seeds will eventually be migrated out of this repo.

Repository Context
Work transitions between repos during the POC:

Phase

Repository

Documentation Location

Phase 1 Discovery

Main dbt repo (vida/dbt)

vida/dbt/docs/

Phase 2 Repo Setup

Creates new parallel repo (vida/dbt-staging-poc)

Docs migrate to new repo

Phase 3 Selectors

Parallel repo (vida/dbt-staging-poc)

vida/dbt-staging-poc/docs/

Phase 4 Airflow

Airflow repo (vida/airflow) + parallel dbt repo

Both repos

Phase 5 Validation

Parallel repo (vida/dbt-staging-poc)

vida/dbt-staging-poc/docs/

Phase 6 Merge-Back

Both repos → Main dbt repo

Docs merge to vida/dbt/docs/

Key transitions:

Phase 1 → Phase 2: Documentation created in main repo moves to parallel repo when it's created (TASK-2.1)

Phase 6 → Done: All docs, patterns, and validated code merge back to main repo

Epic-Level (Apply to all tasks)
Scope Phase 1-5 to first-wave domains only: claim_medical, claim_pharmacy, lab, member_enrollment, provider, billing, b2b, metrics.

Comparisons: existing dataset = dbt_stage, new dataset = data_modeling__staging.

Docs: create per-domain folders under docs/<domain>/.

Selectors: use hierarchical tags tag:stage, tag:stage--<domain>, tag:stage--<domain>--<provider>.

Airflow: generalized DAG accepts list of full dbt commands.

All classifications must be justified by consumer requirements, not current implementation.

First-Wave Domains (Scope)
Domain

Model Count (estimate)

Classification (initial assumption)

Notes

claim_medical

94

raw_ingestion

Largest domain

claim_pharmacy

61

raw_ingestion

 

lab

37

raw_ingestion

 

member_enrollment

TBD

raw_ingestion

 

provider

TBD

raw_ingestion

 

billing

TBD

raw_ingestion

 

b2b

45

raw_ingestion

 

metrics

3

live_compute

Must remain views

Task Dependencies (Critical Path)
PHASE 1 (Discovery):
┌─────────────────────────────────────────────────────────┐
│ 1.0 Initialize LLM Tooling Files (DAY 1 - before all)   │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│  1.1 Registry + Consumer Discovery                      │
│  1.2 Live Compute Identification   } Can run in         │
│  1.3 Materialization Audit         } parallel           │
│  1.4 Snapshot Requirements         }                    │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼ (all 4 must complete)
┌─────────────────────────────────────────────────────────┐
│  1.5 Representative Model Selection                     │
│  1.6 Consumer Dependencies (Detailed)  } Sequential     │
│  1.9 Define Explicit Contracts         }                │
│  1.10 Contract Tests                   }                │
└─────────────────────────────────────────────────────────┘
                          │
┌─────────────────────────────────────────────────────────┐
│  1.7 Selector Naming ADR       } Can run in             │
│  1.8 Tagging Taxonomy ADR      } parallel               │
│  1.11 Contract Evolution ADR   } with 1.5-1.10          │
└─────────────────────────────────────────────────────────┘

PHASE 2 (Repo Setup): 2.1 can start early; doc migration happens incrementally
2.1 → 2.2 → 2.3 → 2.4 → 2.5 → 2.6 → 2.7 (sequential)

PHASE 3 (Implementation): Depends on Phase 2 complete
3.1-3.8: 8 domain stories, can run in parallel by domain group

PHASE 4 (Airflow): Depends on Phase 3 complete
4.1 → 4.2 → 4.3 → 4.4

PHASE 5 (Validation): Depends on Phase 4 complete
5.1 → 5.2, 5.3, 5.4 (parallel) → 5.5 → 5.6

PHASE 6 (Documentation): Can start during Phase 5
6.1-6.4 (parallel) → 6.5 → 6.6 (rollback plan) → 6.7, 6.8 (merge incrementally)

Success Criteria
The POC succeeds when:

Isolation proven: First-wave domains build independently without triggering unrelated domains

Validation parity: Representative models match row count + bytes exactly (or have documented exceptions)

Contract tests pass: Consumer-defined contracts are enforced via dbt tests

Airflow operational: Domain DAG triggers successfully in staging environment

No downstream breaks: Intermediate/marts models that consume staging still pass

Build time measured: Baseline established for isolated domain build vs full build

Seed migration path documented: All tmp_seeds have a migration plan to ingestion service

Note: Success Criteria describe the overall POC; Definition of Done below applies per first-wave domain. Domains can be merged incrementally as they meet the Definition of Done—don't wait for all 8.

Definition of Done — First-Wave Domains
Per-domain checklist (repeat for each of 8 domains):

Representative model selected and documented

Domain builds independently with dbt build --select tag:stage--<domain>

Contract tests pass for the representative model

Validation parity met (row count and bytes exactly match)

Exceptions documented

Runbooks created

Documentation Structure
Each domain gets a folder: docs/<domain>/ with these files:

docs/
├── claim_medical/
│   ├── registry.md              # REQUIRED: Domain classification, model list, consumer summary
│   ├── representative.md        # REQUIRED: Which model was selected and why
│   ├── consumer_contracts.md    # REQUIRED: Detailed column-level consumer dependencies
│   ├── live_compute.md          # OPTIONAL: Only if domain has view-only models
│   ├── snapshot_requirements.md # OPTIONAL: Only if domain has snapshot needs
│   ├── exceptions.md            # OPTIONAL: Only if domain has exceptions
│   ├── validation_report.md     # REQUIRED: Parity check results
│   ├── runbook_local.md         # REQUIRED: How to build locally
│   └── runbook_airflow.md       # REQUIRED: How to trigger in Airflow
├── claim_pharmacy/
│   └── ... (same structure)
├── _templates/
│   └── domain/                  # Templates for each doc type
├── adr/
│   ├── 001-selector-naming.md
│   ├── 002-tagging-taxonomy.md
│   ├── 003-contract-evolution.md
│   └── 004-domain-dag-architecture.md
├── seed_migration_plan.md       # Global: all seeds and migration paths
├── rollback_plan.md             # Global: how to revert if POC fails
│
│   # === LLM TOOLING FILES ===
│   # UPDATE CONTINUOUSLY as we learn new patterns.
│
├── style_guide.md               # Naming conventions, patterns, anti-patterns
├── references.md                # Links to key docs, architectural principles
└── skill.md                     # LLM skill definition for Claude Code / Codex agents

LLM Tooling Files (Keep Updated!)
These files enable agentic workflows. Update them as you learn new patterns.

File

Purpose

Update When

style_guide.md

Naming conventions, tag patterns, code patterns, anti-patterns

New convention established, anti-pattern discovered

references.md

Links to main epic, ADRs, external resources (dbt docs, BQ docs)

New reference added, doc location changes

skill.md

Skill definition for LLM agents (context, tools, examples)

Workflow changes, new task types added

Phase 1 — Discovery & Design
Repo: vida/dbt (main repo)

LLM Tooling: Update style_guide.md, references.md, skill.md as you discover patterns

Key principle: Consumer discovery (1.1-1.4) must complete BEFORE representative model selection (1.5). You cannot pick a representative without knowing what consumers require.

TASK-1.0 Initialize LLM Tooling Files
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

TASK-1.1 Registry + Consumer Discovery (First-wave only)
Why: Before we can classify domains or select representatives, we must understand WHO consumes staging and WHAT they need.

Scope: High-level discovery—identify consumers and classify domains. Detailed column analysis happens in TASK-1.6.

Checklist:

For each first-wave domain, list all intermediate/marts models that ref() it

Count total consumers per domain

Classify domain (raw_ingestion, config, live_compute, snapshot_candidate)

Justify classification with consumer requirement

Produce docs/<domain>/registry.md for each domain

Acceptance Criteria: 8 registry.md files created (one per first-wave domain). Each domain has classification + justification. Consumer count documented.

Estimated Effort: 1-2 days (can parallelize across 4 engineers, 2 domains each)

TASK-1.2 Live Compute (Consumer Needs Fresh Data)
Why: A view isn't a cost optimization—it's a contract saying "consumer needs current state when queried."

Checklist:

For each candidate live_compute model, identify the consumer requiring freshness

Document the freshness SLA (implicit or explicit)

Justify with consumer requirement: "Consumer X needs real-time data for Y reason"

Document in docs/<domain>/live_compute.md (only if applicable to this domain)

Flag models that are views but have NO consumer freshness requirement (convert to table)

Acceptance Criteria: live_compute.md created for domains with view-only models (likely just metrics). Each view has a named consumer and freshness justification.

Estimated Effort: 0.5 days

TASK-1.3 Materialization Audit
Audit materialization only for first-wave domains

Note expected impact on row count + bytes parity

Cross-reference with TASK-1.2: live_compute models stay views, others become tables

TASK-1.4 Snapshot Requirements
Rules:

Config sources → ALWAYS snapshot (no justification needed; history is required for audit)

Raw ingestion sources → snapshot only if consumer needs history (justify with consumer requirement)

Checklist:

Identify all config-type sources → mark for snapshot (no discussion needed)

For raw_ingestion sources, determine if ANY consumer needs historical state

Justify raw_ingestion snapshots with consumer requirement: "Consumer X needs point-in-time data for Y reason"

If raw_ingestion source has no consumer history requirement, no snapshot needed

Document in docs/<domain>/snapshot_requirements.md (only if applicable to this domain)

If deferred, document in docs/<domain>/exceptions.md

TASK-1.5 Representative Model Selection
Why: Now that we know what consumers require (1.1-1.4), we can select models that best represent the consumer contract.

Checklist:

Pick representative model closest to union of downstream contracts

Selection must be informed by consumer dependencies from TASK-1.1

Record selection rationale in docs/<domain>/representative.md

Include: Which consumers depend on this model, what columns they use

Selection Criteria:

Has downstream consumers (not orphaned)

Not the simplest model (more than select * from source)

Not the most complex (avoid edge cases)

Representative of domain's typical transformation pattern

Highest consumer overlap score (union-of-contracts coverage)

Acceptance Criteria: 8 representative.md files (one per domain). Each explains WHY this model was selected.

Estimated Effort: 1 day (requires judgment; should involve senior engineer review)

TASK-1.6 Consumer Dependencies (Detailed Column Analysis)
Why: Now that we've selected representative models (TASK-1.5), we need detailed column-level analysis to define contracts (TASK-1.9).

Scope: Deep dive into the representative model's consumers. Which exact columns do they use? How?

Checklist:

For the representative model, identify every consumer

For each consumer, list which columns are actually selected/used

Note usage pattern: join key, filter, aggregation, passthrough

Infer implicit constraints: join key → NOT NULL, identifier → UNIQUE

Document in docs/<domain>/consumer_contracts.md

Acceptance Criteria: 8 consumer_contracts.md files (one per domain). Each file lists: consumer model, columns used, usage pattern, inferred constraints.

Estimated Effort: 2-3 days (can parallelize across 4 engineers, 2 domains each)

TASK-1.7 Selector Naming
Replace selector ADR with hierarchical tag scheme

Document in docs/adr/001-selector-naming.md

TASK-1.8 Tagging Taxonomy
Keep tags minimal for selection; avoid extra dimensions for POC

Document in docs/adr/002-tagging-taxonomy.md

TASK-1.9 Define Explicit Contracts
Why: dbt contracts enforce schema at build time. We define contracts based on consumer requirements discovered in TASK-1.6.

Checklist:

For each representative model, add contract configuration:

models:
  - name: stg_claim_medical__aetna
    config:
      contract:
        enforced: true
    columns:
      - name: claim_id
        data_type: string
        constraints:
          - type: not_null
      - name: member_id
        data_type: string
      # ... columns required by consumers

Include ONLY columns required by consumers (from TASK-1.6)

Add constraints inferred from consumer usage patterns:

Join keys → not_null

Identifiers → not_null (consider unique if consumer assumes it)

Contracts must reflect consumer requirements from TASK-1.6

Document exceptions in docs/<domain>/exceptions.md

Acceptance Criteria:

8 representative models have contract: enforced: true

Each contract includes all consumer-required columns with data types

Constraints match inferred requirements from TASK-1.6

TASK-1.10 Contract Tests
Why: Tests validate data quality beyond schema. Some tests run on staging models, others on downstream models.

Checklist:

Define tests for first-wave representative models:

On staging model (run during staging build):

models:
  - name: stg_claim_medical__aetna
    columns:
      - name: claim_id
        tests:
          - not_null
          - unique
      - name: service_date
        tests:
          - not_null

On downstream model (later) (validates contract from consumer perspective):

models:
  - name: int_claim_medical
    tests:
      - dbt_utils.expression_is_true:
          expression: "claim_id IS NOT NULL"
          config:
            where: "source = 'aetna'"

Test severity = error for contract-critical tests (not warnings)

Document which tests are contract tests vs data quality tests

Tests must cover all constraints defined in TASK-1.9

Acceptance Criteria:

Each representative model has tests for NOT NULL columns

Unique constraints have corresponding unique tests

All tests pass on current data before POC proceeds

TASK-1.11 Contract Evolution
Keep ADR short; define exception handling pattern

Document in docs/adr/003-contract-evolution.md

Phase 2 — Parallel Repo Setup
Repo: Creates vida/dbt-staging-poc (new parallel repo) — all subsequent work moves here

LLM Tooling: Document repo structure decisions, copy patterns from Phase 1

TASK-2.1 Create Repo
Keep repo minimal; avoid nonessential macros/scripts

NO permanent seeds directory (seeds are ingestion, not transformation)

Structure: models/, macros/, contracts/, docs/, scripts/

Copy Phase 1 documentation from vida/dbt/docs/ to new repo's docs/ folder:

All docs/<domain>/ folders created in Phase 1

All ADRs (docs/adr/)

LLM tooling files (style_guide.md, references.md, skill.md)

TASK-2.2 Copy Staging Models
Copy first-wave domains only for POC

TASK-2.3 Copy Macros
Copy only macros required by first-wave domains

TASK-2.4 Seed Dependencies (TECH DEBT)
Why: Seeds mix ingestion with transformation. They're temporarily allowed while the ingestion service is being developed, but ALL seeds must migrate out eventually.

Allow seeds/tmp_seeds for POC (TECH DEBT)

Document each seed's migration path to ingestion service:

Seed name

Which staging models ref it

Target: schema + table in ingestion layer

Load mechanism (GCS upload, API, etc.)

Consumer needs history? (determines if source needs snapshot)

Output: docs/seed_migration_plan.md

Every seed must have a migration plan—no orphan seeds

TASK-2.5 Configure Comparison Dataset
Set target dataset to data_modeling__staging; compare to dbt_stage

TASK-2.6 Remove Invoke/CI Complexity
Strip invoke, CI workflows, unnecessary scripts

Allow only seeds/tmp_seeds (clearly marked as tech debt)

TASK-2.7 Validate Single Model Build
Why: Prove the parallel repo works end-to-end before proceeding to full domain builds.

Checklist:

Run dbt deps — packages install successfully

Run dbt seed --select tag:tmp-seeds — seeds load (if applicable)

Run dbt run --select <representative_model> for ONE domain (e.g., claim_medical)

Verify table created in data_modeling__staging dataset

Verify row count > 0

Verify schema looks correct

Acceptance Criteria: At least one representative model builds successfully. Output is in data_modeling__staging, NOT dbt_stage.

Estimated Effort: 0.5 days

Phase 3 — Domain Isolation Implementation
Repo: vida/dbt-staging-poc (parallel repo)
LLM Tooling: Document tagging patterns, materialization rules, contract enforcement patterns, any anti-patterns discovered

Overview
Phase 3 implements domain isolation by applying tags, setting materializations, configuring partitions, and enforcing contracts on all first-wave domains. Work is organized by domain to enable parallel execution across team members.

Domain Groups
Domains are grouped by source pattern, which determines implementation approach:

Group

Domains

Source

Key Considerations

Fivetran/Bootstrap

claim_medical, claim_pharmacy, lab

Fivetran with mapping seeds

Bootstrap layer applies schema mappings before staging; provider-level tags needed (stage--claim_medical--aetna); largest models requiring partition configs

Database

b2b, provider, member

Direct database via Fivetran

Simpler staging pattern; provider and member share vida folder requiring model-level tagging

Seeds/Bootstrap

billing

Seed files (22 seeds)

Staging models must be created (tech debt); config classification requires snapshots

Live Compute

metrics

Computed views

Must remain views; no partition configs; 3 models only

Work Structure
Create 8 domain stories (one per domain). Each story covers the full implementation for that domain. This enables parallel work while keeping domain-specific context together.

Domain Stories
TASK-3.1 Domain Isolation — claim_medical
Source Pattern: Fivetran/Bootstrap
Model Count: 100 (95 staging + 5 co-located intermediates)
Classification: raw_ingestion
Representative Model: stg_claim_medical__aetna_cvs_health

Context
Claim medical data arrives via Fivetran from payer sources (Aetna, Anthem, BCBS, Cigna, UHC, etc.). Mapping seeds define schema transformations applied in a bootstrap layer. This is the largest domain with provider-level granularity requiring the full tag hierarchy.

Primary consumers: fct_claim_medical, fct_claim_admits, 10 intermediate models, 3 BI exposures.

Implementation Checklist
Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--claim_medical'] in dbt_project.yml
- [ ] Apply provider-level tags on individual models: tag:stage--claim_medical--aetna, etc.
- [ ] Include 5 int_claim_medical__* models co-located in folder
- [ ] Verify: dbt ls --select tag:stage--claim_medical returns 100 models

Materialization:
- [ ] Convert 89 views to tables (see docs/claim_medical/registry.md for list)
- [ ] 11 models already tables — no change needed
- [ ] No live_compute models in this domain

Partition Config:
- [ ] Apply partition on ingest_at field for all table-materialized models
- [ ] Use monthly partitioning: partition_by={'field': 'ingest_at', 'data_type': 'timestamp', 'granularity': 'month'}

Contract Enforcement:
- [ ] Enable contract on representative model: contract: {enforced: true}
- [ ] Add 44 consumer-required columns with data types (see docs/claim_medical/consumer_contracts.md)
- [ ] Add NOT NULL constraints: _file, ingest_at, place_of_service_code
- [ ] Resolve exception: place_of_service_code has 8.9M nulls — investigate claim types and either fix data or relax constraint (see docs/claim_medical/exceptions.md)
- [ ] Add contract tests: not_null for constrained columns

Validation:
- [ ] dbt build --select tag:stage--claim_medical succeeds
- [ ] dbt test --select tag:stage--claim_medical passes (or exceptions documented)
- [ ] Contract tests pass on representative model

Acceptance Criteria
Tags applied and selector returns 100 models

89 view→table materializations complete

Partition configs applied to all tables

Contract enabled on stg_claim_medical__aetna_cvs_health

Exception for place_of_service_code resolved or documented with rationale

References
Registry: docs/claim_medical/registry.md

Consumer contracts: docs/claim_medical/consumer_contracts.md

Representative selection: docs/claim_medical/representative.md

Exceptions: docs/claim_medical/exceptions.md

TASK-3.2 Domain Isolation — claim_pharmacy
Source Pattern: Fivetran/Bootstrap
Model Count: 64 (57 staging + 3 co-located intermediates + 4 other)
Classification: raw_ingestion
Representative Model: stg_claim_pharmacy__aetna_cvs_health

Context
Pharmacy claims arrive via Fivetran from PBMs (CVS, ESI, Optum, etc.). Same bootstrap pattern as claim_medical. Consumers include medication adherence models requiring 180-365 day rolling windows.

Primary consumers: fct_claim_pharmacy, int_claim_pharmacy__cvs, int_claim_pharmacy__caprx.

Implementation Checklist
Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--claim_pharmacy']
- [ ] Apply provider-level tags on individual models
- [ ] Include 3 int_claim_pharmacy__* models co-located in folder
- [ ] Verify: dbt ls --select tag:stage--claim_pharmacy returns 64 models

Materialization:
- [ ] Convert 63 views to tables
- [ ] 1 model already table — no change needed

Partition Config:
- [ ] Apply partition on ingest_at field
- [ ] Monthly granularity

Contract Enforcement:
- [ ] Enable contract on representative model
- [ ] Add consumer-required columns with data types (see docs/claim_pharmacy/consumer_contracts.md)
- [ ] Add NOT NULL constraints per consumer requirements
- [ ] Resolve any exceptions documented in docs/claim_pharmacy/exceptions.md

Validation:
- [ ] dbt build --select tag:stage--claim_pharmacy succeeds
- [ ] dbt test --select tag:stage--claim_pharmacy passes

Acceptance Criteria
Tags applied and selector returns 64 models

63 view→table materializations complete

Contract enabled on representative model

All exceptions resolved

References
Registry: docs/claim_pharmacy/registry.md

Consumer contracts: docs/claim_pharmacy/consumer_contracts.md

Representative selection: docs/claim_pharmacy/representative.md

TASK-3.3 Domain Isolation — lab
Source Pattern: Fivetran/Bootstrap
Model Count: 38 (36 staging + 1 LOINC mapping + 1 co-located intermediate)
Classification: raw_ingestion
Representative Model: stg_lab__aetna_cvs_health

Context
Lab results arrive via Fivetran from payers and lab vendors (Quest). Includes LOINC mapping reference data. Lab results are immutable events — no snapshot requirements.

Primary consumers: fct_lab, fct_lab__aetna, int_lab__aetna_mapped.

Implementation Checklist
Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--lab']
- [ ] Include int_lab__humana co-located in folder
- [ ] Verify: dbt ls --select tag:stage--lab returns 38 models

Materialization:
- [ ] Convert 37 views to tables
- [ ] 1 model already table — no change needed

Partition Config:
- [ ] Apply partition on ingest_at field
- [ ] Monthly granularity

Contract Enforcement:
- [ ] Enable contract on representative model
- [ ] Add consumer-required columns (see docs/lab/consumer_contracts.md)
- [ ] Resolve any exceptions

Validation:
- [ ] dbt build --select tag:stage--lab succeeds
- [ ] dbt test --select tag:stage--lab passes

Acceptance Criteria
Tags applied and selector returns 38 models

37 view→table materializations complete

Contract enabled on representative model

References
Registry: docs/lab/registry.md

Consumer contracts: docs/lab/consumer_contracts.md

TASK-3.4 Domain Isolation — b2b
Source Pattern: Database
Model Count: 45
Classification: raw_ingestion
Representative Model: stg_b2b__accounts_payingorganization

Context
B2B data comes from the application database via Fivetran. Contains mixed data: reference entities (orgs, programs, tiers) and transactional eligibility. One model (stg_b2b__ingest_historicaleligibleuser) must remain incremental.

Primary consumers: dim_paying_organization, fct_eligibility_continuity, fct_eligible_member_health_plan (68 total consumers).

Implementation Checklist
Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--b2b']
- [ ] Verify: dbt ls --select tag:stage--b2b returns 45 models

Materialization:
- [ ] Convert 44 views to tables
- [ ] Exception: stg_b2b__ingest_historicaleligibleuser stays incremental (preserves history accumulation)

Partition Config:
- [ ] Apply partition on appropriate timestamp field (created_at or modified_at)
- [ ] Consider partition strategy for high-volume eligibility tables

Contract Enforcement:
- [ ] Enable contract on representative model
- [ ] Add consumer-required columns (see docs/b2b/consumer_contracts.md)
- [ ] Resolve any exceptions in docs/b2b/exceptions.md

Validation:
- [ ] dbt build --select tag:stage--b2b succeeds
- [ ] dbt test --select tag:stage--b2b passes
- [ ] Verify incremental model behavior preserved

Acceptance Criteria
Tags applied and selector returns 45 models

44 view→table materializations complete

stg_b2b__ingest_historicaleligibleuser remains incremental

Contract enabled on representative model

References
Registry: docs/b2b/registry.md

Consumer contracts: docs/b2b/consumer_contracts.md

Exceptions: docs/b2b/exceptions.md

TASK-3.5 Domain Isolation — member
Source Pattern: Database (vida subset)
Model Count: 4
Classification: raw_ingestion
Representative Model: stg_vida__member

Context
Member data is a subset of the vida staging domain. These 4 models live in models/stage/vida/ alongside provider models, requiring model-level tagging (not folder-level). stg_vida__member has 184 direct consumers — highest-risk model.

Primary consumers: dim_member, 87 intermediate models, 78 marts models (186 total).

Implementation Checklist
Tagging:
- [ ] Model-level tags required (shares folder with provider)
- [ ] Apply tags directly in model config or YAML:
  - stg_vida__member: tags: ['stage', 'stage--member']
  - stg_vida__member_health_condition: tags: ['stage', 'stage--member']
  - stg_vida__member_physicaldevice: tags: ['stage', 'stage--member']
  - stg_vida__payments_membertiereligibility: tags: ['stage', 'stage--member']
- [ ] Verify: dbt ls --select tag:stage--member returns 4 models

Materialization:
- [ ] Convert all 4 views to tables

Partition Config:
- [ ] Apply partition on modified_at or appropriate timestamp
- [ ] Consider partition strategy for stg_vida__member (high consumer count)

Contract Enforcement:
- [ ] Enable contract on stg_vida__member (representative model)
- [ ] High-risk: 184 consumers — contract must be carefully scoped
- [ ] Add consumer-required columns (see docs/member/consumer_contracts.md)
- [ ] Resolve any exceptions in docs/member/exceptions.md

Validation:
- [ ] dbt build --select tag:stage--member succeeds
- [ ] dbt test --select tag:stage--member passes
- [ ] Verify no overlap with tag:stage--provider

Acceptance Criteria
Model-level tags applied and selector returns exactly 4 models

4 view→table materializations complete

Contract enabled on stg_vida__member

No tag overlap with provider domain

References
Registry: docs/member/registry.md

Consumer contracts: docs/member/consumer_contracts.md

Exceptions: docs/member/exceptions.md

TASK-3.6 Domain Isolation — provider
Source Pattern: Database (vida subset)
Model Count: 10
Classification: raw_ingestion
Representative Model: stg_vida__provider

Context
Provider data is a subset of the vida staging domain. These 10 models live in models/stage/vida/ alongside member models, requiring model-level tagging. Includes provider task models (Braze canvas, interventions, work logs).

Primary consumers: dim_provider_history, fct_provider_capacity__live (65 total consumers).

Implementation Checklist
Tagging:
- [ ] Model-level tags required (shares folder with member)
- [ ] Apply tags directly in model config or YAML for all 10 models:
  - stg_vida__provider
  - stg_vida__provider_tasks_brazecanvas
  - stg_vida__provider_tasks_brazecanvasstep
  - stg_vida__provider_tasks_historicalstepchangerequesttask
  - stg_vida__provider_tasks_intervention
  - stg_vida__provider_tasks_interventiontask
  - stg_vida__provider_tasks_interventiontask_applicable_programs
  - stg_vida__provider_tasks_messagetask
  - stg_vida__provider_tasks_stepchangerequesttask
  - stg_vida__provider_tasks_taskworklog
- [ ] Verify: dbt ls --select tag:stage--provider returns 10 models

Materialization:
- [ ] Convert all 10 views to tables

Partition Config:
- [ ] Apply partition on modified_at or appropriate timestamp

Contract Enforcement:
- [ ] Enable contract on stg_vida__provider (representative model)
- [ ] Add consumer-required columns (see docs/provider/consumer_contracts.md)
- [ ] Resolve any exceptions in docs/provider/exceptions.md

Validation:
- [ ] dbt build --select tag:stage--provider succeeds
- [ ] dbt test --select tag:stage--provider passes
- [ ] Verify no overlap with tag:stage--member

Acceptance Criteria
Model-level tags applied and selector returns exactly 10 models

10 view→table materializations complete

Contract enabled on stg_vida__provider

No tag overlap with member domain

References
Registry: docs/provider/registry.md

Consumer contracts: docs/provider/consumer_contracts.md

Exceptions: docs/provider/exceptions.md

TASK-3.7 Domain Isolation — billing
Source Pattern: Seeds/Bootstrap
Model Count: 0 staging models (22 seeds)
Classification: config
Representative Model: stg_billing__configuration_contract (to be created)

Context
TECH DEBT: Billing currently has only seed files — no staging models exist. This task requires creating the staging layer before applying tags, materializations, and contracts.

Billing is classified as config (not raw_ingestion) because these seeds define business rules affecting billing calculations. Config data requires audit trails via snapshots.

Primary consumers: int_billing_configuration, dim_paying_organization_foundation__billing_configuration (19 total consumers).

Implementation Checklist
Create Staging Layer (Tech Debt):
- [ ] Create directory: models/stage/billing/
- [ ] For each of 22 seeds, create corresponding stg_billing__<name>.sql:
  - Example: stg_billing__configuration_contract.sql → select * from {{ ref('seed_billing_configuration__contract') }}
- [ ] Update downstream models to reference staging instead of seeds (future task, not blocking)

Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--billing']
- [ ] Verify: dbt ls --select tag:stage--billing returns 22 models (after creation)

Materialization:
- [ ] All staging models materialize as tables (config domain)

Partition Config:
- [ ] Partition on effective_date where applicable
- [ ] Some config tables are small and may not need partitioning

Contract Enforcement:
- [ ] Enable contract on representative model (stg_billing__configuration_contract)
- [ ] Add consumer-required columns (see docs/billing/consumer_contracts.md)
- [ ] Resolve exception: Some billing seed tests are unverified (seed not materialized in dbt_stage)

Snapshot Requirement (Config Domain):
- [ ] Create snapshots for billing configuration seeds (per AGENTS.md: "Config data is ALWAYS snapshotted")
- [ ] Document in docs/billing/snapshot_requirements.md

Validation:
- [ ] dbt seed --select tag:stage--billing succeeds (if seeds tagged)
- [ ] dbt build --select tag:stage--billing succeeds
- [ ] dbt test --select tag:stage--billing passes

Acceptance Criteria
22 staging models created from seeds

Tags applied and selector returns 22 models

Contract enabled on representative model

Snapshot strategy documented

2 orphaned seeds investigated: seed_billing__products, seed_mondelez_lincoln_tier_3_adjustment

References
Registry: docs/billing/registry.md

Consumer contracts: docs/billing/consumer_contracts.md

Snapshot requirements: docs/billing/snapshot_requirements.md

Seed migration plan: docs/seed_migration_plan.md

Exceptions: docs/billing/exceptions.md

TASK-3.8 Domain Isolation — metrics
Source Pattern: Live Compute
Model Count: 3
Classification: live_compute
Representative Model: stg_all_weights_flagged

Context
MUST REMAIN VIEWS. Metrics domain serves clinical decision-making for CHF rescue interventions. Consumers require query-time freshness — table materialization would introduce unacceptable latency.

Primary consumer: sync_flagged_weights_joined_with_member (clinical workflow).

Implementation Checklist
Tagging:
- [ ] Apply folder-level tag: +tags: ['stage', 'stage--metrics']
- [ ] Verify: dbt ls --select tag:stage--metrics returns 3 models

Materialization:
- [ ] NO CHANGE — all 3 models remain as views
- [ ] Explicitly set materialized: view to prevent accidental override

Partition Config:
- [ ] NOT APPLICABLE — views cannot be partitioned

Contract Enforcement:
- [ ] Enable contract on representative model (stg_all_weights_flagged)
- [ ] Add consumer-required columns (see docs/metrics/consumer_contracts.md)
- [ ] Resolve any exceptions in docs/metrics/exceptions.md

Validation:
- [ ] dbt ls --select tag:stage--metrics returns 3 models
- [ ] Views execute without error
- [ ] Contract tests pass

Acceptance Criteria
Tags applied and selector returns 3 models

All 3 models remain as views

Contract enabled on representative model

Live compute justification documented in docs/metrics/live_compute.md

References
Registry: docs/metrics/registry.md

Live compute justification: docs/metrics/live_compute.md

Consumer contracts: docs/metrics/consumer_contracts.md

Exceptions: docs/metrics/exceptions.md

Phase 3 Summary
Domain

Models

Source Pattern

Tagging

Materialization

Contract

claim_medical

100

Fivetran/Bootstrap

Folder + provider-level

89 view→table

stg_claim_medical__aetna_cvs_health

claim_pharmacy

64

Fivetran/Bootstrap

Folder + provider-level

63 view→table

stg_claim_pharmacy__aetna_cvs_health

lab

38

Fivetran/Bootstrap

Folder

37 view→table

stg_lab__aetna_cvs_health

b2b

45

Database

Folder

44 view→table, 1 incremental

stg_b2b__accounts_payingorganization

member

4

Database (vida subset)

Model-level

4 view→table

stg_vida__member

provider

10

Database (vida subset)

Model-level

10 view→table

stg_vida__provider

billing

22

Seeds/Bootstrap

Folder

22 seed→table

stg_billing__configuration_contract

metrics

3

Live Compute

Folder

No change (views)

stg_all_weights_flagged

Total

286

 

 

249 changes

8 contracts

Dependencies
TASK-3.1 (claim_medical)  ─┐
TASK-3.2 (claim_pharmacy) ─┼─→ Can run in parallel (same Fivetran/Bootstrap pattern)
TASK-3.3 (lab)            ─┘

TASK-3.4 (b2b)            ─┐
TASK-3.5 (member)         ─┼─→ Can run in parallel (same Database pattern)
TASK-3.6 (provider)       ─┘   Note: member and provider require coordination on model-level tagging

TASK-3.7 (billing)        ─→ Independent (requires staging layer creation)

TASK-3.8 (metrics)        ─→ Independent (simplest domain, no materialization changes)

Contract Enforcement Reference
Per ADR 003 (Contract Evolution), all contracts follow these rules:

Change

Classification

Action

Add column

Non-breaking

Merge unilaterally

Remove column

Breaking

Consumer coordination required

Add NOT NULL constraint

Breaking

Data must be clean first

Relax constraint

Non-breaking

Merge unilaterally

Exception Handling Pattern:
1. Document in docs/<domain>/exceptions.md
2. Investigate root cause
3. Either fix data OR relax constraint
4. Enforce only after resolution
5. All exceptions must be resolved before Phase 3 completion

Current Exception Count by Domain:
- claim_medical: 1 (place_of_service_code nulls)
- b2b: 1 (incremental model materialization)
- billing: 2 (unverified seed tests)
- member: TBD
- provider: TBD
- metrics: TBD

Estimated Effort
Domain

Complexity

Effort

Parallelizable With

claim_medical

High (100 models, provider tags)

1-2 days

claim_pharmacy, lab

claim_pharmacy

Medium (64 models, provider tags)

1 day

claim_medical, lab

lab

Medium (38 models)

0.5-1 day

claim_medical, claim_pharmacy

b2b

Medium (45 models, incremental exception)

1 day

member, provider

member

Low (4 models, model-level tags)

0.5 day

b2b, provider

provider

Low (10 models, model-level tags)

0.5 day

b2b, member

billing

High (staging layer creation)

1-2 days

Independent

metrics

Low (3 models, no materialization)

0.25 day

Independent

Total

 

~6-8 days (with 3-4 parallel tracks)

 

Phase 4 — Airflow
Repos: vida/airflow (DAG code) + vida/dbt-staging-poc (dbt commands)

LLM Tooling: Document DAG patterns, command sequences, failure handling patterns

TASK-4.1 DAG Architecture
Generalized DAG accepts list of dbt commands

Document architecture in docs/adr/004-domain-dag-architecture.md

TASK-4.2 Domain Build Script
Script runs a list of dbt commands sequentially

Exit on first failure (fail-fast)

TASK-4.3 First Domain DAG (claim_medical)
Why: Prove the Airflow pattern works with one domain before rolling out to all 8.

Checklist:

Create DAG that runs command list:

dbt seed --select tag:tmp-seeds (TECH DEBT)

dbt build --select tag:tmp-staging (TECH DEBT)

dbt build --select tag:stage--claim_medical
* DAG can be triggered manually in staging Airflow
* DAG completes successfully
* Document that steps 1-2 are temporary while ingestion service is developed

Acceptance Criteria: DAG visible in Airflow UI as dbt_domain__claim_medical. Manual trigger succeeds. All 3 steps complete without error.

Estimated Effort: 1 day

Note: After validating claim_medical, create DAGs for remaining 7 domains.

TASK-4.4 Remaining Domain DAGs
Create DAGs for: claim_pharmacy, lab, member_enrollment, provider, billing, b2b, metrics

Can parallelize across engineers (each engineer takes 2-3 domains)

OR use DAG factory pattern if team prefers (optional)

Each DAG follows same pattern as TASK-4.3

Phase 5 — Equivalence Validation
Repo: vida/dbt-staging-poc (parallel repo)

LLM Tooling: Document validation queries, tolerance patterns, common failure modes

TASK-5.1 Strategy
Compare dbt_stage vs data_modeling__staging

Baseline parity: row count + bytes

No tolerance by default: exact match required unless exception is documented

Bytes definition: BigQuery logical bytes (INFORMATION_SCHEMA.TABLE_STORAGE)

TASK-5.2 Schema Parity
Validate consumer contract columns + types first

Extra columns in new output = OK

Missing columns in new output = FAIL

TASK-5.3 Row Count Parity
Compare row counts for representative models

If counts differ, document exception with rationale in docs/<domain>/exceptions.md

TASK-5.4 Hash Parity
Optional, only when stable primary key exists

Use sampling for large tables

TASK-5.5 Validation Queries
Generate only for first-wave representative models

TASK-5.6 Reporting
Write docs/<domain>/validation_report.md

Document exceptions in docs/<domain>/exceptions.md

Phase 6 — Documentation & Merge-Back
Repo: vida/dbt-staging-poc → merges to vida/dbt (main repo)

LLM Tooling: Final audit (TASK-6.5) — files should already be comprehensive from continuous updates

TASK-6.1 Local Runbook
docs/<domain>/runbook_local.md

TASK-6.2 Airflow Runbook
docs/<domain>/runbook_airflow.md

TASK-6.3 POC Findings
docs/<domain>/poc_findings.md

Include: what worked, what broke, wrong assumptions, conventions needed

Include: seed migration status and blockers

TASK-6.4 Domain Isolation Template
docs/_templates/domain_isolation.md

Reusable checklist for adding next-wave domains

Standardize per-domain doc structure in docs/_templates/domain/

TASK-6.5 Audit LLM Tooling Files
Why: Final check that LLM tooling files are complete before merge-back. These files should have been updated continuously throughout the POC.

Checklist:

Audit docs/style_guide.md:

Has at least 5 documented patterns (with examples)?

Has at least 3 anti-patterns (with "why not" explanations)?

Includes code review checklist?

Audit docs/references.md:

Links to all 4 ADRs?

Links to all domain docs created?

Audit docs/skill.md:

Example prompts are battle-tested (not hypothetical)?

"What agent should/shouldn't do" reflects actual POC experience?

Fill any gaps discovered during audit

Acceptance Criteria: All 3 files are comprehensive (not skeleton/placeholder). No obvious gaps in patterns or references.

This is an AUDIT, not a "write everything at the end" task. If files are incomplete here, the team wasn't updating them throughout the POC.

TASK-6.6 Rollback Plan
Why: Document how to revert BEFORE merging anything. If the POC fails or causes production issues, we need a clear path to revert.

Document rollback procedure:

How to disable domain DAGs in Airflow

How to revert to monolithic build

How to restore original materialization settings

How to clean up data_modeling__staging dataset

Identify rollback triggers:

Validation parity fails (any difference without documented exception)

Downstream models break

Build time regresses significantly

Airflow DAG fails repeatedly

Assign rollback decision owner

Output: docs/rollback_plan.md

TASK-6.7 Merge-Back Checklist
Exclude tmp seeds and tmp staging patterns from merge

Verify success criteria met for domains being merged

Document any tech debt being carried forward

PR reviewed by at least one other engineer

TASK-6.8 Execute Merge
Note: Domains can be merged incrementally as they pass validation. Don't wait for all 8 — merge passing domains for improved velocity.

For each passing domain:

Validation parity confirmed (exact match or documented exception)

Contract tests pass

Runbooks complete

Create PR and merge to main repo

Rollback Triggers (Quick Reference)
Scenario

Action

Validation parity fails (any difference)

Investigate; document exception or fix before merge

Downstream model fails

Rollback domain DAG; debug in parallel repo

Airflow DAG fails 3+ times

Disable DAG; investigate

Build time > 2x baseline

Investigate materialization/partition configs

Contract test fails

Do NOT rollback; fix the contract violation

Contract test fails and blocks production

Rollback if no safe fix exists; document exception and revisit contract

Summary: Task Count & Effort Estimates
Phase

Tasks

Estimated Effort

Parallelizable?

Phase 1 Discovery

12 tasks (1.0-1.11)

5-7 days

Yes (4 tracks)

Phase 2 Repo Setup

7 tasks (2.1-2.7)

2-3 days

Mostly sequential

Phase 3 Domain Isolation

8 tasks (3.1-3.8, one per domain)

6-8 days

Yes (by domain group)

Phase 4 Airflow

4 tasks (4.1-4.4)

2-3 days

Sequential

Phase 5 Validation

6 tasks (5.1-5.6)

2-3 days

Yes (3 parallel)

Phase 6 Documentation

8 tasks (6.1-6.8)

2-3 days

Yes

Total

45 tasks

~20-25 days

 

Team Allocation (3-4 Engineers)
Week 1: Discovery (Phase 1)

Day 1: One engineer creates TASK-1.0 (LLM tooling files) before other work begins

Engineers A+B: TASK-1.1 (split 4 domains each)

Engineer C: TASK-1.2, 1.3

Engineer D: TASK-1.4, ADRs (1.7, 1.8, 1.11)

End of week: All meet to review classifications, select representatives (TASK-1.5)

Checkpoint: Team sign-off on domain classifications before proceeding to Phase 2

Week 2: Contracts + Repo Setup (Phase 1 cont. + Phase 2)

Engineers A+B: TASK-1.6, 1.9, 1.10 (split domains)

Engineers C+D: Phase 2 repo setup (TASK-2.1 can start immediately; copy docs incrementally as Phase 1 produces them)

Checkpoint: Parallel repo builds successfully (TASK-2.7) before proceeding to Phase 3

Week 3: Domain Isolation (Phase 3)

Engineers split domains by group:

A: claim_medical, claim_pharmacy

B: lab, b2b

C: member, provider (coordinate on model-level tagging)

D: billing (staging layer creation), metrics

Checkpoint: All domains pass dbt build --select tag:stage--<domain>

Week 4: Airflow + Validation (Phase 4 + 5)

Engineers A+B: Phase 4 Airflow DAGs

Engineers C+D: Phase 5 validation queries

Checkpoint: claim_medical DAG runs successfully before creating remaining 7 domain DAGs

Week 5: Documentation + Merge (Phase 6)

All engineers: validation queries, reporting, runbooks

Merge domains incrementally as they pass validation

Checkpoint: Rollback plan documented (TASK-6.6) before any merges

Jira Import Checklist
When creating Jira tickets:

Create Epic: "Staging Domain Isolation POC"

Create tasks per phase (see structure below)

Set dependencies using "blocked by" links

Add labels: phase-1, phase-2, etc.

Add labels: first-wave, domain-isolation, tech-debt (for seed tasks)

Assign to sprint based on phase

Link to this document in Epic description

Suggested Jira Structure
Epic: Staging Domain Isolation POC
├── Phase 1: Discovery & Design
│   ├── TASK-1.0: Initialize LLM Tooling Files
│   ├── TASK-1.1: Registry + Consumer Discovery [4 subtasks: 2 domains each]
│   ├── TASK-1.2: Live Compute Identification
│   ├── TASK-1.3: Materialization Audit
│   ├── TASK-1.4: Snapshot Requirements
│   ├── TASK-1.5: Representative Model Selection
│   ├── TASK-1.6: Consumer Dependencies (Detailed)
│   ├── TASK-1.7: Selector Naming ADR
│   ├── TASK-1.8: Tagging Taxonomy ADR
│   ├── TASK-1.9: Define Explicit Contracts
│   ├── TASK-1.10: Contract Tests
│   └── TASK-1.11: Contract Evolution ADR
├── Phase 2: Parallel Repo Setup (7 tasks)
├── Phase 3: Domain Isolation (8 domain stories)
│   ├── TASK-3.1: Domain Isolation — claim_medical
│   ├── TASK-3.2: Domain Isolation — claim_pharmacy
│   ├── TASK-3.3: Domain Isolation — lab
│   ├── TASK-3.4: Domain Isolation — b2b
│   ├── TASK-3.5: Domain Isolation — member
│   ├── TASK-3.6: Domain Isolation — provider
│   ├── TASK-3.7: Domain Isolation — billing
│   └── TASK-3.8: Domain Isolation — metrics
├── Phase 4: Airflow (4 tasks)
├── Phase 5: Equivalence Validation (6 tasks)
└── Phase 6: Documentation & Merge-Back
    ├── TASK-6.1: Local Runbook
    ├── TASK-6.2: Airflow Runbook
    ├── TASK-6.3: POC Findings
    ├── TASK-6.4: Domain Isolation Template
    ├── TASK-6.5: Audit LLM Tooling Files
    ├── TASK-6.6: Rollback Plan (BEFORE any merges)
    ├── TASK-6.7: Merge-Back Checklist
    └── TASK-6.8: Execute Merge (incremental)