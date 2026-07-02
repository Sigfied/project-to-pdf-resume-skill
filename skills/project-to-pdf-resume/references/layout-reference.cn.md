---
title: 排版参考
description: 紧凑中文技术简历排版指导，以及安全参考用户提供的 PDF/TEX 排版样本。
lang: zh-CN
version: 1.0.0
---

# 排版参考

当最终简历需要采用紧凑中文技术简历风格，或用户提供现有 PDF/TEX 简历作为排版样例时，读取本文件。

## 样例使用原则

如果用户提供 PDF 和 TEX 样例：

1. 先渲染 PDF，逐页做视觉检查。
2. 再读取 TEX 源文件，提取边距、字号、列表、section 和宏定义。
3. 只提炼排版规则，不复制私人简历内容。
4. 用户未要求重设计时，保留现有视觉风格。
5. 写入 Skill 文档的内容必须通用、可公开发布。

## 推荐的紧凑 LaTeX 风格

最终 PDF 简历默认使用 LaTeX。中文技术简历优先采用支持 CJK 的紧凑配置，例如：

```tex
\documentclass[10.5pt,a4paper]{ctexart}
\usepackage[margin=1.34cm,top=1.18cm,bottom=1.02cm]{geometry}
\usepackage{enumitem}
\usepackage{titlesec}
\usepackage{hyperref}
\pagenumbering{gobble}
\hypersetup{hidelinks}

\setlength{\parindent}{0pt}
\setlength{\parskip}{0pt}
\linespread{1.06}
\setlist[itemize]{leftmargin=1.65em,itemsep=3pt,topsep=3pt,parsep=0pt}
\setlist[enumerate]{leftmargin=1.75em,itemsep=3pt,topsep=3pt,parsep=0pt}
\titleformat{\section}{\normalsize\bfseries}{}{0em}{}
\titlespacing*{\section}{0pt}{10pt}{6pt}
\newcommand{\project}[3]{\vspace{5pt}\textbf{\normalsize #1}\hfill #2\hfill #3\par\vspace{2pt}}
\newcommand{\labeltext}[1]{\vspace{4pt}\textbf{#1}\par\vspace{2pt}}
```

具体数值必须在渲染和视觉检查后再微调。

## 页面与字体

- 使用 A4。
- 基础字号约 `10.5pt`。
- 内容密度较高时，正文使用 `\small`。
- 短简历隐藏页码。
- 边距紧凑但可读：左右约 `1.3cm`，顶部约 `1.1-1.2cm`，底部约 `1.0cm`。
- 段首不缩进，段间距为 0。
- 行距约 `1.06`，保证中文和中英混排可读。
- 超链接隐藏样式，避免联系方式区域产生视觉噪音。

## 层级结构

采用克制的层级：

1. 姓名/页眉最大且加粗。
2. 一级 section 加粗、紧凑，不默认使用横线装饰。
3. 项目标题一行三段：项目名、角色、时间。
4. 项目内小标签加粗，例如 `项目简介`、`技术路线`、`个人职责`、`项目成果`。
5. 职责和成果适合用编号列表；技能适合用项目符号列表。

除非用户现有风格已经使用，否则不要加入卡片、图标、粗边框、色块或装饰元素。

## 项目块模式

每个主要项目使用固定结构：

1. 一行项目标题：
   - 左侧：项目名
   - 中间：角色或职责等级
   - 右侧：时间范围
2. 简短项目简介。
3. 技术路线/技术栈一行。
4. 编号列出个人职责。
5. 有确认成果时，编号列出项目成果。

不同项目的小标签保持一致，这样密集简历仍然容易扫读。

## 分页策略

- 默认控制在一页或两页，除非用户另有要求。
- 两页技术简历中，可以手动让重要项目从第二页开始。
- 优先保证项目块完整，不要为了填满页面而破坏阅读节奏。
- 不要让项目标题和第一个标签分离。
- 不要让教育经历或补充信息挤压核心项目。

## 密度规则

- 列表间距保持紧凑，`itemsep` 和 `topsep` 可在 `3pt` 左右。
- 标签和其对应文本要靠近。
- 先合并弱 bullet，再考虑缩小字号。
- 先删除重复技术栈，再考虑收窄边距。
- 即使内容密集，也要保留可读行高。
- 如果继续塞内容会伤害可读性，允许底部留白。

## 视觉 QA

渲染 PDF 为图片后检查：

- 页眉对齐和联系方式可读性
- 左边缘和 bullet 缩进是否一致
- 一级 section、项目标题、标签和 bullet 层级是否清晰
- CJK 字形是否截断，中英混排是否别扭
- section 标题是否孤立在页底
- 项目标题是否和正文分离
- 使用手动分页时，第二页开头是否是有意义的内容

编译成功不等于排版完成。只有通过视觉检查，才算满足排版参考。
