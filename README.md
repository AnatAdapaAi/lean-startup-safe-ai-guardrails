# Lean Startup Safe AI & User Protection Guardrails

A cloud-agnostic, model-agnostic collection of **37 production AI engineering skills** focused on **protecting users, preventing accidental data loss, blocking untrusted prompt/data ingestion, and enforcing zero-idle FinOps discipline** for bootstrapped and early-stage startups.

---

## Core Architectural Pillars (Cloud & Model Agnostic)

### 1. User Safety & Data Integrity First (Regardless of Cloud or LLM)
- **Zero Accidental Data Loss:** Deterministic pre-execution checks before any destructive storage, database, or repository operation (`accidental-data-loss-prevention`, `public-repo-sanitizer`).
- **Untrusted Ingestion Firewall:** Sanitize third-party webhooks, documents, and user inputs before they enter agent context (`untrusted-ingestion-sanitizer`, `json-schema-contract-testing`, `python-pydantic-schema-validation`).
- **Inclusive & Accessible UX:** Enforce multi-gender localization and web accessibility checks (`i18n-multigender-localization`, `web-a11y-debugging`).

### 2. Universal FinOps & Token Discipline for AI Agents
- **Zero-Idle Compute Guard (`finops-zero-idle-guard`):** Never leave dev workstations, GPU nodes, or orchestrators running when idle. Use scale-to-zero serverless containers and cheap object storage for state snapshots.
- **3-Stage Task-Tiered Model Routing:**
  1. **Stage 1 — Fast Context Pull (Zero Thinking):** Use lightweight/fast models **with thinking disabled (`thinking_budget=0`)** for initial file discovery, routing, and schema mapping.
  2. **Stage 2 — Deep Execution:** Invoke heavy reasoning models **only** on distilled context payloads.
  3. **Stage 3 — Independent Verification Gate:** Run a separate auditor pass (`ml-empirical-validation`, `ml-drift-detection-guard`) before committing outputs.
- **Off-Peak Regional Batching + Cheap Object Storage:** Store intermediate state in low-cost object storage (Parquet/JSON) and schedule heavy batch jobs during the target region's local night hours to unlock up to **40%–50% automatic billing discounts** and avoid `429` retry storms (`exponential-backoff-jitter`, `load-shedding-health-checks`).

---

## Complete Universal Skill Catalog (37 Skills)
- [`skills/accidental-data-loss-prevention.SKILL.md`](skills/accidental-data-loss-prevention.SKILL.md)
- [`skills/agent-otel-observability.SKILL.md`](skills/agent-otel-observability.SKILL.md)
- [`skills/chrome-extension-manifest-v3.SKILL.md`](skills/chrome-extension-manifest-v3.SKILL.md)
- [`skills/data-autocleaning.SKILL.md`](skills/data-autocleaning.SKILL.md)
- [`skills/docker-python-slim-packaging.SKILL.md`](skills/docker-python-slim-packaging.SKILL.md)
- [`skills/enforcing-resource-attribution.SKILL.md`](skills/enforcing-resource-attribution.SKILL.md)
- [`skills/exponential-backoff-jitter.SKILL.md`](skills/exponential-backoff-jitter.SKILL.md)
- [`skills/finops-zero-idle-guard.SKILL.md`](skills/finops-zero-idle-guard.SKILL.md)
- [`skills/git-branch-pr-workflow.SKILL.md`](skills/git-branch-pr-workflow.SKILL.md)
- [`skills/github-actions-gcloud-auth.SKILL.md`](skills/github-actions-gcloud-auth.SKILL.md)
- [`skills/i18n-multigender-localization.SKILL.md`](skills/i18n-multigender-localization.SKILL.md)
- [`skills/json-schema-contract-testing.SKILL.md`](skills/json-schema-contract-testing.SKILL.md)
- [`skills/literature-search-arxiv.SKILL.md`](skills/literature-search-arxiv.SKILL.md)
- [`skills/load-shedding-health-checks.SKILL.md`](skills/load-shedding-health-checks.SKILL.md)
- [`skills/managing-python-dependencies.SKILL.md`](skills/managing-python-dependencies.SKILL.md)
- [`skills/mermaid-architecture-diagrams.SKILL.md`](skills/mermaid-architecture-diagrams.SKILL.md)
- [`skills/ml-best-practices.SKILL.md`](skills/ml-best-practices.SKILL.md)
- [`skills/ml-drift-detection-guard.SKILL.md`](skills/ml-drift-detection-guard.SKILL.md)
- [`skills/ml-empirical-validation.SKILL.md`](skills/ml-empirical-validation.SKILL.md)
- [`skills/notebook-guidance.SKILL.md`](skills/notebook-guidance.SKILL.md)
- [`skills/openapi-spec-generator.SKILL.md`](skills/openapi-spec-generator.SKILL.md)
- [`skills/public-repo-sanitizer.SKILL.md`](skills/public-repo-sanitizer.SKILL.md)
- [`skills/python-ast-static-scanner.SKILL.md`](skills/python-ast-static-scanner.SKILL.md)
- [`skills/python-duckdb-parquet.SKILL.md`](skills/python-duckdb-parquet.SKILL.md)
- [`skills/python-fastapi-cloud-run.SKILL.md`](skills/python-fastapi-cloud-run.SKILL.md)
- [`skills/python-polars-dataframes.SKILL.md`](skills/python-polars-dataframes.SKILL.md)
- [`skills/python-pydantic-schema-validation.SKILL.md`](skills/python-pydantic-schema-validation.SKILL.md)
- [`skills/python-pytest-benchmarking.SKILL.md`](skills/python-pytest-benchmarking.SKILL.md)
- [`skills/python-scipy-signal-processing.SKILL.md`](skills/python-scipy-signal-processing.SKILL.md)
- [`skills/retrieving-developer-knowledge.SKILL.md`](skills/retrieving-developer-knowledge.SKILL.md)
- [`skills/rust-cargo-test-harness.SKILL.md`](skills/rust-cargo-test-harness.SKILL.md)
- [`skills/schema-lineage-mapper.SKILL.md`](skills/schema-lineage-mapper.SKILL.md)
- [`skills/schema-mapping-validator.SKILL.md`](skills/schema-mapping-validator.SKILL.md)
- [`skills/schema-mapping.SKILL.md`](skills/schema-mapping.SKILL.md)
- [`skills/staged-rollout-canary-guard.SKILL.md`](skills/staged-rollout-canary-guard.SKILL.md)
- [`skills/untrusted-ingestion-sanitizer.SKILL.md`](skills/untrusted-ingestion-sanitizer.SKILL.md)
- [`skills/web-a11y-debugging.SKILL.md`](skills/web-a11y-debugging.SKILL.md)

---
*Researched, architected, and field-tested by **Anat Lerner** (@AnatAdapaAi).*
