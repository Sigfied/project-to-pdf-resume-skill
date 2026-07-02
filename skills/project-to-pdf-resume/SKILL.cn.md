# 项目到 PDF 简历

## 概览

从本地项目文件夹生成技术简历时，先用代码证据提取架构、功能、技术栈和实现细节，再让用户确认成果、规模、职责、上线情况和业务价值。不要凭代码推断影响力，也不要编造成果。

默认产出是一组可追溯的简历材料：项目证据笔记、成果追问清单、打磨后的简历 bullet、源文件更新，以及在需要时生成并校验过的 PDF。

## 必走流程

1. 确认输入和目标。
   - 明确项目文件夹、目标岗位、资历层级、简历语言，以及需要项目素材、简历 bullet、源文件、PDF，还是全部。
   - 如果目标不清楚，可以从文件夹名或已有简历文件里谨慎推断，并说明这是暂定假设。
   - 私有信息只能出现在用户任务产物里，不要写入可复用的 Skill 文档。

2. 先盘点代码，再写简历。
   - 先查看主要文件类型和 manifest，判断工程类型：后端、前端、数据链路、移动端、CLI/工具、库、基础设施或混合工程。
   - 根据工程类型决定第一批要读的文件，不要把所有仓库都按同一种方式分析。
   - 使用 `rg --files`、`find`、构建清单、README、配置文件和入口文件进行扫描。
   - 优先阅读路由、控制器、处理器、service、repository、任务、worker、队列/缓存、SQL/查询构造、schema/迁移、测试和部署文件。
   - 在写任何简历结论前，先建立证据表。

3. 建立证据表。
   - 每个项目记录定位、技术栈、核心模块、请求/数据流、技术亮点、实现细节和证据文件。
   - 每条结论标记为 `Code evidence`、`Reasonable inference` 或 `Needs user confirmation`。
   - 职责、规模、上线使用、指标、业务价值、降本和性能提升，除非文档明确写出，否则都需要用户确认。
   - 把可复用项目素材和最终简历文案分开，方便生成多份不同方向的简历。

4. 追问缺失成果。
   - 代码证据过一遍之后再问，不要一开始就让用户空泛描述成果。
   - 问题要短，并基于已观察到的项目行为。
   - 优先追问上线使用、使用方、规模、可靠性、性能、成本、迁移、自动化和候选人的真实职责。
   - 涉及客户、内部系统、敏感指标时，要确认是否可以写进简历。

5. 草拟简历材料。
   - 匹配用户要求的语言；没有要求时，沿用已有简历语言。
   - 按“动作 + 方法 + 结果”组织 bullet。
   - 按目标岗位相关性、技术深度、已确认成果强度排序。
   - 在素材里保留不确定性；最终简历里删除未确认结论。

6. 生成并校验 PDF。
   - 最终简历排版优先使用 LaTeX，因为它更适合稳定字体、紧凑版式、可重复构建和高质量 PDF 输出。
   - 只有已有非 LaTeX 源文件已经是用户的标准简历流程，或用户明确要求该格式时，才优先沿用非 LaTeX。
   - 更新源文件，编译或渲染 PDF，检查页数；条件允许时把页面渲染成图片，检查间距、溢出、版面平衡和字体。
   - 反复迭代，直到 PDF 可读、紧凑且事实可靠。

## 参考文档

- 英文流程：[references/workflow.md](references/workflow.md)
- 中文流程：[references/workflow.cn.md](references/workflow.cn.md)
- 英文访谈与写作：[references/interview-and-writing.md](references/interview-and-writing.md)
- 中文访谈与写作：[references/interview-and-writing.cn.md](references/interview-and-writing.cn.md)
- 英文 PDF 生产：[references/pdf-production.md](references/pdf-production.md)
- 中文 PDF 生产：[references/pdf-production.cn.md](references/pdf-production.cn.md)
- 英文排版参考：[references/layout-reference.md](references/layout-reference.md)
- 中文排版参考：[references/layout-reference.cn.md](references/layout-reference.cn.md)

## 规则

- 不要把私有项目细节写进可复用 Skill 文档。
- 不要仅凭代码推断业务影响。
- 优先写项目特定实现细节，不要堆技术栈。
- 用户确认前，保留不确定性。
- 用户未要求重设计时，沿用现有简历风格。
- 没有渲染和检查过 PDF，不要声称完成。
- 代码、文档或用户确认都无法支撑的结论，不要写进最终简历。
