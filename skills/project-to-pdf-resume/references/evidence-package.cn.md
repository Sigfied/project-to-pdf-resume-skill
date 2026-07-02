---
title: 证据包格式
description: 使用 Markdown 存储详细证据描述，并用 JSON metadata 和 index 描述组织结构。
lang: zh-CN
version: 1.1.0
---

# 证据包格式

读代码时同步建立证据包。证据包是后续简历 bullet 和 PDF 内容的事实来源。

使用 **Markdown 保存详细证据描述**，使用 **JSON 保存组织结构 metadata**。Markdown 适合长推理、代码观察和用户可读笔记；JSON 告诉 agent 每份 Markdown 在哪里、结论如何关联项目/文件/问题/bullet，以及哪些条目可用、待确认或已拒绝。

## 产物结构

如果需要落盘，默认使用这个结构，除非用户指定其他形式：

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

如果用户没有要求写文件，也要在工作笔记中保持同样的分离：Markdown 风格的详细证据，加一个紧凑的 JSON-like 索引用于导航。

## Metadata JSON

创建 `evidence-package.meta.json` 描述证据包本身：

```json
{
  "schema_version": "1.1.0",
  "package_type": "project-to-pdf-resume.evidence-package",
  "created_at": "YYYY-MM-DDTHH:mm:ssZ",
  "updated_at": "YYYY-MM-DDTHH:mm:ssZ",
  "language": "zh-CN",
  "target_role": "后端工程师",
  "target_seniority": "中高级",
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

优先使用相对路径、项目别名或用户批准的标签。除非用户明确要求本地私有笔记，否则不要把私有绝对路径写入可复用产物。

## Index JSON

创建 `evidence-package.index.json`，作为跨 Markdown 文档的 agent-readable map：

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

JSON 值保持简短。详细推理写进 Markdown，再用 `markdown_ref` 链接过去。

允许的 `evidence_status`：

- `Code evidence`
- `Reasonable inference`
- `Needs user confirmation`

允许的 `decision`：

- `Use`
- `Use cautiously`
- `Ask user`
- `Hold`
- `Reject`

## Markdown 文档

### `00-intake-summary.md`

记录目标岗位、资历、语言、请求输出、源文件夹、假设和隐私说明。

### `01-project-index.md`

每个文件夹或子项目一个小节：

```markdown
## project-a

- Track: Backend
- Type hypothesis: Go service
- Target-role fit: API, storage, async work
- High-signal files: `go.mod`, `cmd/...`, `internal/...`
- Review status: ready for evidence card
```

### `02-file-evidence-index.md`

让读代码过程可审计：

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

每个值得写进简历的项目一张卡：

```markdown
## project-a

### Project Purpose
一句话说明代码看起来解决的问题。

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
- 是否上线到生产？
```

### `04-claim-ledger.md`

每个 claim 一个小节，保持 claim ID 稳定：

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

把待确认结论转换成聚焦问题：

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

只有当 claim 已经有来源时，才开始起草 bullet：

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

## 小项目精简证据表

小项目或快速分析时，一张 Markdown 表加一个很小的 JSON index 就足够：

| 项目 | 证据 | 简历含义 | 状态 |
| --- | --- | --- | --- |
| project-a | 依赖文件显示语言、框架、存储 | 具备后端服务和持久化集成 | Code evidence |
| project-b | worker 入口串联输入、转换和输出模块 | 具备清晰运行流的数据处理链路 | Code evidence |
| project-c | 存在 UI 页面和查询构造模块 | 可能是用户侧分析流程或内部工具 | Reasonable inference |
| project-a | 已确认的可量化改进 | 强成果 bullet | Needs user confirmation |

## 完成标准

证据包满足这些条件后，才足够进入简历草稿：

- 每条最终 bullet 候选至少关联一个 claim ID
- 每个 claim ID 都有 Markdown 证据详情和 JSON 索引元数据
- 每个 claim ID 都能追溯到代码、文档、合理推断或用户确认
- 指标、上线使用、职责范围、规模和业务影响已由用户确认，或从最终文案移除
- 假设保持可见，但不会在最终简历中伪装成事实
- 可复用文档中没有本机私有绝对路径
- 弱结论被标记为 `Hold` 或 `Reject`，不会漏进简历
- 不需要重新读完整项目，就能找到最强证据
