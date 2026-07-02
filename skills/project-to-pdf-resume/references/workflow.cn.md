---
title: 端到端工作流
description: 将项目文件夹转换为有证据支撑的简历材料和 PDF 输出的阶段路由。
lang: zh-CN
version: 1.0.0
---

# 端到端工作流

把本文件当作工作流路由，而不是唯一参考文档。只有当前任务进入对应阶段时，才加载更深的 reference。

## 阶段地图

| 阶段 | 目标 | 下一步读取 |
| --- | --- | --- |
| 输入确认 | 确认项目文件夹、目标岗位、语言、输出形式和假设。 | [intake-and-scan.cn.md](intake-and-scan.cn.md) |
| 工程类型扫描 | 深入阅读前，先判断主要文件类型和 manifest。 | [intake-and-scan.cn.md](intake-and-scan.cn.md) |
| 代码盘点 | 按工程类型阅读高信号文件。 | [intake-and-scan.cn.md](intake-and-scan.cn.md) |
| 证据包 | 产出 Markdown 证据文档和 JSON metadata/index 文件。 | [evidence-package.cn.md](evidence-package.cn.md) |
| 架构综合 | 总结项目目的、组件流、技术亮点和岗位匹配。 | [architecture-and-materials.cn.md](architecture-and-materials.cn.md) |
| 成果访谈 | 只追问缺失的上线、规模、职责、指标或影响事实。 | [interview-and-writing.cn.md](interview-and-writing.cn.md) |
| 简历写作 | 把已验证结论转换成面向岗位的 bullet。 | [interview-and-writing.cn.md](interview-and-writing.cn.md) |
| PDF 生产 | 更新源文件，优先 LaTeX，编译/渲染并校验输出。 | [pdf-production.cn.md](pdf-production.cn.md) |
| 排版细化 | 需要中文紧凑技术简历版式时应用排版参考。 | [layout-reference.cn.md](layout-reference.cn.md) |

英文任务按同一阶段读取对应 `.md` 文件。

## 必要产物

只产出用户当前输出目标需要的材料，但内部追溯关系必须保留。

| 产物 | 作用 | 默认格式 |
| --- | --- | --- |
| 输入摘要 | 记录文件夹、目标岗位、简历语言、输出形式和假设。 | Markdown 或 JSON object |
| 项目扫描 | 记录工程类型假设和高信号文件。 | Markdown table 或 JSON array |
| 证据包 | 把结论关联到文件、推断状态、追问和 bullet 候选。 | Markdown 详情 + JSON metadata/index |
| 架构笔记 | 解释运行流、模块边界、实际技术栈和亮点。 | Markdown |
| 成果问题 | 聚焦会增强最终 bullet 的用户确认事实。 | Markdown list 或 JSON array |
| 简历 bullet | 把已验证证据转成简洁的岗位定向表述。 | Markdown |
| 源文件 | 更新或创建简历源文件。 | PDF 输出优先 LaTeX |
| 已校验 PDF | 页数和视觉检查通过后的最终渲染结果。 | PDF |

## 执行循环

1. 确认输入和目标输出。
2. 深入读代码前先扫描工程类型。
3. 盘点高信号文件并建立证据目录。
4. 用 claim ID 和 source link 建立证据包。
5. 综合架构和目标岗位定位。
6. 只针对缺失证据追问成果。
7. 从已验证结论生成简历 bullet。
8. 需要 PDF 时生产源文件和 PDF。
9. 校验渲染输出并迭代，直到可读、紧凑且事实可靠。

## 停止条件

以下结论不要进入最终简历：

- 没有代码、文档、合理推断或用户确认支撑
- 未经用户确认就声称指标、规模、上线使用、职责、节省或业务价值
- 依赖用户尚未批准的私密表述
- 把项目写得比证据支持的规模或资历更大
