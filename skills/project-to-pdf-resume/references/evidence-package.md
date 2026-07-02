---
title: Evidence Package Format
description: Markdown evidence documents plus JSON metadata and index files for agent-readable organization.
lang: en
version: 1.1.0
---

# Evidence Package Format

Build the evidence package while reading code. The package is the source of truth for later resume bullets and PDF content.

Use **Markdown for detailed evidence descriptions** and **JSON for organization metadata**. Markdown keeps long reasoning, code observations, and user-readable notes easy to inspect. JSON tells agents where each Markdown document is, how claims connect to projects/files/questions/bullets, and which items are ready, pending, or rejected.

## Package Layout

When writing artifacts to disk, use this layout unless the user requests another structure:

```text
resume-evidence/
├── evidence-package.meta.json
├── evidence-package.index.json
└── markdown/
    ├── 00-intake-summary.md
    ├── 01-project-index.md
    ├── 02-file-evidence-index.md
    ├── 03-project-evidence-cards.md
    ├── 04-claim-ledger.md
    ├── 05-outcome-question-backlog.md
    └── 06-resume-bullet-candidates.md
```

If the user did not ask for files, keep the same separation in working notes: Markdown-style evidence details, plus a compact JSON-like index for navigation.

## Metadata JSON

Create `evidence-package.meta.json` to describe the package itself:

```json
{
  "schema_version": "1.1.0",
  "package_type": "project-to-pdf-resume.evidence-package",
  "created_at": "YYYY-MM-DDTHH:mm:ssZ",
  "updated_at": "YYYY-MM-DDTHH:mm:ssZ",
  "language": "en",
  "target_role": "Backend Engineer",
  "target_seniority": "mid-senior",
  "document_format": "markdown-plus-json-index",
  "source_project_count": 2,
  "privacy": {
    "contains_private_source_paths": false,
    "contains_confidential_names": false,
    "requires_user_redaction_review": true
  },
  "documents": [
    {
      "doc_id": "claim-ledger",
      "kind": "claim_ledger",
      "path": "markdown/04-claim-ledger.md",
      "description": "Detailed claim table with evidence status and decisions."
    }
  ]
}
```

Use relative paths, project aliases, or user-approved labels. Do not write private absolute paths into reusable artifacts unless the user explicitly asks for local-only notes.

## Index JSON

Create `evidence-package.index.json` as the agent-readable map across Markdown documents:

```json
{
  "schema_version": "1.1.0",
  "projects": [
    {
      "project_id": "project-a",
      "display_name": "project-a",
      "track": "Backend",
      "type_hypothesis": "Go service",
      "review_status": "ready_for_evidence_card",
      "markdown_ref": {
        "path": "markdown/01-project-index.md",
        "anchor": "project-a"
      },
      "evidence_card_ref": {
        "path": "markdown/03-project-evidence-cards.md",
        "anchor": "project-a"
      },
      "claim_ids": ["C-001", "C-002"]
    }
  ],
  "claims": [
    {
      "claim_id": "C-001",
      "project_id": "project-a",
      "claim": "Service exposes request handling and delegates business logic.",
      "evidence_status": "Code evidence",
      "decision": "Use",
      "confidence": "high",
      "needs_confirmation": false,
      "source_refs": [
        {
          "type": "file",
          "path": "src/.../handler.ext",
          "markdown_ref": {
            "path": "markdown/02-file-evidence-index.md",
            "anchor": "project-a-handler"
          }
        }
      ],
      "claim_markdown_ref": {
        "path": "markdown/04-claim-ledger.md",
        "anchor": "c-001"
      }
    }
  ],
  "outcome_questions": [
    {
      "question_id": "Q-001",
      "project_id": "project-a",
      "priority": "high",
      "related_claim_ids": ["C-002"],
      "markdown_ref": {
        "path": "markdown/05-outcome-question-backlog.md",
        "anchor": "q-001"
      }
    }
  ],
  "bullet_candidates": [
    {
      "bullet_id": "B-001",
      "project_id": "project-a",
      "target_role": "Backend Engineer",
      "source_claim_ids": ["C-001"],
      "final_copy_ready": false,
      "markdown_ref": {
        "path": "markdown/06-resume-bullet-candidates.md",
        "anchor": "b-001"
      }
    }
  ]
}
```

