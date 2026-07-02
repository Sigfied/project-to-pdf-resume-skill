# End-to-End Workflow

Use this reference for the complete workflow: project folders -> code analysis -> architecture and feature understanding -> outcome interview -> resume writing -> PDF.

## Deliverables

Produce these artifacts as the task requires:

| Artifact | Purpose |
| --- | --- |
| Intake summary | Records project folders, target role, resume language, output type, and assumptions. |
| Evidence package | Links each resume claim to code, docs, inference, user confirmation, follow-up questions, and draft bullet candidates. |
| Architecture notes | Describes component boundaries, request/data flow, storage, async work, and operational mechanisms. |
| Outcome questions | Asks only for missing facts that materially improve the resume. |
| Resume bullets | Converts verified evidence into concise role-targeted statements. |
| Source document | Updates or creates the resume source file. |
| Verified PDF | Final rendered output after page-count and visual checks. |

## 1. Intake

Capture these fields before analysis:

| Field | Default |
| --- | --- |
| Project folders | Use the folders named by the user. If unclear, inspect likely workspace roots and state assumptions. |
| Target role | Infer from folder names or existing resume files only when the user does not specify it. |
| Resume language | Match the user's request or the existing resume. |
| Output | Ask whether the user wants notes, bullets, source document, rendered PDF, or all outputs. |
| Existing style | Reuse an existing source document or PDF style when available. |

Do not block on missing metrics during intake. Analyze the code first, then ask outcome questions with context.

If the user provides multiple folders, decide whether they represent one resume direction or several tracks. Separate tracks when the folders clearly serve different role targets, technology domains, or seniority narratives.

## 2. Project Type Scan

Before deep code analysis, identify the dominant project type from file names, extensions, and manifests. This reduces blind reading and helps choose high-signal files first.

Use a quick inventory:

```bash
rg --files <project-folder>
find <project-folder> -maxdepth 3 -type f
```

Classify by signals:

| Signals | Likely project type | First files to inspect |
| --- | --- | --- |
| `go.mod`, `cmd/`, `internal/`, `main.go` | Go backend, CLI, or service | `go.mod`, `main.go`, router/handler/service/storage packages |
| `pom.xml`, `build.gradle`, `src/main/java`, `src/main/kotlin` | JVM backend, job, or data service | build file, application entrypoint, controllers, jobs, services, repositories |
| `package.json`, `src/`, `vite`, `next`, `react`, `vue` | Frontend or full-stack web | `package.json`, route/page files, API clients, state, query builders |
| notebooks, `requirements.txt`, `pyproject.toml`, `airflow`, `dbt`, `spark` | Python data, ML, or automation | dependency file, DAG/job entrypoints, models, transforms, configs |
| `Dockerfile`, `helm`, `terraform`, `k8s`, CI files | Infrastructure or deployment-heavy project | deployment manifests, environment config, pipeline definitions |
| `Package.swift`, `.xcodeproj`, `.xcworkspace`, `*.swift` | iOS/macOS app or package | package/project file, app entrypoint, views, models, services |
| `Cargo.toml`, `src/main.rs`, `src/lib.rs` | Rust service, CLI, or library | manifest, entrypoint, modules, tests |
| multiple strong signals | Mixed or monorepo | map subprojects first, then analyze each lane separately |

Record the classification as a hypothesis, not a conclusion. A repository can contain multiple project types.

## 3. Project Inventory

After classification, scan broadly and then read according to project type:

```bash
rg --files <project-folder>
find <project-folder> -maxdepth 3 -type f
```

Prioritize:

- README and documentation files
- build manifests and dependency files
- entrypoints and routing files
- controllers, handlers, services, repositories, data access layers
- scheduled jobs, workers, consumers, producers, queue handlers
- query builders, parsers, schema management, migrations
- tests, deployment files, CI files, and runtime configuration

When an automated inventory script exists in the user's workspace, use it only as a first-pass hypothesis. Verify important claims by opening representative files.

## 4. Evidence Directory

Create a short evidence directory before detailed writing:

| Evidence type | Look for | Resume value |
| --- | --- | --- |
| Dependencies | manifests, lockfiles, modules | stack and framework usage |
| Entry points | main files, routers, CLI commands, workers | runtime shape and responsibilities |
| Domain model | schemas, entities, DTOs, migrations | business objects and data design |
| Flow control | services, orchestrators, pipelines, queues | architecture and execution flow |
| Reliability | retries, transactions, locks, idempotency, tests | production readiness |
| Operations | configs, CI, deployment, observability | delivery and maintainability |

Do not treat dependency presence as skill mastery by itself. A dependency becomes resume-worthy only when the code shows how it is used.

## 5. Evidence Package

Build a reusable evidence package while reading the project. The package should be detailed enough that another agent can trace every final resume bullet back to source material, while still being compact enough for the user to review.

Produce these working artifacts when the project has enough substance:

