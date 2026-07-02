# Project To PDF Resume Skill

An evidence-first Agent Skill for turning local project folders into technical resume material, LaTeX-based resume sources, and verified PDF resumes.

The Skill is designed for resume workflows where an agent must inspect code before writing, separate code-backed facts from user-confirmed outcomes, and avoid inventing metrics, ownership, production usage, or business impact.

This repository uses the standard Skill shape: an installable directory containing `SKILL.md` with YAML frontmatter, plus optional bundled reference files. It is not tied to one specific agent runtime.

## What It Does

- Scans one or more local project folders.
- Detects the dominant project type before deep analysis.
- Extracts architecture, implementation details, technical highlights, and evidence files.
- Builds an evidence package with project indexes, file evidence indexes, project evidence cards, claim ledgers, outcome question backlogs, and bullet candidates.
- Labels claims as `Code evidence`, `Reasonable inference`, or `Needs user confirmation`.
- Asks targeted questions about production usage, scale, responsibility, metrics, and outcomes.
- Drafts resume bullets from verified evidence.
- Prefers LaTeX for final PDF typesetting and requires rendered PDF verification.
- Includes compact LaTeX layout guidance for Chinese technical resumes and user-provided PDF/TEX layout samples.
- Includes matching Simplified Chinese documentation for each Skill document.

## Repository Layout

```text
.
├── README.md
├── LICENSE
├── .gitignore
└── skills/
    └── project-to-pdf-resume/
        ├── SKILL.md
        ├── SKILL.cn.md
        └── references/
            ├── workflow.md
            ├── workflow.cn.md
            ├── interview-and-writing.md
            ├── interview-and-writing.cn.md
            ├── layout-reference.md
            ├── layout-reference.cn.md
            ├── pdf-production.md
            └── pdf-production.cn.md
```

The installable Skill is contained in `skills/project-to-pdf-resume/`. The repository-level README and license are intentionally kept outside the Skill folder so the Skill itself stays compact and portable.

The Skill folder intentionally contains no runtime-specific metadata such as Codex-only or OpenAI-only configuration. If an agent runtime supports optional metadata, add that in your own environment without changing the portable Skill core.

## Installation

Clone the repository:

```bash
git clone https://github.com/Sigfied/project-to-pdf-resume-skill.git
```

Copy the installable Skill folder into the skills directory used by your agent runtime:

```bash
mkdir -p /path/to/your/skills
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume /path/to/your/skills/
```

If your agent supports loading Skills from a repository path, point it at:

```text
skills/project-to-pdf-resume
```

Start a new agent session, reload skills, or follow your runtime's discovery process so `SKILL.md` is indexed.

### Runtime Examples

Codex-style local install:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Other runtimes:

```bash
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume <your-agent-skills-directory>/
```

Use the directory that contains `SKILL.md`; do not copy only the Markdown file, because the Skill depends on `references/`.

## Usage Examples

Ask your agent to use the Skill on one or more local folders:

```text
Use the project-to-pdf-resume skill to analyze these project folders and turn them into a backend resume PDF.
```

```text
Use this Skill to inspect the code first, ask me only for missing production outcomes, then draft resume bullets and produce a LaTeX PDF.
```

```text
Use the Chinese docs for this Skill and generate resume material in Simplified Chinese.
```

## Workflow Summary

1. Confirm project folders, target role, resume language, and desired outputs.
2. Identify the dominant project type from file extensions and manifests.
3. Read high-signal files based on the project type.
4. Build an evidence package before drafting claims.
5. Ask the user for missing outcomes and responsibility details.
6. Write target-role-specific resume bullets.
7. Prefer LaTeX for final PDF typesetting.
8. Compile or render the PDF and visually verify the output.

## Verification

Basic local checks:

```bash
ruby -ryaml -e 's=File.read("skills/project-to-pdf-resume/SKILL.md"); fm=s[/\A---\n(.*?)\n---\n/m,1]; y=YAML.safe_load(fm); abort("bad name") unless y["name"] =~ /\A[a-z0-9-]+\z/; abort("missing description") if y["description"].to_s.empty?; puts "ok"'
```

If you have the official Skill validation script and its dependencies installed, run:

```bash
python3 path/to/quick_validate.py skills/project-to-pdf-resume
```

Expected portable Skill files:

```bash
find skills/project-to-pdf-resume -maxdepth 3 -type f | sort
```

## Privacy And Accuracy Rules

- Do not put private project details into reusable Skill documentation.
- Do not infer business impact from code alone.
- Treat metrics, production usage, ownership, scale, and business value as user-confirmed facts.
- Keep uncertain claims in notes, not final resume bullets.
- Generalize confidential names, customers, internal systems, and sensitive metrics unless the user explicitly approves exact wording.

## Chinese Documentation

Each primary Skill document has a Simplified Chinese counterpart:

- `SKILL.md` -> `SKILL.cn.md`
- `workflow.md` -> `workflow.cn.md`
- `interview-and-writing.md` -> `interview-and-writing.cn.md`
- `layout-reference.md` -> `layout-reference.cn.md`
- `pdf-production.md` -> `pdf-production.cn.md`

## License

MIT License. See [LICENSE](LICENSE).

---

# Project To PDF Resume Skill 中文说明

这是一个标准 Agent Skill，用于把本地项目文件夹整理成技术简历素材、LaTeX 简历源文件和经过校验的 PDF 简历。

它强调先读代码，再写简历：架构、功能、技术栈和实现细节来自代码证据；上线效果、规模、职责、指标和业务价值必须由用户确认。

仓库采用通用 Skill 结构：可安装目录中包含 `SKILL.md` 和 `references/`，不绑定某一个 agent 运行时。

## 安装

```bash
git clone https://github.com/Sigfied/project-to-pdf-resume-skill.git
mkdir -p /path/to/your/skills
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume /path/to/your/skills/
```

关键是复制整个包含 `SKILL.md` 的目录，而不是只复制单个文件。

Codex 风格的本地安装示例：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R project-to-pdf-resume-skill/skills/project-to-pdf-resume "${CODEX_HOME:-$HOME/.codex}/skills/"
```

然后重启 agent、重新加载 skills，或按你的运行时要求触发发现流程。

## 使用

```text
使用 project-to-pdf-resume skill，分析这些项目文件夹，先读代码，再问我缺失的上线效果，最后整理成简历描述并生成 PDF。
```

默认会优先使用 LaTeX 做最终 PDF 排版；只有在用户明确要求其他格式，或已有非 LaTeX 源文件是权威简历流程时，才使用其他格式。