Keep JSON values concise. Put detailed reasoning in Markdown, then link to it with `markdown_ref`.

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

## Markdown Documents

### `00-intake-summary.md`

Include target role, target seniority, language, requested outputs, source folders, assumptions, and privacy notes.

### `01-project-index.md`

One section per folder or subproject:

```markdown
## project-a

- Track: Backend
- Type hypothesis: Go service
- Target-role fit: API, storage, async work
- High-signal files: `go.mod`, `cmd/...`, `internal/...`
- Review status: ready for evidence card
```

### `02-file-evidence-index.md`

Make code reading auditable:

```markdown
## project-a-handler

- Project: project-a
- File: `src/.../handler.ext`
- Evidence category: entry point
- Observed detail: Routes requests into service module.
- Why it matters: Shows runtime responsibility boundary.
- Related claims: C-001
```

### `03-project-evidence-cards.md`

Create one card per resume-worthy project:

```markdown
## project-a

### Project Purpose
One sentence about the problem the code appears to solve.

### Architecture Flow
`API -> service -> repository -> storage`

### Core Modules
- `service`: business orchestration

### Stack In Use
- PostgreSQL: evidenced by repository/query module

### Technical Highlights
- transaction handling
- idempotent retry

### Candidate Contribution
Needs user confirmation.

### Confirmed Outcomes
None yet.

### Unknowns
- Was this deployed to production?
```

### `04-claim-ledger.md`

Use one section per claim. Keep the claim ID stable:

```markdown
## C-001

- Project: project-a
- Track: Backend
- Claim: Service exposes request handling and delegates business logic.
- Evidence status: Code evidence
- Sources:
  - `src/.../handler.ext`: Handler calls service layer.
- Resume use: bullet technical base
- Confidence: high
- Needs confirmation: no
- Decision: Use
```

### `05-outcome-question-backlog.md`

Convert pending claims into targeted questions:

```markdown
## Q-001

- Project: project-a
- Priority: high
- Question: Did this run in production or support real users?
- Why it matters: Separates demo code from delivered work.
- Unlocks: production delivery bullet
- Related claims: C-002
```

### `06-resume-bullet-candidates.md`

Draft bullets only after claims have source links:

```markdown
## B-001

- Target role: Backend Engineer
- Project: project-a
- Draft bullet: Built a layered service flow covering request handling, business orchestration, and persistence.
- Source claims: C-001
- Strength: medium
- Blockers: needs confirmed outcome
- Final copy ready: false
```

## Compact Evidence Map

For small projects or quick passes, a single Markdown table plus a tiny JSON index is enough:

| Project | Evidence | Resume meaning | Status |
| --- | --- | --- | --- |
| project-a | Manifest shows language/framework/storage dependencies | Backend service with persistent storage integration | Code evidence |
| project-b | Worker entrypoint connects source, transform, and sink modules | Data processing pipeline with clear runtime flow | Code evidence |
| project-c | UI pages and query builder modules are present | User-facing analytical workflow or internal tool | Reasonable inference |
| project-a | documented measurable improvement | Strong outcome bullet | Needs user confirmation |

## Definition Of Done

The evidence package is ready for resume drafting when:

- every final bullet candidate links to at least one claim ID
- every claim ID has Markdown evidence details and JSON index metadata
- every claim ID links to code, docs, reasonable inference, or user confirmation
- all metrics, production usage, ownership level, scale, and business impact are user-confirmed or removed
- assumptions are visible and do not appear as facts in final copy
- private absolute paths are absent from reusable documentation
- weak claims have a `Hold` or `Reject` decision instead of leaking into the resume
- the strongest evidence is easy to find without rereading the whole project