| Artifact | Required contents | Purpose |
| --- | --- | --- |
| `project-index` | track, project folder/name, project type hypothesis, target role fit, high-signal files, review status | Keeps multiple folders and resume tracks organized. |
| `file-evidence-index` | relative path, evidence category, observed detail, why it matters, related claim IDs | Prevents vague claims by tying observations to concrete files. |
| `project-evidence-card` | problem, architecture flow, core modules, stack-in-use, reliability/performance mechanisms, candidate contribution, confirmed outcomes, unknowns | Gives each project a compact technical narrative before resume writing. |
| `claim-ledger` | claim, evidence status, source, confidence, resume wording, follow-up question, final decision | Tracks which claims are safe, cautious, pending, or rejected. |
| `outcome-question-backlog` | question, why it matters, possible bullet unlocked, priority | Turns missing production/business facts into focused user questions. |
| `resume-bullet-candidates` | draft bullet, source claims, target role, strength, blockers | Separates evidence collection from final copywriting. |

### Project Index

Use one row per folder or subproject:

| Track | Project | Type hypothesis | Target-role fit | High-signal files | Status |
| --- | --- | --- | --- | --- | --- |
| Backend | project-a | Go service | API, storage, async work | `go.mod`, `cmd/...`, `internal/...` | Ready for evidence card |
| Data | project-b | Python pipeline | ingestion, transform, orchestration | `pyproject.toml`, `jobs/...`, `models/...` | Needs more source review |

Keep type hypotheses provisional until architecture evidence confirms them.

### File Evidence Index

Use this index to make code reading auditable:

| Project | File | Evidence category | Observed detail | Why it matters | Claim ID |
| --- | --- | --- | --- | --- | --- |
| project-a | `src/.../handler.ext` | Entry point | Routes requests into service module | Shows runtime responsibility boundary | C-001 |
| project-a | `src/.../repository.ext` | Data access | Encapsulates storage queries | Supports persistence and layering claim | C-002 |
| project-b | `jobs/.../transform.ext` | Data flow | Validates and transforms input records | Supports pipeline implementation claim | C-003 |

Prefer relative paths and short observed details. Avoid absolute local paths in reusable materials.

### Project Evidence Card

Create one compact card per resume-worthy project:

| Field | What to write |
| --- | --- |
| Project purpose | One sentence about the problem the code appears to solve. |
| Architecture flow | One line such as `API -> service -> repository -> storage` or `source -> validation -> transform -> sink`. |
| Core modules | Main folders/classes/functions and their responsibilities. |
| Stack in use | Technologies with actual usage evidence, not just dependencies. |
| Technical highlights | Reliability, performance, concurrency, data quality, security, tooling, or maintainability mechanisms visible in code. |
| Candidate contribution | Ownership level or likely contribution. Mark as pending unless the user confirms it. |
| Confirmed outcomes | Metrics, production status, user impact, adoption, savings, or delivery result confirmed by the user. |
| Unknowns | Open questions that would materially improve the resume. |

### Claim Ledger

Maintain a claim ledger as the main evidence table:

| Track | Project | Claim ID | Claim | Evidence status | Source | Observed detail | Resume use | Confidence | Needs confirmation | Follow-up | Decision |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Backend | project-a | C-001 | Service exposes request handling and delegates business logic | Code evidence | `src/.../handler.ext` | Handler calls service layer | Bullet technical base | High | No | - | Use |
| Backend | project-a | C-002 | Project improved latency or throughput | Needs user confirmation | user interview | Metric not visible in code | Outcome bullet | Low | Yes | What changed after launch? | Pending |
| Data | project-b | C-003 | Pipeline validates input before transformation | Code evidence | `jobs/.../transform.ext` | Validation happens before transform | Bullet technical base | Medium | No | - | Use cautiously |

Use consistent decisions:

- `Use`: safe to include in final resume wording.
- `Use cautiously`: include only with conservative phrasing.
- `Ask user`: needs outcome, ownership, production, or scale confirmation.
- `Hold`: keep in notes but do not use yet.
- `Reject`: remove from final resume because evidence is weak or irrelevant.

### Outcome Question Backlog

Convert pending claims into targeted questions:

| Priority | Project | Question | Why it matters | Bullet unlocked |
| --- | --- | --- | --- | --- |
| High | project-a | Did this run in production or support real users? | Separates demo code from delivered work. | Production delivery bullet |
| High | project-a | Is there a measurable before/after result? | Converts implementation into impact. | Metric-backed achievement |
| Medium | project-b | What data volume or schedule did it handle? | Adds scale and operational context. | Data pipeline scale bullet |

Ask only questions that can change final resume wording. Do not ask broad curiosity questions during the resume pass.

### Resume Bullet Candidate Pool

Draft bullets only after claims have source links:

| Target role | Project | Draft bullet | Source claims | Strength | Blockers |
| --- | --- | --- | --- | --- | --- |
| Backend | project-a | Built a layered service flow covering request handling, business orchestration, and persistence. | C-001, C-002 | Medium | Needs confirmed outcome |
| Data | project-b | Implemented a validation-first data processing flow from input ingestion to transformed output. | C-003 | Medium | Needs scale or usage context |

The candidate pool is not final resume copy. It is a staging area for comparing wording, role fit, and missing evidence.

