# [Replace with a real best-in-class one-pager]

Drop a completed one-pager here that represents the quality bar for the team.

The One-Pager Agent will read this file before drafting to calibrate tone, length, and structure.

Guidelines for a good reference one-pager:
- Fits on one page (~500 words or less)
- Leads with the problem, not the solution
- Includes at least one data point in the opening
- Ends with a specific, actionable ask
- Raises more questions than it answers — this is alignment, not a spec

Title: Isolate Warehouse Failures by Domain
Owner(s): PM David K, Tech Lead Chris K
Date: 2026-01-07
CORE
Executive Summary
What: Split warehouse builds into independent domain selectors so a model failure in one area doesn't block builds in others.
Why: When a claims model breaks, labs, eligibility, and billing all wait. One error in one domain wastes hours of compute and delays all data delivery.

Dimension
Score
Rationale
Impact
8
Core fix for "one bad source blocks everything." Moves blast radius from ~80% to ≥95% isolated.
Confidence
8
Well-understood pattern, problem clearly diagnosed.
Effort
4
Multiple systems, careful coordination required.
ICE
256




Why This Matters
Problem: The warehouse builds as a single monolithic DAG. Any model error—a bad join, a schema mismatch, a failed test—stops everything. Teams sit idle waiting for unrelated code to get fixed.
This is about code/model failures during the build process.
Business Impact:
• Operational Efficiency: Reduce wasted compute by ~60% when partial failures occur
• Time to Recovery: Other domains continue delivering data during incidents
• Blast Radius: Move from ~80% affected to ≥95% isolated
Urgency: Moderate
What We're Building
In one sentence: Tag-based selectors and run groups that let each domain build independently, with failures contained to the affected area.
The approach: Implement a tagging taxonomy (domain + tier + schedule) in dbt_project.yml. Create selectors.yml with run groups for each domain. Update Airflow DAGs to use isolated selectors instead of full builds.
User/system changes:
• Scheduled builds run by domain selector, not full DAG
• Failures skip only downstream dependencies within the affected domain
• Other domains continue building on schedule
What we're NOT doing (for now):
• Changing model SQL or business logic
• Full repo restructure
• Event-driven triggers (post-MVP)
Timeline & Dependencies
Dates: Start [Week 1] → Complete [Week 4]
Dependencies: Airflow DAG access, dbt_project.yml ownership, agreement on domain boundaries
Success Metrics
Leading (immediate): Tier-1 domain builds complete in <15 minutes; failures isolated to single domain; other domains unaffected during incidents
Lagging (long-term): Reduced time-to-recovery; decreased compute waste; fewer all-hands-on-deck incidents
Measured by: Airflow task logs, BigQuery audit logs
