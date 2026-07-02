---
name: project-to-pdf-resume
description: Use when turning one or more local project folders into an evidence-backed technical resume, resume bullets, LaTeX source, or rendered PDF, especially when code architecture and user-confirmed outcomes must be separated.
---

# Project To PDF Resume

## Overview

Create a technical resume from local project folders without inventing impact. Use code evidence for architecture, features, stack, and implementation details; use user confirmation for outcomes, scale, ownership, production results, and business value.

The default output is an evidence-backed resume package: project evidence notes, a claim ledger, outcome questions, polished bullets, source document updates, and a verified PDF when requested.

## Progressive Reference Loading

Start with this `SKILL.md` only. Load bundled references only when the current task reaches the stage that needs that detail; do not preload every reference at the start.

| When the task needs... | Read this reference |
| --- | --- |
| Stage routing, deliverable overview, or stop conditions | [references/workflow.md](references/workflow.md) |
| Intake, project type scanning, code inventory, or first evidence directory | [references/intake-and-scan.md](references/intake-and-scan.md) |
| Evidence package artifacts, JSON metadata, claim ledger schema, or traceability rules | [references/evidence-package.md](references/evidence-package.md) |
| Architecture synthesis, target-role track selection, material organization, or draft gates | [references/architecture-and-materials.md](references/architecture-and-materials.md) |
| Outcome interview questions, claim wording, bullet drafting, or final resume copy review | [references/interview-and-writing.md](references/interview-and-writing.md) |
| Source document creation, LaTeX-first PDF production, compilation, rendering, or visual QA | [references/pdf-production.md](references/pdf-production.md) |
| Compact Chinese technical resume layout, or a user-provided PDF/TEX layout sample that should guide the final PDF | [references/layout-reference.md](references/layout-reference.md) |

For Simplified Chinese work, load the matching `.cn.md` counterpart at the same stage.

After reading a reference, apply only the guidance relevant to the current stage, then return to the required flow below.

## Required Flow

1. Confirm inputs and target.
   - Identify the project folders, target role, seniority, resume language, and expected output: notes, resume bullets, source document, PDF, or all of them.
   - If the target is unclear, infer a tentative direction from folder names and existing resume files, then state the assumption.
   - Preserve private context in task outputs only. Do not copy private project names, company names, file paths, or business details into reusable skill documentation.

2. Inventory code before writing.
   - First inspect dominant file types and manifests to classify the project type: backend, frontend, data pipeline, mobile app, CLI/tooling, library, infrastructure, or mixed.
   - Use that classification to choose the first files to read instead of treating every repository the same.
   - Traverse each project folder with `rg --files`, `find`, manifests, README files, config files, and entrypoints.
   - Read high-signal files: routes, controllers, handlers, services, repositories, jobs, workers, queue/cache code, SQL/query builders, schema or migration code, tests, and deployment files.
   - Build a source-of-truth evidence package before writing resume claims.

3. Build an evidence package.
   - For every project, record role positioning, stack, core modules, request/data flow, technical highlights, implementation details, and evidence files.
   - Include JSON-first metadata and artifacts: project index, file evidence index, project evidence card, claim ledger, outcome question backlog, and resume bullet candidate pool when the project has enough substance.
   - Label each claim as `Code evidence`, `Reasonable inference`, or `Needs user confirmation`.
   - Treat ownership, scale, production usage, metrics, business value, cost savings, and performance gains as `Needs user confirmation` unless they are explicitly documented.
   - Separate reusable project notes from final resume copy so the same evidence can support multiple role-specific resumes.

4. Ask for missing outcomes.
   - Ask after the code evidence pass, not before.
   - Keep questions short and grounded in observed project behavior.
   - Prefer questions about production usage, users, scale, reliability, performance, cost, migration, automation, and the candidate's actual responsibility.
   - Ask for exact wording permission when claims may reveal confidential customers, internal systems, or sensitive metrics.

5. Draft resume material.
   - Match the user's requested language; otherwise use the language of the existing resume.
   - Convert evidence into action + method + result bullets.
   - Rank bullets by target-role relevance, technical depth, and confirmed outcome strength.
   - Keep uncertainty visible in notes; remove unresolved claims from final bullets.

6. Produce and verify the PDF when requested.
   - Prefer LaTeX for final resume typesetting because it gives stable typography, compact layout control, repeatable builds, and strong PDF output.
   - Reuse an existing non-LaTeX source only when it is already the user's canonical resume workflow or the user requests that format.
   - Generate or update the source document, compile/render the PDF, inspect page count, render pages to images when possible, and check spacing, overflow, section balance, and font rendering.
   - Iterate until the PDF is readable, compact, and truthful.

## Rules

- Do not include private project details in reusable skill documentation.
- Do not infer business impact from code alone.
- Prefer project-specific implementation details over generic stack lists.
- Keep uncertainty visible until the user confirms it.
- Preserve existing resume style unless the user asks for a redesign.
- Verify rendered output before claiming a PDF is complete.
- If a claim cannot be verified from code, docs, or user confirmation, keep it out of the final resume.
