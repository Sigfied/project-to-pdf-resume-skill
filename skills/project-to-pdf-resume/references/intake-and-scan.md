---
title: Intake And Project Scan
description: Input confirmation, project type classification, code inventory, and first evidence directory.
lang: en
version: 1.0.0
---

# Intake And Project Scan

Use this reference before writing any resume claims. The goal is to understand what kind of project is in front of you and which files deserve attention first.

## Intake Summary

Capture these fields before analysis:

| Field | Default |
| --- | --- |
| Project folders | Use the folders named by the user. If unclear, inspect likely workspace roots and state assumptions. |
| Target role | Infer from folder names or existing resume files only when the user does not specify it. |
| Seniority | Infer cautiously from user request or existing resume; keep uncertain if unclear. |
| Resume language | Match the user's request or the existing resume. |
| Output | Ask whether the user wants notes, bullets, source document, rendered PDF, or all outputs. |
| Existing style | Reuse an existing source document or PDF style when available. |

Do not block on missing metrics during intake. Analyze the code first, then ask outcome questions with context.

If the user provides multiple folders, decide whether they represent one resume direction or several tracks. Separate tracks when folders clearly serve different role targets, technology domains, or seniority narratives.

## Project Type Scan

Before deep code analysis, identify the dominant project type from file names, extensions, and manifests.

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

Record classification as a hypothesis, not a conclusion. A repository can contain multiple project types.

## Project Inventory

After classification, scan broadly and then read according to project type.

Prioritize:

- README and documentation files
- build manifests and dependency files
- entrypoints and routing files
- controllers, handlers, services, repositories, data access layers
- scheduled jobs, workers, consumers, producers, queue handlers
- query builders, parsers, schema management, migrations
- tests, deployment files, CI files, and runtime configuration

When an automated inventory script exists in the user's workspace, use it only as a first-pass hypothesis. Verify important claims by opening representative files.

## Evidence Directory

Create a short evidence directory before detailed writing:

| Evidence type | Look for | Resume value |
| --- | --- | --- |
| Dependencies | manifests, lockfiles, modules | stack and framework usage |
| Entry points | main files, routers, CLI commands, workers | runtime shape and responsibilities |
| Domain model | schemas, entities, DTOs, migrations | business objects and data design |
| Flow control | services, orchestrators, pipelines, queues | architecture and execution flow |
| Reliability | retries, transactions, locks, idempotency, tests | production readiness |
| Operations | configs, CI, deployment, observability | delivery and maintainability |

Do not treat dependency presence as skill mastery by itself. A dependency becomes resume-worthy only when code shows how it is used.
