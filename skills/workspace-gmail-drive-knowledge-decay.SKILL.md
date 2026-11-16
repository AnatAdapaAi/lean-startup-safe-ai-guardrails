---
name: workspace-gmail-drive-knowledge-decay
description: >
  Enforces our 4-phase FinOps workflow for isolating historical patent and External-Counsel correspondence into a sealed
  zero-read vault (while protecting shared folder PROTECTED_EXTERNAL_SHARED_FOLDER_ID), connecting Gmail alerts
  and finance/admin labels safely, and refreshing decayed Drive research via delta updates rather than from scratch.
version: 1.0.0
category: workspace-finops-governance
---

# Gmail Ingestion, Sealed Patent Vault & Drive Knowledge Decay Governance

## 1. Purpose & FinOps Architecture
We execute a deterministic 4-phase pipeline across Google Drive and Gmail that eliminates redundant token
consumption on historical patent files, protects our live external counsel shared folder, harvests AI failure
alerts at near-zero cost, and refreshes decayed research documents incrementally rather than from scratch.

## 2. Pre-Flight IAM, API & Path Verification
Before running any Drive or Gmail automation, we execute our mandatory pre-flight checks:
1. **Immutable Folder Guardrail Check:** Verify that folder ID `PROTECTED_EXTERNAL_SHARED_FOLDER_ID` (our live
   shared folder with External-Counsel) is loaded in the hard-exclusion list
(`IMMUTABLE_PROTECTED_FOLDER_IDS`). Any tool
   call attempting to move, rename, modify, or alter permissions on `PROTECTED_EXTERNAL_SHARED_FOLDER_ID` or its
   children aborts immediately before hitting the Google Drive API.
2. **API & Model Armor Readiness:** Confirm that `drive.googleapis.com`, `gmail.googleapis.com`, and
   `modelarmor.googleapis.com` are enabled, and that both `untrusted-input-shield` and
   `ip-and-pii-egress-shield` are active in `us-central1`.
3. **Least-Privilege Action Gate:** Block destructive Drive actions (`Trash file`, `Share file`, `Update formulas`)
   unless an explicit `ANAT` approval token is present in the execution context.

## 3. Phase 0 (First Priority): Patent & External-Counsel Correspondence Sealed Vault (`team-00-pct` & `team-06-legal`)
Before any research or organization agent scans our Drive or Gmail, `team-00-pct` and `team-06-legal` run a
zero-LLM metadata sweep ($0 token cost):
- **Zero-Token Metadata Discovery:** Query Drive and Gmail metadata for historical patent drafts (`IP-Formula`,
  `IP-Formula`, `PCT`, `Provisional`, `Claims`) and past correspondence with External-Counsel, explicitly
excluding parent folder
  `PROTECTED_EXTERNAL_SHARED_FOLDER_ID`.
- **One-Time Consolidation into Sealed Vault:** Move all superseded patent drafts and exported historical
  correspondence threads into a single sealed cold-storage folder named
  `00_SEALED_PATENT_AND_External-Counsel_HISTORY_NO_AGENT_READ`.
- **Permanent Agent Read Exclusion:** Stamp the sealed vault folder ID into our manifest with
  `agent_read_access: DENY`. No agent across our 22 teams ever reads, embeds, or summarizes files inside this
  sealed vault again, saving thousands of tokens per run and preventing outdated legal drafts from polluting
  active patent claims.

## 4. Phase 1: Gmail Intelligence Alerts & Newsletters Harvester (`team-10-research` & `team-09-eval`)
We connect our AI failure alerts and technical newsletters to our research agency without exposing personal
inbox threads:
- **Label-Scoped Polling:** Poll only messages under the `Enterprise AI/Intel-Alerts` Gmail label that have not yet
  been recorded in our `PROCESSED_GMAIL_IDS.json` ledger.
- **Inline Model Armor Sanitization:** Pass incoming newsletter bodies through `untrusted-input-shield`
  to neutralize indirect prompt injection and malicious links within our 2M/month free token tier.
- **Zero-Thinking Extraction:** Use `gemini-3.8-flash` with `thinkingBudget: 0` ($0.50/1M tokens) to extract
  structured JSON records of (a) newly reported AI silent failures, (b) FinOps optimization techniques, and
  (c) recommended skill updates into GCS.

## 5. Phase 2: Finance & Administration Gmail Workflow (`team-07-finance` & `team-05-admin`)
We connect `team-07-finance` and `team-05-admin` to Gmail under strict egress and privacy controls:
- **Scoped Labels & SDP Redaction:** Read only `Enterprise AI/Finance-Invoices` and `Enterprise AI/Admin-Ops`
  through our
  `ip-and-pii-egress-shield` (Sensitive Data Protection enabled).
- **Draft-Only Enforcement:** Extract invoice totals, vendor metadata, and deadlines into our structured finance
  ledger, and prepare Gmail replies strictly as **Drafts** (`users.drafts.create`). Direct email transmission
  (`users.messages.send`) remains hard-blocked until human review.

## 6. Phase 3: Deep-Content Drive Triage & Incremental Research Refresh (`team-19-cos` & `team-10-research`)
To fix superficial folder-only organization and rehabilitate decayed research without starting from zero:
1. **Single-Pass Content Fingerprinting (`gemini-3.8-flash`, `thinkingBudget: 0`):** Read the actual document
   body once, compute its SHA-256 hash, assign it to one of our 22 teams, and score its `Rot Level`:
   - `ACTIVE_CANONICAL`: Current and ready for production use.
   - `NEEDS_DELTA_REFRESH`: Valuable core research whose model benchmarks, pricing, or citations predate our
     current 2026 baseline.
   - `SUPERSEDED_ARCHIVE`: Duplicate or obsolete draft to move into cold archive.
2. **Targeted Delta Research (`gemini-4-carbon`):** For documents marked `NEEDS_DELTA_REFRESH`, we preserve 100%
   of the valid foundational analysis and generate a concise `_DELTA_2026_UPDATE.md` companion file covering
   only what changed since the original document was written.
3. **Audited Physical File Placement (`gemini-4-argon`):** Execute the verified Drive file moves (`files.update`
   with `addParents`/`removeParents`) so every file resides in its canonical team folder with an active workflow
   index.
