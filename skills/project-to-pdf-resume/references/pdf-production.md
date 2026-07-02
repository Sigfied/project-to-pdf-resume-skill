---
title: PDF Production
description: LaTeX-first source generation, PDF compilation, rendering, visual QA, and final output contract.
lang: en
version: 1.0.0
---

# PDF Production

Use this reference when generating or updating the final resume PDF.

## Output Contract

A PDF task is complete only when these are available:

- source document path
- rendered PDF path
- compile/render command or tool used
- page count
- visual inspection result
- remaining unconfirmed facts, if any

## Source Selection

Prefer LaTeX for final resume typesetting unless there is a strong reason not to. LaTeX is the default because resumes benefit from precise typography, stable page breaks, compact spacing, reproducible builds, and high-quality PDF output.

Use this source priority:

1. Existing LaTeX source that already produced the current resume.
2. Existing non-LaTeX source only when it is clearly the user's canonical resume workflow or the user requests it.
3. Existing PDF plus nearby source file, preferably converted or maintained in LaTeX when practical.
4. Existing Markdown resume material converted into LaTeX.
5. New compact LaTeX resume source.

Preserve the user's current visual style unless they ask for a redesign.

## Layout Choice

Default to LaTeX when:

- the user wants a polished PDF
- the resume is dense and needs tight spacing control
- multilingual font handling matters
- repeatable builds are important
- page count and line breaks need precise control

Use another format when:

- the user explicitly asks for DOCX, Markdown, HTML, or another format
- an existing non-LaTeX source is the authoritative resume
- the target platform requires a specific editable format
- local LaTeX tooling is unavailable and cannot be installed

When using LaTeX, prefer a compact resume template with semantic sections, reusable commands, consistent list spacing, and explicit font support for the resume language.

When the user provides existing resume PDFs or TEX sources as layout references, inspect them before changing layout. Extract reusable layout rules only: page size, margins, font scale, section hierarchy, list spacing, project heading structure, page-break strategy, and visual density. Do not copy private content from the samples into reusable Skill documentation.

For a compact Chinese technical resume style, read [layout-reference.md](layout-reference.md).

## Source Rules

- Use a LaTeX engine and template that support the resume language; for CJK resumes, prefer XeLaTeX/LuaLaTeX-compatible font handling when available.
- Keep section headings compact.
- Use short project headers: role, organization/project, and dates when available.
- Reduce visual clutter before shrinking text too aggressively.
- Avoid fragile manual spacing unless the PDF has been visually checked.
- Keep source paths and output paths predictable.

## Content Before Layout

Before touching layout:

1. remove unresolved claims from final bullets
2. rank projects by target-role relevance
3. shorten repeated stack descriptions
4. move detailed evidence into notes, not the final resume
5. confirm names, metrics, and sensitive details are approved

## Compile Or Render

Use the project's established compiler or renderer when visible. For LaTeX, prefer the existing repository command; otherwise use a reliable local command such as `tectonic` or `xelatex` when available.

If compilation or rendering fails:

1. Read the first real error, not only the final summary.
2. Fix missing packages, unsupported characters, font configuration, invalid markup, or layout overflow.
3. Recompile or rerender.

Common characters that may require escaping in LaTeX text:

| Character | Escape |
| --- | --- |
| `%` | `\%` |
| `&` | `\&` |
| `_` | `\_` |
| `#` | `\#` |

For non-LaTeX formats, use the equivalent escaping and font rules for the chosen renderer.

## Verify

Always verify the actual rendered PDF before saying it is finished.

Minimum checks:

1. Confirm the PDF exists.
2. Confirm the page count is expected.
3. Render pages to images when possible.
4. Inspect images for:
   - font rendering
   - clipped text
   - cramped or uneven spacing
   - section balance
   - bullets crossing margins
   - orphan lines and awkward page breaks
   - whether the strongest information appears early

## Visual QA Checklist

Inspect every page:

| Area | Check |
| --- | --- |
| Header | name, role, contact, and links are readable and not crowded |
| Summary | target-role signal appears early |
| Projects | strongest project is not buried |
| Bullets | no line spills beyond margins; indentation is consistent |
| Sections | headings are visually distinct but compact |
| Typography | fonts render correctly for the resume language |
| Density | page is full but not cramped |
| Privacy | sensitive names or numbers are generalized when required |

Useful commands vary by environment:

```bash
pdfinfo path/to/resume.pdf
pdftoppm -png -r 180 path/to/resume.pdf path/to/output-prefix
```

If a preferred PDF renderer is unavailable, use available PDF tooling to confirm page count and any available renderer for visual inspection.

## Common Fixes

| Problem | Fix |
| --- | --- |
| Extra page | remove weak bullets, repeated stack wording, and background prose |
| Dense project block | split long bullet, compress method clause, or move detail to notes |
| Weak first page | reorder projects or rewrite summary for the target role |
| Overflowing line | shorten wording before reducing font size |
| Poor font rendering | switch font or compiler/renderer settings |
| Inconsistent spacing | adjust list spacing or section spacing globally |
| Unclear impact | revise bullet with confirmed outcome or remove it |

## Iteration Rules

- If the PDF spills to an unwanted extra page, first tighten weak copy and remove repetition.
- If bullets are too dense, merge related technologies into one clause.
- If the first page lacks target-role signal, reorder projects or rewrite the summary.
- If a line overflows, shorten wording before forcing smaller font.
- If the PDF renders differently after moving machines, check fonts and compiler settings.

Stop iterating only when content, layout, and verification all pass. A successful compile alone is not enough.

## Delivery

Return:

- Final PDF path.
- Source path.
- A short summary of what changed.
- Any remaining facts that still need user confirmation.

Do not claim the PDF is final unless it was rendered and checked.
