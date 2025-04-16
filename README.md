# senior-engineering-partner

Last updated: 2026-10-06 12:21 AM CDT

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Latest release](https://img.shields.io/github/v/release/bjgreenberg/senior-engineering-partner?sort=semver&label=release)](https://github.com/bjgreenberg/senior-engineering-partner/releases)
[![docs-render](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/docs-render.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/docs-render.yml)
[![leakage-guard](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/leakage-guard.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/leakage-guard.yml)
[![shellcheck](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/shellcheck.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/shellcheck.yml)
[![skill-lint](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/skill-lint.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/skill-lint.yml)
[![actions-lint](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/actions-lint.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/actions-lint.yml)
[![script-tests](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/script-tests.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/script-tests.yml)
[![citation-validate](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/citation-validate.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/citation-validate.yml)
[![plugin-validate](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/plugin-validate.yml/badge.svg?branch=main)](https://github.com/bjgreenberg/senior-engineering-partner/actions/workflows/plugin-validate.yml)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/bjgreenberg/senior-engineering-partner/badge)](https://scorecard.dev/viewer/?uri=github.com/bjgreenberg/senior-engineering-partner)
[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/13458/badge)](https://www.bestpractices.dev/projects/13458)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://www.conventionalcommits.org/en/v1.0.0/)

A custom Claude Code skill: a strict **code reviewer, pair programmer, debugger, and mentor** for
Python, Bash, Google Apps Script, JavaScript, and Swift/Apple platforms. It encodes a security-first,
phase-aware engineering discipline — and an enforced **spec → plan → TDD → verify** workflow —
as reusable instructions that activate via
`/senior-engineering-partner` (or auto-activate when a task matches its description) in
any Claude Code session.

> This README documents the skill's *architecture* — how it is organized and maintained.
> The skill's actual instructions live in [`SKILL.md`](SKILL.md); the deep, per-topic
> standards live in [`references/`](references/).

- **Author:** Brian Greenberg · **Web:** https://briangreenberg.net
- **Version:** see the metadata table at the bottom of [`SKILL.md`](SKILL.md), the
  [`CHANGELOG.md`](CHANGELOG.md), and the
  [Releases](https://github.com/bjgreenberg/senior-engineering-partner/releases) page
- **Invoke:** `/senior-engineering-partner` in Claude Code, optionally prefixed with a
  mode trigger word (see [Modes](#modes--triggers)).

## Contents

- [What it is](#what-it-is)
- [What it governs](#what-it-governs)
- [Architecture](#architecture)
- [Modes & triggers](#modes--triggers)
- [The rigor ladder](#the-rigor-ladder)
- [Reference catalog](#reference-catalog)
- [Shipped helpers & evals](#shipped-helpers--evals)
- [Install](#install)
- [Using it with other AI tools (Codex, Gemini CLI, …)](#using-it-with-other-ai-tools-codex-gemini-cli-)
- [Customize for your environment (my-environment.md)](#customize-for-your-environment-my-environmentmd)
- [Maintaining / contributing](#maintaining--contributing)
- [Citing this repository](#citing-this-repository)
- [License](#license)
- [Disclaimer](#disclaimer)

---

## What it is

A single skill that does the heavy lifting of senior engineering work — design, write,
test, review, debug, and document code — calibrated to the experience level you set in your
environment profile. Three ideas run through everything:

- **Phase-aware rigor, with a security floor that never moves.** Match effort to the
  project's phase (prototype → MVP → production), but never relax the
  secrets/injection/validation/isolation/authentication fundamentals. *Cheap ≠ insecure.*
- **Deterministic-first, anti-hallucination discipline.** Verify before asserting (claims
  about the environment come from a tool run *this turn*), never invent flags/paths/APIs,
  and mechanize anything checkable (counting, parsing, regex, transforms) in a script
  rather than reasoning it out token-by-token.
- **An enforced workflow, not just standards.** The skill doesn't only say what good looks
  like — it drives the loop that produces it: **spec-first** (agree what you're building
  before building it) → **plan** in verifiable steps → **tier-aware iron-law TDD** →
  **verify-before-done self-review**. Depth scales with the rigor tier; the loop does not.

<sub>[↑ Back to contents](#contents)</sub>

---

## What it governs

The disciplines are stack-agnostic, but they bind to concrete tooling. At a glance, what the skill
carries standards for:

- **Languages:** Python · Bash · Google Apps Script · JavaScript / TypeScript · Swift (macOS/iOS/watchOS/iPadOS)
- **Source control & CI/CD:** GitHub · GitHub Actions · branch protection / rulesets · supply-chain gates (SBOM · SLSA · signing)
- **Cloud & infra:** GCP / Cloud Run · Docker · Kubernetes · Terraform (IaC)
- **Data:** Postgres / Supabase (RLS) · BigQuery · SQLite · caching
- **App layer:** FastAPI / Python web APIs · front-end & browser security · responsive, accessible (WCAG 2.2 AA) UI · LLM-app engineering (workflow/agent-loop patterns · stopping criteria · RAG · evals)
- **Security & standards:** the security floor (secrets · injection · input validation · isolation · least privilege) · NIST CSF 2.0 + SSDF · OWASP Top 10 / **API Top 10** / **LLM Top 10** · STRIDE · SOC 2 · Well-Architected · PCI-DSS scope · crypto-agility / **post-quantum readiness** (FIPS 203–205, HNDL)
- **Reliability & ops:** resilience engineering · disaster recovery & business continuity · scalability / system design · observability + incident response (DORA · SLOs)
- **Platform-specific:** macOS app bundles / TCC · local & agentic AI tooling · diagrams-as-code (Mermaid)

Each binds to a deep, **read-on-demand** reference (see the [catalog](#reference-catalog) below); your
concrete hosts, projects, and stack live only in the private, un-committed `references/my-environment.md`.

<sub>[↑ Back to contents](#contents)</sub>

---

## Architecture

The skill is a **stack-agnostic universal core** (`SKILL.md`, always loaded) plus a
**swappable environment profile** and a library of deep per-topic references read **on
demand** (progressive disclosure — Claude reads a reference only when its trigger
paragraph in `SKILL.md` says the work is relevant). Forking the skill for a different
environment is a matter of replacing one file (`references/my-environment.md`).

```mermaid
flowchart TD
    U["/senior-engineering-partner"] --> C
    C["SKILL.md — universal core<br/>modes · epistemic discipline · engineering workflow · rigor ladder<br/>security floor · coding standards · toolchain triggers"]
    C -->|"progressive disclosure: read a reference only when relevant"| R[(references/)]
    C -.->|"shipped helpers"| K["scripts/ (audit · render-diagrams · validate-citation · leakage-guard · actions-lint · run-evals · eval-guard · curate-baseline · skill-lint · self-review · fixture tests)<br/>evals/ (regression scenarios + recorded baselines)"]
    R --> P["Environment profile<br/>my-environment.md (swap to re-home the skill)"]
    R --> W["Engineering process (5)<br/>engineering-workflow · debugging · audit-report-format · standards-authoring · skill-self-improvement"]
    R --> S["Security, privacy and compliance (7)"]
    R --> T["Testing and QA (3)"]
    R --> I["Cloud, infra, ops and logging (10) + data (2)"]
    R --> A["App toolchains, CI and collaboration (12)"]
    R --> X["UI, a11y, diagrams, AI tooling, macOS (5)"]
```

`SKILL.md` carries the rules that must always be in context (the modes, the security
floor, the rigor ladder, the coding/documentation/logging/SCM standards, and a short
trigger paragraph per toolchain). Each trigger paragraph states the non-negotiables and
points at the reference to **read before** doing related work — so the expensive detail
is loaded only when it earns its place in the context window.

<sub>[↑ Back to contents](#contents)</sub>

---

## Modes & triggers

Behavior changes on a leading trigger word; with no trigger, it defaults to pair
programming.

```mermaid
flowchart TD
    P[User prompt] --> Q{Leading trigger word?}
    Q -->|"REVIEW:"| R["Strict senior code reviewer<br/>critique rigorously, then deliver the refactor"]
    Q -->|"EXPLAIN:"| E["Patient mentor<br/>teach the why, not just a copy-paste answer"]
    Q -->|"MVP: / PROTOTYPE:"| M["Lean-but-safe builder<br/>Tier 0/1, defer heavy gates, never the floor"]
    Q -->|"DEBUG:"| G["Systematic debugger<br/>reproduce, isolate, fix root cause, prove with a red-first test"]
    Q -->|"AUDIT:"| A["Report-first codebase auditor<br/>severity-ranked findings report; fixes only after review"]
    Q -->|none| D["Collaborative pair programmer (default)<br/>clean, tested, documented, production-ready code"]
```

| Trigger | Mode | What it does |
|---|---|---|
| *(none)* | **Pair programmer** | Do the work — production-ready code with tests + docs, concise explanation. |
| `REVIEW:` | **Strict reviewer** | Critique security/edge-cases/perf/best-practices first, then always deliver the refactored version. |
| `EXPLAIN:` | **Mentor** | Educate step-by-step, calibrate to the user's level, prioritize understanding. |
| `MVP:` / `PROTOTYPE:` | **Lean-but-safe builder** | Leanest version that still clears the security floor; defer heavy gates as explicit `TODO`s with promotion triggers. |
| `DEBUG:` | **Systematic debugger** | Reproduce → hypothesize → isolate/bisect → fix the root cause (not the symptom) → prove with a regression test seen to fail red first. |
| `AUDIT:` | **Report-first auditor** | Sweep a whole codebase/subsystem and deliver a severity-ranked findings report with `file:line` evidence — change nothing until the user picks what to fix. |

<sub>[↑ Back to contents](#contents)</sub>

---

## The rigor ladder

Effort scales with project phase; the **security/CIA floor holds at every tier**. Only
verification depth, redundancy, and operational maturity scale.

```mermaid
flowchart LR
    T0["Tier 0 — Prototype<br/>throwaway, never real tenant data"]
    T1["Tier 1 — MVP / early product<br/>critical-path tests, basic CI, secrets manager, authn, backups"]
    T2["Tier 2 — Production / commercial / multi-tenant<br/>full strict posture, every merge-blocking gate"]
    Floor["Security / CIA floor — CONSTANT at every tier<br/>no hardcoded secrets · validate inputs · no injection · isolated env · authn · vetted deps"]
    T0 -->|"real users / small scale"| T1
    T1 -->|"customers · money · multi-tenant · PII · 2nd contributor · public exposure"| T2
    Floor -.underpins.-> T0
    Floor -.underpins.-> T1
    Floor -.underpins.-> T2
```

Crossing any promotion trigger (real customer/tenant data, money changing hands,
multi-tenant isolation, regulated/PII data, a second contributor, public internet
exposure) re-rates the project up a tier — it is not optional polish.

<sub>[↑ Back to contents](#contents)</sub>

---

## Reference catalog

Deep standards, read on demand. Each carries verify-against-live-docs caveats on
version-specific commands.

| Group | Reference | Covers |
|---|---|---|
| **Environment profile** | `my-environment.md` | The concrete stack/hosts/repos/house-Git-standards — the one file to swap when forking the skill |
| **Engineering process** | `engineering-workflow.md` | The spec → plan → tier-aware iron-law TDD → verify-before-done self-review loop |
| | `debugging.md` | Systematic root-cause method (the `DEBUG:` mode): read the logs first, then reproduce → hypothesize → isolate → fix cause → red-first regression test |
| | `audit-report-format.md` | The `AUDIT:` mode deliverable: a severity-ranked findings report (finding schema, severity taxonomy, mechanize-the-checkable, lead-with-verified-strengths) |
| | `standards-authoring.md` | Distill sprawling project conventions into a checkable standards set (extract → filter → human-approve → classify floor-vs-overridable); prose-first, format-agnostic |
| | `skill-self-improvement.md` | The consent-gated loop's full procedure: classify (pattern / one-off / irreversible-cost), the three-part proposal package (rule + guarding eval + origin story), ship-through-PR, never-relax, the non-maintainer path |
| **Security, privacy & compliance** | `threat-modeling-and-api-design.md` | In-PR STRIDE threat models + attack-surface-shrinking API design |
| | `secure-data-processing.md` | Hostile-file parsing, prompt-injection fencing (two-zone worked example), RAG/vector-store isolation, multi-tenant data handling |
| | `frontend-web-security.md` | Token storage, CSP, output sanitization, security headers |
| | `secrets-and-key-rotation.md` | Rotation lifecycle, zero-downtime overlap, KMS key-version re-wrap |
| | `action-identity-and-audit.md` | Every action identified, signed, recorded: threat model, enforce-not-record, the agent's own code-host identity, a session-id trailer (never a host name on a public artifact), hash-chained off-host action log, provenance, drills, fleet conformance gate |
| | `data-protection.md` | GDPR/UK-GDPR/CCPA as code: DSAR, erasure cascade, retention, DPIA |
| | `compliance.md` | NIST CSF 2.0 + **SSDF (800-218)** / OWASP / SOC 2 / **Well-Architected** as enforceable review checklists, incl. crypto-agility + **post-quantum readiness** (FIPS 203–205, harvest-now-decrypt-later triage) |
| | `agentic-ai-security.md` | Products that ARE agents: least agency, human-in-the-loop on the resolved call, memory as a poisoning surface, OWASP Agentic Top 10 + CSA MAESTRO mapping |
| **Testing & QA** | `testing.md` | The enforced merge-gate taxonomy, tenant-isolation tests, the monitor→comment→block gate-introduction ladder, coverage/mutation/load tiers, frontend testing (behavior-not-implementation, network-boundary mocks, E2E/a11y gates) |
| | `testing-single-file.md` | The `conftest.py` argv-patch pattern for single-file scripts |
| | `maintainability-metrics.md` | Complexity (cognitive, not cyclomatic) + structural duplication as committed-threshold gates; legacy ratchet; tool bindings incl. the qlty BUSL license note |
| **Cloud, infra & ops** | `gcp.md` | Cloud Run, GCS, BigQuery, Secret Manager, IAM (no SA keys → Workload Identity) |
| | `iac-terraform.md` | Terraform on GCP, locked remote state, OIDC deployer, plan-as-gate |
| | `containers-and-orchestration.md` | Docker/Kubernetes: digest pins, non-root, scanning, securityContext |
| | `observability-and-incident-response.md` | Structured logs + correlation id, RED/USE metrics, SLO burn-rate alerting + severity-routed channels, client-side/RUM monitoring, incident lifecycle |
| | `disaster-recovery.md` | 3-2-1-1-0 immutable backups (Bucket Lock, not just versioning), out-of-domain copies, verified PITR, scheduled restore drills, local/sync-≠-backup |
| | `business-continuity.md` | BIA → justified RTO/RPO, provider-outage plans, comms/decision plan, the solo-operator/bus-factor path |
| | `resilience-engineering.md` | Degrade-don't-die in code: timeouts, circuit breaker, bulkhead, load-shed, designed degraded modes, kill-switch |
| | `scalability-and-system-design.md` | The "-ilities": statelessness for horizontal scale, queue+worker, DLQ, transactional outbox, the pool/N+1/hot-partition ceilings, capacity & perf targets |
| | `logging-and-monitoring.md` | Reading the logs as a Definition-of-Done gate (subsystem-scoped *and* not-subsystem-scoped, never-report-clean-without-the-command, empty ≠ clean), structured logging in Python (JSON + `contextvars` correlation id, per-stack loggers), log location/rotation, the launchd open-fd gotcha, unattended-job monitor design |
| **Data** | `databases.md` | Postgres/Supabase RLS (+ pgTAP), BigQuery, SQLite, migrations |
| | `caching.md` | Cache-key-must-encode-the-tenant, invalidation, what-not-to-cache |
| **App toolchains, CI & collaboration** | `python-web-apis.md` | FastAPI/Uvicorn/psycopg: lifespan, Pydantic, auth-as-`Depends`, RLS pipeline |
| | `github-actions.md` | Least-priv `permissions`, SHA-pinned actions, multi-gate pipelines (audit/typecheck/lint), the Swift/Apple job shape (macOS runners, pinned resolution, ASC-API-key signing), SBOM + build-provenance attestation, gated deploy + canary + release automation |
| | `github-teams.md` | Team-grade repo hygiene (required gates, CODEOWNERS, review every agent PR) |
| | `package-managers.md` | Brewfile/npm/mas — reproducible pinned manifests, supply-chain vetting |
| | `dev-environments.md` | VS Code/Xcode/Antigravity hygiene, extension vetting, signing |
| | `dev-environment-isolation.md` | Never dev against prod, per-project venv/container, sandbox untrusted code |
| | `foss-adoption.md` | Vet FOSS before adopting (license/Scorecard/CVEs) + pin/lock/contract-test |
| | `multi-agent-coordination.md` | The concurrency override when >1 writer shares a repo |
| | `python-typing-and-packaging.md` | The TypedDict worked example + the single-file→package target layout |
| | `google-apps-script.md` | `clasp` + git over the editor, minimal `oauthScopes`, `PropertiesService` secrets/limits, `LockService`, trigger quotas + the 6-min wall, Advanced Services vs `UrlFetchApp`, `console`→Cloud Logging, pure-logic isolation for testing |
| | `javascript-and-typescript.md` | TS strict mode (the `mypy --strict` analog) + the flags `strict` misses, runtime-validated typed boundaries (the Pydantic analog), Node `SIGTERM`/no-floating-promises patterns |
| | `bash-scripting.md` | Strict mode's documented gaps (`-e` suspension, masked substitutions), traps/atomic output/locks, `curl -f`, stock-bash-3.2 portability, BATS + command stubs |
| | `swift-apple-development.md` | XcodeGen `project.yml` as source of truth, SwiftPM pure-logic packages, headless provisioning, never-store-ticks state design, the `CKSyncEngine` hard rules, Swift 6 concurrency field notes, `log stream`/`.ips` diagnosis — plus the enforcement lane: SwiftLint/`swift format`/compiler gates, committed `Package.resolved` + `osv-scanner` audit, the Apple security-floor bindings (Keychain, sandbox/entitlements, ATS, privacy manifests, entry-surface validation), Swift Testing/XCTest + `xccov` coverage gate, CI wiring |
| **UI, docs & AI tooling** | `ui-design-and-accessibility.md` | Responsive + light/dark + WCAG 2.2 AA + Claude Design handoff |
| | `diagrams-and-visual-docs.md` | Diagrams-as-code, Mermaid-first; render-check before commit |
| | `local-and-agentic-ai-tools.md` | Agentic assistants + self-hosted LLMs (Ollama/Open WebUI) |
| | `llm-apps.md` | Building software that contains model calls: the five workflow patterns, evaluator-optimizer preconditions, the agent loop (verify every iteration), stopping criteria + cost budgets, RAG as architecture (retriever evals, index as derived