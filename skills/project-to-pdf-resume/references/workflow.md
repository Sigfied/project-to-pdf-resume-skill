---
title: End-to-End Workflow
description: Stage router for turning project folders into evidence-backed resume material and PDF output.
lang: en
version: 1.0.0
---

# End-to-End Workflow

Use this file as the workflow router, not as the only reference. Load each deeper reference only when its stage is active.

## Stage Map

| Stage | Goal | Load next |
| --- | --- | --- |
| Intake | Confirm folders, target role, language, output type, and assumptions. | [intake-and-scan.md](intake-and-scan.md) |
| Project type scan | Identify dominant file types and manifests before deep reading. | [intake-and-scan.md](intake-and-scan.md) |
| Code inventory | Read high-signal files by project type. | [intake-and-scan.md](intake-and-scan.md) |
| Evidence package | Produce Markdown evidence documents plus JSON metadata/index files. | [evidence-package.md](evidence-package.md) |
| Architecture synthesis | Summarize project purpose, component flow, technical highlights, and role fit. | [architecture-and-materials.md](architecture-and-materials.md) |
| Outcome interview | Ask only for missing production, scale, ownership, metric, or impact facts. | [interview-and-writing.md](interview-and-writing.md) |
| Resume writing | Convert verified claims into target-role bullets. | [interview-and-writing.md](interview-and-writing.md) |
| PDF production | Update source, prefer LaTeX, compile/render, and verify output. | [pdf-production.md](pdf-production.md) |
| Layout refinement | Apply compact Chinese technical resume layout guidance when relevant. | [layout-reference.md](layout-reference.md) |

For Simplified Chinese work, load the `.cn.md` counterpart for each stage.

## Required Deliverables

Produce only the artifacts needed for the user's requested output, but keep internal traceability intact.

| Artifact | Purpose | Default format |
| --- | --- | --- |
| Intake summary | Records folders, target role, resume language, output type, assumptions. | Markdown or JSON object |
| Project scan | Captures project type hypothesis and high-signal files. | Markdown table or JSON array |
| Evidence package | Links claims to files, inference status, questions, and bullet candidates. | Markdown details plus JSON metadata/index |
| Architecture notes | Explains runtime flow, module boundaries, stack-in-use, and highlights. | Markdown |
| Outcome questions | Focuses user confirmation on facts that can improve final bullets. | Markdown list or JSON array |
| Resume bullets | Converts verified evidence into concise role-targeted statements. | Markdown |
| Source document | Updates or creates the resume source. | Prefer LaTeX for PDF output |
| Verified PDF | Final rendered output after page-count and visual checks. | PDF |

## Execution Loop

1. Confirm intake and target output.
2. Scan project type before deep code reading.
3. Inventory high-signal files and create an evidence directory.
4. Build the evidence package with claim IDs and source links.
5. Synthesize architecture and target-role positioning.
6. Ask outcome questions only where evidence is missing.
7. Draft resume bullets from verified claims.
8. Produce source and PDF when requested.
9. Verify rendered output and revise until readable, compact, and truthful.

## Stop Conditions

Do not move a claim into final resume copy when:

- it has no code, docs, reasonable inference, or user confirmation
- it states metrics, scale, production usage, ownership, savings, or business value without user confirmation
- it depends on private wording the user has not approved
- it makes the project sound larger or more senior than the evidence supports
