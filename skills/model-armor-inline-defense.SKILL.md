---
name: model-armor-inline-defense
description: >
  Configures and enforces Google Cloud Model Armor inline protection across Agent Runtime, MCP servers,
  Apigee, and load balancers with tiered placement strategy and FinOps token budgeting.
version: 1.0.0
category: security-and-governance
---

# Model Armor Inline Defense & Tiered Placement Architecture

## 1. Purpose & Technical Scope
We use Google Cloud Model Armor to provide inline, zero-app-change protection across our agent interaction
paths - including Vertex AI Agent Runtime, Gemini Enterprise, MCP tool servers, Apigee, and GKE Ingress/Load
Balancers. Rather than applying identical heavy filtering to every internal agent, we enforce a tiered
placement architecture that maximizes security at untrusted boundaries while preserving zero false-positives
and lean FinOps token consumption inside trusted engineering sandboxes.

## 2. Pre-Flight IAM, API & Path Verification
Before provisioning or attaching Model Armor templates, we execute our mandatory pre-flight verification:
1. **API Readiness Check:** Confirm that `modelarmor.googleapis.com`, `dlp.googleapis.com` (Sensitive Data
   Protection), and `aiplatform.googleapis.com` are enabled in our target project.
2. **IAM Role Verification:** Verify that our administration identity holds `roles/modelarmor.admin` and that
   the runtime service agent holds `roles/modelarmor.user` and `roles/dlp.user`.
3. **Regional Alignment Check:** Confirm that the Model Armor template region (for example, `us-central1`)
   matches the exact region of our deployed Reasoning Engines and Agent Gateways to prevent cross-region
   network latency and egress charges.

## 3. Where to Deploy Model Armor (3-Tier Agent Placement Matrix)
We classify every agent team into one of three operational tiers before binding a Model Armor template:

| Placement Tier | Target Agent Workloads | Recommended Template Profile | Architectural Rationale |
| :--- | :--- | :--- | :--- |
| **Tier 1: Untrusted Ingestion Perimeter** | External research, customer support, sales assistants, workspace connectors (email, calendar, shared docs, web/MCP) | `untrusted-input-shield` (Prompt Injection + Jailbreak + Malicious URLs + PDF/Doc scanning ENABLED) | Blocks indirect prompt injection embedded inside third-party emails, web pages, calendar invites, and uploaded documents before they reach the model. |
| **Tier 2: Sensitive Egress & Compliance** | Legal, compliance, finance, HR, and executive reporting agents | `sensitive-egress-sdp-shield` (Sensitive Data Protection PII redaction + High-confidence Jailbreak ENABLED) | Prevents accidental leakage of PII, financial identifiers, or confidential records in outgoing agent responses and tool payloads. |
| **Tier 3: Trusted Internal Engineering** | Core software engineering (Rust/Python/Wasm), security auditing, red-team evaluation, and FinOps automation | Bypass inline blocking (or attach in `INSPECT_ONLY` audit mode) | Code and security agents routinely process exploit signatures, raw stack traces, and cryptographic primitives that trigger false-positive content blocks. |

## 4. FinOps Cost Discipline & Token Budgeting
- **Leveraging the Monthly Free Tier:** Model Armor provides the first 2,000,000 tokens per month at zero cost,
  and charges a lean $0.10 per 1,000,000 tokens thereafter (where 1 token equals 4 non-whitespace characters).
- **Zero-Cost SDP & Document Scanning:** Enabling Sensitive Data Protection (PII redaction), malicious URI
  detection, and inline PDF/document scanning within Model Armor incurs zero surcharge beyond the base token
  rate.
- **Avoiding Redundant Double-Scanning:** When an upstream Agent Gateway or Load Balancer already enforces an
  inline Model Armor template on incoming requests, we do not re-invoke the standalone `sanitizeUserPrompt` REST
  API inside the downstream agent code.

## 5. Reference Provisioning Workflow
We provision regional templates deterministically using the regional endpoint
`https://modelarmor.<REGION>.rep.googleapis.com/v1/projects/<PROJECT_ID>/locations/<REGION>/templates`:
- Configure `piAndJailbreakFilterSettings` with `filterEnforcement: ENABLED` and `confidenceLevel: MEDIUM_AND_ABOVE`
  for Tier-1 untrusted ingestion agents.
- Configure `maliciousUriFilterSettings` with `filterEnforcement: ENABLED` on all agents browsing external URLs
  or parsing incoming emails.
- Configure `sdpSettings.basicConfig` with `filterEnforcement: ENABLED` on Tier-2 legal and financial agents to
  redact sensitive identifiers automatically before output delivery.
