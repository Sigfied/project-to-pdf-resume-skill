---
title: Architecture And Resume Materials
description: Architecture synthesis, target-role track selection, intermediate material organization, and draft gates.
lang: en
version: 1.0.0
---

# Architecture And Resume Materials

Use this reference after the first evidence pass. It turns code observations into project narratives and role-targeted resume material.

## Architecture Understanding

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

## Resume Track Selection

Choose the project emphasis for the target role:

| Target signal | Emphasize |
| --- | --- |
| Backend role | API design, service layering, storage, reliability, async work, performance, deployment. |
| Data role | ingestion, transformation, modeling, storage, query, orchestration, data quality, lineage. |
| Full-stack role | user workflow, API integration, state management, product completeness, delivery. |
| Platform role | reusable capabilities, configuration, multi-project support, observability, operations. |
| AI/tooling role | workflow automation, prompt/tool integration, evaluation, safety, human review loops. |

If several tracks are plausible, create separate draft sections instead of forcing one generic resume.

## Material Organization

Create intermediate material before final resume writing:

1. `Project list`: representative projects for the target role.
2. `Tech stack`: stack plus actual usage, not dependency dumping.
3. `Architecture`: component flow and module boundaries.
4. `Technical highlights`: why the implementation is worth mentioning.
5. `Implementation details`: concrete files, patterns, transactions, queues, caches, jobs, APIs, queries, tests, or deployment mechanisms.
6. `Outcomes`: confirmed outcomes first; pending questions second.
7. `Resume bullets`: concise bullets ready for the final resume.

This intermediate material lets the user correct facts before layout work begins.

## Resume Draft Gate

Before touching layout, verify:

- every final bullet has evidence or user confirmation
- the strongest project appears early
- each project has a clear role in the target narrative
- weak bullets are merged or removed
- unresolved claims are kept in notes, not final copy
- confidential names and metrics use approved wording

## Finalization Loop

Use this loop until done:

1. Generate draft bullets from verified evidence.
2. Ask user for outcome gaps.
3. Integrate confirmed outcomes.
4. Update the resume source.
5. Compile or render the PDF.
6. Inspect the rendered pages.
7. Fix spacing, overflow, page count, and weak bullets.
8. Deliver final paths and unresolved assumptions.
