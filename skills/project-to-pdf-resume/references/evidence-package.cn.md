---
title: 证据包格式
description: 结构化证据产物、JSON metadata 格式、结论台账和完成标准。
lang: zh-CN
version: 1.0.0
---

# 证据包格式

读代码时同步建立证据包。证据包是后续简历 bullet 和 PDF 内容的事实来源。

中间产物优先使用 JSON，因为 agent 更容易检查、diff、校验和复用。可以额外为用户生成 Markdown 摘要。

## 产物文件

如果需要落盘，默认使用这个结构，除非用户指定其他形式：

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

如果用户没有要求写文件，也要在工作笔记中保持相同的数据形状。

## Metadata JSON

创建 `evidence-package.meta.json` 描述整组产物：

```json
{
  "schema_version": "1.0.0",
  "package_type": "project-to-pdf-resume.evidence-package",
  "created_at": "YYYY-MM-DDTHH:mm:ssZ",
  "language": "zh-CN",
  "target_role": "后端工程师",
  "target_seniority": "中高级",
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

优先使用相对路径、项目别名或用户批准的标签。除非用户明确要求本地私有笔记，否则不要把私有绝对路径写入可复用产物。

## 产物 Schema

### `project-index.json`

每个文件夹或子项目一个对象：

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

让读代码过程可审计：

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

每个值得写进简历的项目一张卡：

```json
[
  {
    "project_id": "project-a",
    "project_purpose": "一句话说明代码看起来解决的问题。",
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
    "unknowns": ["是否上线到生产？"]
  }
]
```

### `claim-ledger.json`

主证据表：

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

### `outcome-question-backlog.json`

把待确认结论转换成聚焦问题：

```json
[
  {
    "priority": "high",
    "project_id": "project-a",
    "question": "是否上线或支持真实用户？",
    "why_it_matters": "区分 demo 和真实交付。",
    "unlocks": "production delivery bullet",
    "related_claim_ids": ["C-002"]
  }
]
```

### `resume-bullet-candidates.json`

只有当 claim 已经有来源时，才开始起草 bullet：

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

## 小项目精简证据表

小项目或快速分析时，可以用精简表替代分散 JSON 文件：

| 项目 | 证据 | 简历含义 | 状态 |
| --- | --- | --- | --- |
| project-a | 依赖文件显示语言、框架、存储 | 具备后端服务和持久化集成 | Code evidence |
| project-b | worker 入口串联输入、转换和输出模块 | 具备清晰运行流的数据处理链路 | Code evidence |
| project-c | 存在 UI 页面和查询构造模块 | 可能是用户侧分析流程或内部工具 | Reasonable inference |
| project-a | 已确认的可量化改进 | 强成果 bullet | Needs user confirmation |

## 完成标准

证据包满足这些条件后，才足够进入简历草稿：

- 每条最终 bullet 候选至少关联一个 claim ID
- 每个 claim ID 都能追溯到代码、文档、合理推断或用户确认
- 指标、上线使用、职责范围、规模和业务影响已由用户确认，或从最终文案移除
- 假设保持可见，但不会在最终简历中伪装成事实
- 可复用文档中没有本机私有绝对路径
- 弱结论被标记为 `Hold` 或 `Reject`，不会漏进简历
- 不需要重新读完整项目，就能找到最强证据
