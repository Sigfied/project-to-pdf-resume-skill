# Layout Reference

Use this reference when the final resume should follow a compact Chinese technical resume style or when the user provides existing PDF/TEX resumes as layout samples.

## Source Policy

If sample PDFs and TEX files are provided:

1. Render the PDFs and inspect every page visually.
2. Read the TEX source for layout parameters and reusable commands.
3. Extract only layout rules, not private resume content.
4. Preserve the user's visual style unless they ask for a redesign.
5. Keep the resulting Skill documentation generic and publishable.

## Preferred Compact LaTeX Style

Use LaTeX as the default source format for final PDF resumes. For Chinese technical resumes, prefer a compact CJK-capable setup similar to:

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

Adjust values only after rendering and visual inspection.

## Page And Typography

- Use A4 paper.
- Use a compact base size around `10.5pt`.
- Use `\small` for body content when density is high.
- Hide page numbers for short resumes.
- Use tight but readable margins: about `1.3cm` left/right, `1.1-1.2cm` top, and `1.0cm` bottom.
- Use zero paragraph indentation and zero paragraph skip.
- Use line spread around `1.06` to keep Chinese and mixed English text readable.
- Use hidden hyperlinks so contact links do not add visual noise.

## Section Hierarchy

Use a restrained hierarchy:

1. Name/header: largest and boldest.
2. Major sections: bold, compact, no decorative rules unless the existing resume uses them.
3. Project heading: project name, role, and date on one horizontal line.
4. Project sublabels: bold labels such as `Overview`, `Tech Stack`, `Responsibilities`, and `Outcomes`.
5. Bullets: numbered lists for responsibilities/outcomes when order matters; itemized lists for skills.

Avoid cards, icons, heavy borders, colored blocks, or decorative visual elements unless the user's existing style already uses them.

## Project Block Pattern

For each major project:

1. One-line project header:
   - left: project name
   - middle: role or ownership level
   - right: date range
2. Short project overview.
3. Technical route/stack line.
4. Responsibilities as numbered bullets.
5. Outcomes as numbered bullets when confirmed.

Keep labels consistent across projects. This makes dense resumes scannable.

## Page Break Strategy

- Target one or two pages unless the user requests otherwise.
- For a two-page technical resume, it is acceptable to manually start a new major project on page two.
- Prefer keeping a project block intact over filling every blank space.
- Do not split a project heading from its first label.
- Do not let education or supplemental sections crowd a strong project section.

## Density Rules

- Use compact list spacing: `itemsep` and `topsep` around `3pt`.
- Keep labels close to the text they introduce.
- Merge weak bullets before reducing font size.
- Remove repeated stack wording before tightening margins.
- Preserve readable line height even in dense resumes.
- Leave some bottom whitespace if forcing more content would hurt readability.

## Visual QA

Render the PDF to images and check:

- header alignment and contact readability
- consistent left edge and bullet indentation
- clear distinction between major sections, project headers, labels, and bullets
- no clipped CJK glyphs or awkward mixed Chinese/English spacing
- no section heading stranded at the bottom of a page
- no project header separated from its body
- page two starts with meaningful content when a manual break is used

Successful compilation is not enough. The layout reference is satisfied only after visual inspection.
