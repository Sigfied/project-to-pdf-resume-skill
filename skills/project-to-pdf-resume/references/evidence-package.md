---
title: Evidence Package Format
description: Structured evidence artifacts, JSON metadata format, claim ledger, and completion rules.
lang: en
version: 1.0.0
---

# Evidence Package Format

Build the evidence package while reading code. The package is the source of truth for later resume bullets and PDF content.

Prefer JSON for durable intermediate artifacts because it is easier for agents to inspect, diff, validate, and reuse across resume variants. A Markdown summary can be added for the user.

## Package Files

When writing artifacts to disk, use this layout unless the user requests another structure:

```text
resume-evidence/
├── evidence-package.meta.json
├── project-index.json
├── file-evidence-index.json
├── project-evidence-cards.json
├── claim-ledger.json
├── outcome-question-backlog.json
├── resume-bullet-candidates.json
└── summary.md
```

If the user did not ask for files, keep the same shapes in working notes.

## Metadata JSON

Create `evidence-package.meta.json` to describe the artifact set:

```json
{
  "schema_version": "1.0.0",
  "package_type": "project-to-pdf-resume.evidence-package",
  "created_at": "YYYY-MM-DDTHH:mm:ssZ",
  "language": "en",
  "target_role": "Backend Engineer",
  "target_seniority": "mid-senior",
  "source_project_count": 2,
  "source_projects": [
    {
      "project_id": "project-a",
      "display_name": "project-a",
      "path_hint": "relative/path/or/user-label",
      "type_hypothesis": "Go service",
      "review_status": "in_progress"
    }
  ],
  "privacy": {
    "contains_private_source_paths": false,
    "contains_confidential_names": false,
    "requires_user_redaction_review": true
  },
  "artifact_files": {
    "project_index": "project-index.json",
    "file_evidence_index": "file-evidence-index.json",
    "project_evidence_cards": "project-evidence-cards.json",
    "claim_ledger": "claim-ledger.json",
    "outcome_question_backlog": "outcome-question-backlog.json",
    "resume_bullet_candidates": "resume-bullet-candidates.json",
    "summary": "summary.md"
  }
}
```

Use relative paths, project aliases, or user-approved labels. Do not write private absolute paths into reusable artifacts unless the user explicitly asks for local-only notes.

## Artifact Schemas

### `project-index.json`

One object per folder or subproject:

```json
[
  {
    "track": "Backend",
    "project_id": "project-a",
    "display_name": "project-a",
    "type_hypothesis": "Go service",
    "target_role_fit": ["API", "storage", "async work"],
    "high_signal_files": ["go.mod", "cmd/...", "internal/..."],
    "review_status": "ready_for_evidence_card",
    "notes": []
  }
]
```

### `file-evidence-index.json`

Use this to make code reading auditable:

```json
[
  {
    "project_id": "project-a",
    "file": "src/.../handler.ext",
    "evidence_category": "entry_point",
    "observed_detail": "Routes requests into service module.",
    "why_it_matters": "Shows runtime responsibility boundary.",
    "related_claim_ids": ["C-001"]
  }
]
```

### `project-evidence-cards.json`

One compact card per resume-worthy project:

```json
[
  {
    "project_id": "project-a",
    "project_purpose": "One sentence about the problem the code appears to solve.",
    "architecture_flow": "API -> service -> repository -> storage",
    "core_modules": [
      {
        "name": "service",
        "responsibility": "Business orchestration"
      }
    ],
    "stack_in_use": [
      {
        "technology": "PostgreSQL",
        "evidence": "repository/query module"
      }
    ],
    "technical_highlights": ["transaction handling", "idempotent retry"],
    "candidate_contribution": {
      "status": "needs_user_confirmation",
      "notes": ""
    },
    "confirmed_outcomes": [],
    "unknowns": ["Was this deployed to production?"]
  }
]
```

### `claim-ledger.json`

This is the main evidence table:

```json
[
  {
    "claim_id": "C-001",
    "track": "Backend",
    "project_id": "project-a",
    "claim": "Service exposes request handling and delegates business logic.",
    "evidence_status": "Code evidence",
    "sources": [
      {
        "type": "file",
        "path": "src/.../handler.ext",
        "observed_detail": "Handler calls service layer."
      }
    ],
    "resume_use": "bullet_technical_base",
    "confidence": "high",
    "needs_confirmation": false,
    "follow_up_question": null,
    "decision": "Use"
  }
]
```

Allowed `evidence_status` values:

- `Code evidence`
- `Reasonable inference`
- `Needs user confirmation`

Allowed `decision` values:

- `Use`
- `Use cautiously`
- `Ask user`
- `Hold`
- `Reject`

### `outcome-question-backlog.json`

Convert pending claims into targeted questions:

```json
[
  {
    "priority": "high",
    "project_id": "project-a",
    "question": "Did this run in production or support real users?",
    "why_it_matters": "Separates demo code from delivered work.",
    "unlocks": "production delivery bullet",
    "related_claim_ids": ["C-002"]
  }
]
```

### `resume-bullet-candidates.json`

Draft bullets only after claims have source links:

```json
[
  {
    "target_role": "Backend Engineer",
    "project_id": "project-a",
    "draft_bullet": "Built a layered service flow covering request handling, business orchestration, and persistence.",
    "source_claim_ids": ["C-001"],
    "strength": "medium",
    "blockers": ["needs confirmed outcome"],
    "final_copy_ready": false
  }
]
```

## Compact Evidence Map

For small projects or quick passes, a compact table can replace separate JSON files:

| Project | Evidence | Resume meaning | Status |
| --- | --- | --- | --- |
| project-a | Manifest shows language/framework/storage dependencies | Backend service with persistent storage integration | Code evidence |
| project-b | Worker entrypoint connects source, transform, and sink modules | Data processing pipeline with clear runtime flow | Code evidence |
| project-c | UI pages and query builder modules are present | User-facing analytical workflow or internal tool | Reasonable inference |
| project-a | documented measurable improvement | Strong outcome bullet | Needs user confirmation |

## Definition Of Done

The evidence package is ready for resume drafting when:

- every final bullet candidate links to at least one claim ID
- every claim ID links to code, docs, reasonable inference, or user confirmation
- all metrics, production usage, ownership level, scale, and business impact are user-confirmed or removed
- assumptions are visible and do not appear as facts in final copy
- private absolute paths are absent from reusable documentation
- weak claims have a `Hold` or `Reject` decision instead of leaking into the resume
- the strongest evidence is easy to find without rereading the whole project
