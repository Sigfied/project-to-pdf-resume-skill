---
title: 输入确认和项目扫描
description: 输入确认、工程类型分类、代码盘点和第一轮证据目录。
lang: zh-CN
version: 1.0.0
---

# 输入确认和项目扫描

写任何简历结论前先使用本 reference。目标是弄清眼前是什么类型的项目，以及哪些文件最值得优先阅读。

## 输入摘要

分析前确认：

| 字段 | 默认处理 |
| --- | --- |
| 项目文件夹 | 使用用户指定的文件夹；不明确时扫描可能的 workspace 根目录并说明假设。 |
| 目标岗位 | 用户未说明时，才从文件夹名或现有简历中谨慎推断。 |
| 资历层级 | 从用户要求或现有简历谨慎推断；不清楚时保持未知。 |
| 简历语言 | 匹配用户要求或现有简历语言。 |
| 输出形式 | 确认需要素材、bullet、源文件、PDF，还是全部。 |
| 现有风格 | 有现有源文件或 PDF 风格时优先复用。 |

不要在输入阶段卡住指标缺失。先分析代码，再基于上下文追问成果。

如果用户给了多个文件夹，判断它们是同一简历方向还是多个方向。文件夹明显对应不同岗位、技术领域或资历叙事时，要拆成多个 track。

## 工程类型扫描

深入读代码前，先通过文件名、扩展名和 manifest 判断主要工程类型。

快速盘点：

```bash
rg --files <project-folder>
find <project-folder> -maxdepth 3 -type f
```

按信号分类：

| 信号 | 可能工程类型 | 优先查看文件 |
| --- | --- | --- |
| `go.mod`, `cmd/`, `internal/`, `main.go` | Go 后端、CLI 或服务 | `go.mod`、`main.go`、router/handler/service/storage 包 |
| `pom.xml`, `build.gradle`, `src/main/java`, `src/main/kotlin` | JVM 后端、任务或数据服务 | 构建文件、应用入口、controller、job、service、repository |
| `package.json`, `src/`, `vite`, `next`, `react`, `vue` | 前端或全栈 Web | `package.json`、route/page、API client、state、query builder |
| notebook、`requirements.txt`, `pyproject.toml`, `airflow`, `dbt`, `spark` | Python 数据、ML 或自动化 | 依赖文件、DAG/job 入口、model、transform、config |
| `Dockerfile`, `helm`, `terraform`, `k8s`, CI 文件 | 基础设施或部署型项目 | 部署清单、环境配置、流水线定义 |
| `Package.swift`, `.xcodeproj`, `.xcworkspace`, `*.swift` | iOS/macOS 应用或包 | package/project、应用入口、view、model、service |
| `Cargo.toml`, `src/main.rs`, `src/lib.rs` | Rust 服务、CLI 或库 | manifest、入口、模块、测试 |
| 多种强信号 | 混合工程或 monorepo | 先映射子项目，再按方向分别分析 |

分类只作为假设，不是最终结论。一个仓库可能包含多种工程类型。

## 项目盘点

分类后，先广度扫描，再按工程类型阅读。

优先阅读：

- README 和文档
- 构建清单和依赖文件
- 入口、路由、CLI、worker
- 控制器、处理器、service、repository、数据访问层
- 定时任务、worker、消费者、生产者、队列处理
- 查询构造、解析器、schema 管理、迁移
- 测试、部署文件、CI 和运行配置

如果用户工作区已有自动盘点脚本，只把脚本输出作为第一轮假设；重要结论必须再打开代表性文件确认。

## 证据目录

详细写作前先建立证据目录：

| 证据类型 | 查看位置 | 简历价值 |
| --- | --- | --- |
| 依赖 | manifest、lockfile、模块 | 技术栈和框架使用 |
| 入口 | main、router、CLI、worker | 运行形态和职责 |
| 领域模型 | schema、entity、DTO、migration | 业务对象和数据设计 |
| 流程控制 | service、orchestrator、pipeline、queue | 架构和执行流 |
| 可靠性 | retry、transaction、lock、idempotency、test | 生产可用性 |
| 运维 | config、CI、deployment、observability | 交付和可维护性 |

不要把“依赖存在”直接等同于“熟练掌握”。只有代码体现了实际使用方式，才值得写进简历。