### Definition of Done

The evidence package is complete enough for resume drafting when:

- every final bullet candidate links to at least one claim ID
- every claim ID links to code, docs, a reasonable inference, or user confirmation
- all metrics, production usage, ownership level, scale, and business impact are user-confirmed or removed
- assumptions are visible and do not appear as facts in final copy
- private absolute paths are absent from reusable documentation
- weak claims have a `Hold` or `Reject` decision instead of leaking into the resume
- the strongest evidence is easy to find without rereading the whole project

### Compact Evidence Map for Small Projects

For small projects or quick passes, a compact evidence map can replace the full package:

| Project | Evidence | Resume meaning | Status |
| --- | --- | --- | --- |
| project-a | Manifest shows language/framework/storage dependencies | Backend service with persistent storage integration | Code evidence |
| project-b | Worker entrypoint connects source, transform, and sink modules | Data processing pipeline with clear runtime flow | Code evidence |
| project-c | UI pages and query builder modules are present | User-facing analytical workflow or internal tool | Reasonable inference |
| project-a | documented measurable improvement | Strong outcome bullet | Needs user confirmation |

Use these status labels:

- `Code evidence`: directly visible in code, manifests, config, docs, or tests.
- `Reasonable inference`: strongly implied by architecture or naming, but should be phrased cautiously.
- `Needs user confirmation`: metrics, business impact, production usage, exact ownership, scale, team size, savings, or adoption.

Add an `Evidence files` column in working notes when the project is large. Use exact relative paths in notes, but avoid publishing private paths in reusable docs.

## 6. Architecture Understanding

For each project, answer:

1. What problem does it appear to solve?
2. What are the main runtime components?
3. What is the request, event, or data flow?
4. What storage, cache, queue, external service, or deployment dependency is involved?
5. What reliability, concurrency, performance, security, or maintainability mechanisms are visible?
6. Which parts are resume-worthy for the target role?

Good architecture summaries are concrete but generic:

- `API layer -> service orchestration -> data access -> storage`
- `input source -> validation -> transformation -> destination`
- `request submission -> asynchronous execution -> status tracking -> result retrieval`

Avoid labels such as "microservice architecture", "high concurrency", or "large scale" unless boundaries, mechanisms, and evidence are visible.

When architecture is complex, summarize at three levels:

1. One-sentence project purpose.
2. Component flow in one line.
3. Implementation highlights with evidence.

## 7. Resume Track Selection

Choose the project emphasis for the target role:

| Target signal | Emphasize |
| --- | --- |
| Backend role | API design, service layering, storage, reliability, async work, performance, deployment. |
| Data role | ingestion, transformation, modeling, storage, query, orchestration, data quality, lineage. |
| Full-stack role | user workflow, API integration, state management, product completeness, delivery. |
| Platform role | reusable capabilities, configuration, multi-project support, observability, operations. |
| AI/tooling role | workflow automation, prompt/tool integration, evaluation, safety, human review loops. |

If several tracks are plausible, create separate draft sections instead of forcing one generic resume.

## 8. Outcome Interview

After building the evidence package, ask only for missing facts that materially improve final bullets:

- Which projects reached production or real users?
- Who used the project: customers, internal teams, developers, operations, analysts, or another audience?
- What measurable result can be stated: latency, throughput, error rate, reliability, cost, efficiency, data volume, user count, request volume, or adoption?
- Was there a migration, replacement, automation, consolidation, or cost reduction result?
- What was the candidate's responsibility: owner, core developer, module owner, maintainer, collaborator, or reviewer?

When the user cannot provide numbers, write qualitative but credible outcomes:

- improved maintainability
- reduced manual operations
- standardized a previously ad hoc workflow
- improved recoverability or operational visibility
- supported recurring usage by a known audience

## 9. Material Organization

Create intermediate material before the final resume:

1. `Project list`: representative projects for the target role.
2. `Tech stack`: stack plus actual usage, not dependency dumping.
3. `Architecture`: component flow and module boundaries.
4. `Technical highlights`: why the implementation is worth mentioning.
5. `Implementation details`: concrete files, patterns, transactions, queues, caches, jobs, APIs, queries, tests, or deployment mechanisms.
6. `Outcomes`: confirmed outcomes first; pending questions second.
7. `Resume bullets`: concise bullets ready for the final resume.

This intermediate material lets the user correct facts before layout work begins.

## 10. Resume Draft Gate

Before touching layout, verify:

- every final bullet has evidence or user confirmation
- the strongest project appears early
- each project has a clear role in the target narrative
- weak bullets are merged or removed
- unresolved claims are kept in notes, not final copy
- confidential names and metrics use approved wording

## 11. Finalization Loop

Use this loop until done:

1. Generate draft bullets from verified evidence.
2. Ask user for outcome gaps.
3. Integrate confirmed outcomes.
4. Update the resume source.
5. Compile or render the PDF.
6. Inspect the rendered pages.
7. Fix spacing, overflow, page count, and weak bullets.
8. Deliver final paths and unresolved assumptions.
