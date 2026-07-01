# 端到端工作流

本文件对应 `workflow.md`，描述完整流程：项目文件夹 -> 代码分析 -> 架构和功能理解 -> 成果访谈 -> 简历写作 -> PDF。

## 产出物

按任务需要产出：

| 产出物 | 作用 |
| --- | --- |
| 输入摘要 | 记录项目文件夹、目标岗位、简历语言、输出形式和假设。 |
| 证据表 | 把每条简历结论关联到代码、文档、推断或用户确认。 |
| 架构笔记 | 描述组件边界、请求/数据流、存储、异步任务和运维机制。 |
| 成果问题 | 只追问会明显增强简历质量的缺失事实。 |
| 简历 bullet | 把已验证证据改写成面向岗位的简洁表述。 |
| 源文件 | 更新或创建简历源文件。 |
| 已校验 PDF | 页数和视觉检查通过后的最终渲染结果。 |

## 1. 输入确认

分析前确认：

| 字段 | 默认处理 |
| --- | --- |
| 项目文件夹 | 使用用户指定的文件夹；不明确时扫描可能的 workspace 根目录并说明假设。 |
| 目标岗位 | 用户未说明时，才从文件夹名或现有简历中谨慎推断。 |
| 简历语言 | 匹配用户要求或现有简历语言。 |
| 输出形式 | 确认需要素材、bullet、源文件、PDF，还是全部。 |
| 现有风格 | 有现有源文件或 PDF 风格时优先复用。 |

不要在输入阶段卡住指标缺失。先分析代码，再基于上下文追问成果。

如果用户给了多个文件夹，判断它们是同一简历方向还是多个方向。文件夹明显对应不同岗位、技术领域或资历叙事时，要拆成多个 track。

## 2. 工程类型扫描

深入读代码前，先通过文件名、扩展名和 manifest 判断主要工程类型。这样可以减少盲读，快速定位第一批高信号文件。

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

## 3. 项目盘点

分类后，先广度扫描，再按工程类型阅读：

```bash
rg --files <project-folder>
find <project-folder> -maxdepth 3 -type f
```

优先阅读：

- README 和文档
- 构建清单和依赖文件
- 入口、路由、CLI、worker
- 控制器、处理器、service、repository、数据访问层
- 定时任务、worker、消费者、生产者、队列处理
- 查询构造、解析器、schema 管理、迁移
- 测试、部署文件、CI 和运行配置

如果用户工作区已有自动盘点脚本，只把脚本输出作为第一轮假设；重要结论必须再打开代表性文件确认。

## 4. 证据目录

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

## 5. 证据表

阅读时维护简洁表格：

| 项目 | 证据 | 简历含义 | 状态 |
| --- | --- | --- | --- |
| project-a | 依赖文件显示语言、框架、存储 | 具备后端服务和持久化集成 | Code evidence |
| project-b | worker 入口串联输入、转换和输出模块 | 具备清晰运行流的数据处理链路 | Code evidence |
| project-c | 存在 UI 页面和查询构造模块 | 可能是用户侧分析流程或内部工具 | Reasonable inference |
| project-a | 已确认的可量化改进 | 强成果 bullet | Needs user confirmation |

状态含义：

- `Code evidence`：代码、依赖、配置、文档或测试中直接可见。
- `Reasonable inference`：架构或命名强烈暗示，但写作要谨慎。
- `Needs user confirmation`：指标、业务影响、上线使用、职责、规模、团队、节省、采用情况。

项目很大时，在工作笔记中增加 `Evidence files` 列。笔记里可以使用相对路径，但不要把私有路径写进可复用文档。

## 6. 架构理解

每个项目回答：

1. 它看起来解决什么问题？
2. 主要运行组件是什么？
3. 请求、事件或数据如何流转？
4. 涉及哪些存储、缓存、队列、外部服务或部署依赖？
5. 能看到哪些可靠性、并发、性能、安全或可维护性机制？
6. 哪些部分对目标岗位最值得写？

好的架构摘要应该具体但通用：

- `API layer -> service orchestration -> data access -> storage`
- `input source -> validation -> transformation -> destination`
- `request submission -> asynchronous execution -> status tracking -> result retrieval`

没有边界、机制和证据时，不要写“微服务架构”“高并发”“大规模”。

复杂架构按三层总结：

1. 一句话项目目的。
2. 一行组件流。
3. 带证据的实现亮点。

## 7. 简历方向选择

按目标岗位选择项目侧重点：

| 目标信号 | 强调内容 |
| --- | --- |
| 后端岗位 | API 设计、服务分层、存储、可靠性、异步任务、性能、部署。 |
| 数据岗位 | 采集、转换、建模、存储、查询、调度、数据质量、血缘。 |
| 全栈岗位 | 用户流程、API 集成、状态管理、产品完整度、交付。 |
| 平台岗位 | 可复用能力、配置化、多项目支持、可观测性、运维。 |
| AI/工具岗位 | 流程自动化、prompt/tool 集成、评估、安全、人审闭环。 |

如果多个方向都合理，创建多份草稿段落，不要强行写成一份泛泛简历。

## 8. 成果访谈

证据表完成后，只追问真正能增强 bullet 的缺失事实：

- 哪些项目上线或被真实用户使用？
- 使用者是谁：客户、内部团队、开发者、运营、分析师，还是其他群体？
- 能写哪些可量化结果：延迟、吞吐、错误率、稳定性、成本、效率、数据量、用户数、请求量、采用情况？
- 是否涉及迁移、替换、自动化、整合或降本？
- 候选人的职责是什么：负责人、核心开发、模块负责人、维护者、协作者，还是评审者？

没有数字时，写可信的定性成果：

- 提升可维护性
- 减少人工操作
- 将临时流程标准化
- 改善故障恢复或运维可见性
- 支撑明确用户群体的持续使用

## 9. 材料组织

最终简历前先组织中间材料：

1. `Project list`：目标岗位相关的代表项目。
2. `Tech stack`：技术栈加实际使用方式，不堆依赖。
3. `Architecture`：组件流和模块边界。
4. `Technical highlights`：说明实现为什么值得写。
5. `Implementation details`：文件、模式、事务、队列、缓存、任务、API、查询、测试或部署机制。
6. `Outcomes`：已确认成果在前，待确认问题在后。
7. `Resume bullets`：可直接进入最终简历的简洁 bullet。

中间材料方便用户在排版前纠正事实。

## 10. 简历草稿门槛

进入排版前确认：

- 每条最终 bullet 都有证据或用户确认。
- 最强项目靠前。
- 每个项目在目标岗位叙事中有清晰作用。
- 弱 bullet 已合并或删除。
- 未确认结论保留在笔记，不进入最终简历。
- 机密名称和指标使用已获批准的表述。

## 11. 收尾循环

持续循环直到完成：

1. 从已验证证据生成草稿 bullet。
2. 追问成果缺口。
3. 整合已确认成果。
4. 更新简历源文件。
5. 编译或渲染 PDF。
6. 检查渲染页面。
7. 修复间距、溢出、页数和弱 bullet。
8. 交付最终路径和未解决假设。
