# 端到端工作流

本文件对应 `workflow.md`，描述完整流程：项目文件夹 -> 代码分析 -> 架构和功能理解 -> 成果访谈 -> 简历写作 -> PDF。

## 产出物

按任务需要产出：

| 产出物 | 作用 |
| --- | --- |
| 输入摘要 | 记录项目文件夹、目标岗位、简历语言、输出形式和假设。 |
| 证据包 | 把每条简历结论关联到代码、文档、推断、用户确认、待追问问题和 bullet 候选。 |
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

## 5. 证据包

读项目时建立可复用的证据包。证据包要详细到能让另一个 agent 追溯每条最终简历 bullet 的来源，同时也要足够紧凑，方便用户审阅和补充。

项目有足够内容时，产出这些工作材料：

| 产物 | 必备内容 | 作用 |
| --- | --- | --- |
| `project-index` 项目索引 | track、项目文件夹/名称、工程类型假设、目标岗位匹配点、高信号文件、审阅状态 | 管理多个文件夹和多个简历方向。 |
| `file-evidence-index` 文件证据索引 | 相对路径、证据类别、观察到的事实、为什么重要、关联 claim ID | 避免空泛结论，把观察绑定到具体文件。 |
| `project-evidence-card` 项目证据卡 | 问题、架构流、核心模块、实际使用技术栈、可靠性/性能机制、候选人贡献、已确认成果、未知项 | 在写简历前，形成每个项目的紧凑技术叙事。 |
| `claim-ledger` 结论台账 | 结论、证据状态、来源、置信度、简历写法、追问问题、最终决策 | 追踪哪些结论可直接使用、需要保守表述、待确认或应删除。 |
| `outcome-question-backlog` 成果追问队列 | 问题、为什么重要、可能解锁的 bullet、优先级 | 把缺失的上线/业务事实变成聚焦问题。 |
| `resume-bullet-candidates` 简历 bullet 候选池 | 草稿 bullet、来源 claim、目标岗位、强度、阻塞项 | 把证据收集和最终文案写作拆开。 |

### 项目索引

每个文件夹或子项目一行：

| Track | 项目 | 类型假设 | 目标岗位匹配 | 高信号文件 | 状态 |
| --- | --- | --- | --- | --- | --- |
| Backend | project-a | Go service | API、存储、异步任务 | `go.mod`, `cmd/...`, `internal/...` | 可进入证据卡 |
| Data | project-b | Python pipeline | 采集、转换、调度 | `pyproject.toml`, `jobs/...`, `models/...` | 还需继续读源码 |

类型判断先作为假设，直到架构证据能支撑它。

### 文件证据索引

用文件证据索引让读代码过程可审计：

| 项目 | 文件 | 证据类别 | 观察到的事实 | 为什么重要 | Claim ID |
| --- | --- | --- | --- | --- | --- |
| project-a | `src/.../handler.ext` | 入口 | 请求进入 service 模块 | 说明运行职责边界 | C-001 |
| project-a | `src/.../repository.ext` | 数据访问 | 封装存储查询 | 支撑持久化和分层结论 | C-002 |
| project-b | `jobs/.../transform.ext` | 数据流 | 输入记录先校验再转换 | 支撑 pipeline 实现结论 | C-003 |

优先使用相对路径和简短观察，不要把本机绝对路径写进可复用材料。

### 项目证据卡

每个值得写进简历的项目建立一张紧凑卡片：

| 字段 | 写什么 |
| --- | --- |
| 项目目的 | 用一句话说明代码看起来解决的问题。 |
| 架构流 | 一行表达，例如 `API -> service -> repository -> storage` 或 `source -> validation -> transform -> sink`。 |
| 核心模块 | 主要目录、类、函数及其职责。 |
| 实际使用技术栈 | 有实际使用证据的技术，不只是依赖列表。 |
| 技术亮点 | 代码中可见的可靠性、性能、并发、数据质量、安全、工具化或可维护性机制。 |
| 候选人贡献 | 负责程度或可能贡献；未获用户确认前标记为待确认。 |
| 已确认成果 | 用户确认过的指标、上线情况、用户影响、采用情况、节省或交付结果。 |
| 未知项 | 会明显增强简历的开放问题。 |

### 结论台账

把结论台账作为主证据表维护：

| Track | 项目 | Claim ID | 结论 | 证据状态 | 来源 | 观察到的事实 | 简历用途 | 置信度 | 是否需确认 | 追问 | 决策 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Backend | project-a | C-001 | 服务暴露请求处理并委托业务逻辑 | Code evidence | `src/.../handler.ext` | handler 调用 service 层 | bullet 技术基础 | High | 否 | - | Use |
| Backend | project-a | C-002 | 项目提升了延迟或吞吐 | Needs user confirmation | user interview | 代码中看不到指标 | 成果 bullet | Low | 是 | 上线后发生了什么变化？ | Pending |
| Data | project-b | C-003 | pipeline 在转换前校验输入 | Code evidence | `jobs/.../transform.ext` | validation 发生在 transform 前 | bullet 技术基础 | Medium | 否 | - | Use cautiously |

统一使用这些决策：

- `Use`：可以进入最终简历表述。
- `Use cautiously`：只能用保守措辞写入。
- `Ask user`：需要用户确认成果、职责、上线或规模。
- `Hold`：保留在笔记里，暂不进入简历。
- `Reject`：证据弱或不相关，从最终简历删除。

### 成果追问队列

把待确认结论转换成有目标的问题：

| 优先级 | 项目 | 问题 | 为什么重要 | 可解锁 bullet |
| --- | --- | --- | --- | --- |
| High | project-a | 是否上线或支持真实用户？ | 区分 demo 和真实交付。 | 生产交付 bullet |
| High | project-a | 有没有可量化前后对比？ | 把实现转化为影响。 | 带指标成果 |
| Medium | project-b | 处理过的数据量或调度频率是多少？ | 增加规模和运维上下文。 | 数据 pipeline 规模 bullet |

只问会改变最终简历写法的问题，不在简历阶段追问泛泛的好奇问题。

### 简历 bullet 候选池

只有当 claim 已经有来源时，才开始起草 bullet：

| 目标岗位 | 项目 | 草稿 bullet | 来源 claim | 强度 | 阻塞项 |
| --- | --- | --- | --- | --- | --- |
| Backend | project-a | Built a layered service flow covering request handling, business orchestration, and persistence. | C-001, C-002 | Medium | 需要确认成果 |
| Data | project-b | Implemented a validation-first data processing flow from input ingestion to transformed output. | C-003 | Medium | 需要规模或使用场景 |

候选池不是最终简历文案，而是用来比较措辞、岗位匹配度和缺失证据的中间区。

### 完成标准

证据包满足这些条件后，才足够进入简历草稿：

- 每条最终 bullet 候选至少关联一个 claim ID。
- 每个 claim ID 都能追溯到代码、文档、合理推断或用户确认。
- 指标、上线使用、职责范围、规模和业务影响已由用户确认，或从最终文案移除。
- 假设保持可见，但不会在最终简历中伪装成事实。
- 可复用文档中没有本机私有绝对路径。
- 弱结论被标记为 `Hold` 或 `Reject`，不会漏进简历。
- 不需要重新读完整项目，就能找到最强证据。

### 小项目精简证据表

小项目或快速分析时，可以用精简证据表替代完整证据包：

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

证据包完成后，只追问真正能增强 bullet 的缺失事实：

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
