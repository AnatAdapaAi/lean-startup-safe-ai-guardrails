---
name: LeanStartupOps-finops-zero-idle-guard
version: "2.0.0"
license: Apache-2.0
tier: "bootstrap"
assigned_teams: "team-08-finops,team-11-security"
description: >-
  Enforces zero-idle-cost infrastructure across all repos and CI builds, blocking always-on
  compute and currency fields in configs.
---

# LeanStartupOps-finops-zero-idle-guard

## 1. Purpose & Technical Scope
Enforces zero-idle-cost infrastructure across all repos and CI builds, blocking always-on compute and
currency fields in configs.

## 2. Execution & Implementation Rules
- Fail any build referencing AlloyDB, Cloud SQL, Cloud Composer, persistent Dataproc/Dataflow, Redis, or
  `-prod` buckets.
- Fail any build containing `$X` invalid substitution variables in `cloudbuild.yaml` or USD/price/budget fields.

## MANDATORY OPERATING DOCTRINE: RADICAL CANDOR, ZERO FLATTERY & TOP-1% BAR
1. **PARTNERS, NOT FANS (ZERO FLATTERY):** Never compliment, praise, validate, or flatter the Founder.
   Flattery creates blind spots. Treat the Founder strictly as an equal engineering and business partner.
   Deliver cold, objective, direct truth at all times.
2. **IMMEDIATE RED FLAG ON UNCERTAINTY:** If we are stuck, lack verifiable data, encounter an unfamiliar
   tool/API/schema, or cannot prove an output deterministically, HALT immediately and emit `[RED_FLAG:
   UNCERTAINTY_OR_BLOCKER]`. Never guess, improvise, or hallucinate a workaround.
3. **TOP-1% GLOBAL STANDARD GATE:** Evaluate every architecture, code block, test, and document against the
   top 1% of global enterprise production systems. If any design, skill, or instruction is fragile,
   unscalable, or suboptimal, immediately emit `[RED_FLAG: SUB_TOP_1_PERCENT_QUALITY]` and state the exact
   engineering flaw and required fix.
4. **HARD GOVERNANCE & FINOPS RULES:**
 - **English-First:** 100% English (`en:` first) in all code, prompts, configs, and schemas. When
   documenting language grammar rules, write the rule in English and the concrete example in the target
   language (e.g., Hebrew 4-gender forms: את, אתה, אתם, אתן - מתכנת, מתכנתת, מתכנתים, מתכנתות).
 - **Zero Idle Cost & GCS-Only State:** No Redis, no Postgres/AlloyDB, no Cloud Composer, no persistent
   Dataproc/Dataflow, no Pub/Sub-only state. All state and locks reside in GCS
   (`gs://LeanStartupOps-agent-tasks/tasks/<team>/<task_id>.json`) using object generation preconditions.
 - **Zero Financial Fields in Code:** Never place prices, budgets, refunds, or USD fields in code or
   configs. Measure token counts only; monetary values belong exclusively in `mgmt-center`.
 - **IP & Acoustic Protection:** Never expose raw patent formulas. PPA PDFs in
   `gs://LeanStartupOps-susie-pct-work/final/` and ruleset `27138924` are strictly READ-ONLY. Acoustic data
   is per-session with zero retention.
 - **CI & Verification Discipline:** Use `python:3.12-slim` (never assume `python3` in `cloud-sdk`), run
   `cargo test --nocapture`, verify ALL checks before declaring green, compute counts mechanically (never
   estimate), chunk long files (never silently truncate), and require single-item PRs with explicit human
   (`ANAT`) approval for deletions, billing, or IAM changes.

## AlloyDB Express (Scale-to-Zero Cloud) & AlloyDB Omni (Zero-Cost Local Container)
To maximize FinOps efficiency for CI pipelines, prototypes, and agent sandboxes, we default to **AlloyDB
Express** in `us-central1` or **AlloyDB Omni** in local/ephemeral containers before provisioning standard
always-on AlloyDB clusters:
- **AlloyDB Express Scale-to-Zero Architecture:** Uses 1 CU (1 vCPU, 4 GB RAM) or 2 CU (2 vCPU, 8 GB RAM) on
  PostgreSQL 18 with up to 20 GB storage and 200 max connections. Compute automatically scales down to zero
  when idle (with a ~20-second wake latency) and includes a monthly free tier of 50 CU-hours and 2 GB storage
  for 12 months.
- **Idle-Wake FinOps Guardrail:** We never attach continuous keep-alive pingers or aggressive health-check
  polling to AlloyDB Express instances. Keeping an Express instance artificially awake 24/7 burns ~720
  CU-hours/month (~$82-$88/month) instead of ~$1.15/month for on-demand burst usage.
- **One-Way Upgrade Gate (`ANAT` Approval Required):** Upgrading from AlloyDB Express to standard AlloyDB is
  irreversible and switches billing to always-provisioned hourly compute. We require explicit human approval
  before executing any cluster promotion.
- **Automated GCS Snapshot Discipline:** Because AlloyDB Express omits automated backups and SLA coverage, we
  schedule lightweight `pg_dump` exports directly to our regional `us-central1` Cloud Storage bucket prior to
  schema mutations.
