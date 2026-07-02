# Project To PDF Resume Skill 中文说明

[English](README.md) | [简体中文](README.zh-CN.md)

这是一个证据优先的标准 Agent Skill，用于把本地项目文件夹整理成技术简历素材、LaTeX 简历源文件和经过校验的 PDF 简历。

它强调先读代码，再写简历：架构、功能、技术栈和实现细节来自代码证据；上线效果、规模、职责、指标和业务价值必须由用户确认。

仓库采用通用 Skill 结构：可安装目录中包含带 YAML frontmatter 的 `SKILL.md` 和可选的 `references/` 文件，不绑定某一个 agent 运行时。

## 关于项目

`project-to-pdf-resume` 是一个可移植的 Agent Skill，用来从真实项目仓库生成技术简历材料。它要求 agent 先阅读代码、判断工程类型、建立证据包，再向用户追问缺失的上线效果，最后整理成面向岗位的简历 bullet，并在需要时生成经过校验的 PDF。

它适合那些不能凭印象写简历、必须从代码和项目产物中提取可信材料的场景。架构、实现细节、技术栈和亮点来自代码证据；上线情况、职责范围、指标、规模和业务价值必须由用户确认。

最终工作流会产出：

- 证据包：项目索引、文件证据索引、项目证据卡、结论台账、成果追问队列和 bullet 候选池
- 区分事实、推断和待确认内容的岗位定向简历 bullet
- 需要 PDF 输出时，优先使用 LaTeX 的简历源文件
- PDF 页数、间距、溢出和可读性校验流程
- 英文和简体中文两套对应文档

## 功能

- 扫描一个或多个本地项目文件夹。
- 在深入分析前，先判断主要工程类型。
- 提取架构、实现细节、技术亮点和证据文件。
- 建立 Markdown + JSON 证据包：Markdown 保存详细证据，JSON 保存 metadata、文档索引、claim 关系、状态和可用性。
- 将结论标记为 `Code evidence`、`Reasonable inference` 或 `Needs user confirmation`。
- 针对上线使用、规模、职责、指标和成果提出聚焦问题。
- 基于已验证证据草拟简历 bullet。
- PDF 排版优先使用 LaTeX，并要求渲染后检查。
- 包含适合中文技术简历的紧凑 LaTeX 排版参考。
- 每份主要 Skill 文档都有对应的简体中文版本。

## 仓库结构

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
├── .gitignore
└── skills/
    └── project-to-pdf-resume/
        ├── SKILL.md
        ├── SKILL.cn.md
        └── references/
            ├── workflow.md
            ├── workflow.cn.md
            ├── intake-and-scan.md
            ├── intake-and-scan.cn.md
            ├── evidence-package.md
            ├── evidence-package.cn.md
            ├── architecture-and-materials.md
            ├── architecture-and-materials.cn.md
            ├── interview-and-writing.md
            ├── interview-and-writing.cn.md
            ├── layout-reference.md
            ├── layout-reference.cn.md
            ├── pdf-production.md
            └── pdf-production.cn.md
```

可安装的 Skill 位于 `skills/project-to-pdf-resume/`。仓库级 README 和 License 放在 Skill 目录外，保证 Skill 本身保持紧凑、可移植。

Skill 目录不包含 Codex-only 或 OpenAI-only 这类运行时专用 metadata。如果某个 agent 运行时支持额外 metadata，可以在自己的环境中添加，不必改动便携 Skill 核心。

`SKILL.md` 包含标准 Skill 必需的 frontmatter。每份 bundled reference 也包含轻量 YAML metadata（`title`、`description`、`lang`、`version`），方便 agent 快速判断是否需要加载。

## 安装

克隆仓库：

```bash
git clone https://github.com/Sigfied/project-to-pdf-resume-skill.git
```

把可安装 Skill 目录复制到你的 agent runtime 使用的 skills 目录：

```bash
mkdir -p /path/to/your/skills
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume /path/to/your/skills/
```

如果你的 agent 支持从仓库路径加载 Skill，指向：

```text
skills/project-to-pdf-resume
```

然后重启 agent、重新加载 skills，或按你的运行时要求触发发现流程，让 `SKILL.md` 被索引。

### 运行时示例

Codex 风格的本地安装：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume "${CODEX_HOME:-$HOME/.codex}/skills/"
```

其他运行时：

```bash
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume <your-agent-skills-directory>/
```

关键是复制整个包含 `SKILL.md` 的目录，而不是只复制单个 Markdown 文件，因为该 Skill 依赖 `references/`。

## 使用示例

让你的 agent 对一个或多个本地项目文件夹使用该 Skill：

```text
使用 project-to-pdf-resume skill，分析这些项目文件夹，并整理成后端岗位简历 PDF。
```

```text
使用这个 Skill，先检查代码，只追问我缺失的上线效果，然后草拟简历 bullet 并生成 LaTeX PDF。
```

```text
使用这个 Skill 的中文文档，用简体中文生成简历材料。
```

## 工作流概览

1. 确认项目文件夹、目标岗位、简历语言和期望输出。
2. 根据文件扩展名和 manifest 判断主要工程类型。
3. 按工程类型阅读高信号文件。
4. 写结论前先建立证据包。
5. 追问缺失的成果和职责细节。
6. 写面向目标岗位的简历 bullet。
7. PDF 排版优先使用 LaTeX。
8. 编译或渲染 PDF，并进行视觉检查。

## 校验

基础本地检查：

```bash
ruby -ryaml -e 's=File.read("skills/project-to-pdf-resume/SKILL.md"); fm=s[/\A---\n(.*?)\n---\n/m,1]; y=YAML.safe_load(fm); abort("bad name") unless y["name"] =~ /\A[a-z0-9-]+\z/; abort("missing description") if y["description"].to_s.empty?; puts "ok"'
```

如果你安装了官方 Skill 校验脚本及其依赖，可以运行：

```bash
python3 path/to/quick_validate.py skills/project-to-pdf-resume
```

预期的便携 Skill 文件：

```bash
find skills/project-to-pdf-resume -maxdepth 3 -type f | sort
```

## 隐私和准确性规则

- 不要把私有项目细节写进可复用 Skill 文档。
- 不要仅凭代码推断业务影响。
- 指标、上线使用、职责、规模和业务价值都视为需要用户确认的事实。
- 不确定结论保留在笔记中，不要进入最终简历 bullet。
- 客户、内部系统和敏感指标需要泛化，除非用户明确批准精确写法。

## 中文文档

每份主要 Skill 文档都有对应的简体中文版本：

- `SKILL.md` -> `SKILL.cn.md`
- `workflow.md` -> `workflow.cn.md`
- `intake-and-scan.md` -> `intake-and-scan.cn.md`
- `evidence-package.md` -> `evidence-package.cn.md`
- `architecture-and-materials.md` -> `architecture-and-materials.cn.md`
- `interview-and-writing.md` -> `interview-and-writing.cn.md`
- `layout-reference.md` -> `layout-reference.cn.md`
- `pdf-production.md` -> `pdf-production.cn.md`

## License

MIT License. See [LICENSE](LICENSE).
