<picture>
  <source media="(prefers-color-scheme: dark)" srcset="readme-assets/header-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="readme-assets/header-light.svg">
  <img alt="Formula Rendering and Export Workflow · ✦ EricMingle69" src="readme-assets/header-light.svg" width="100%">
</picture>

<p align="center">
  <a href="README.md">简体中文</a> · <a href="README.en.md">English</a> · <a href="PERSONAL-NOTICE.md">✦ EricMingle69</a>
</p>

# Formula Rendering and Export Workflow

## Purpose

A skill contract and showcase for exporting Markdown display-math formulas individually as SVG and PNG and mapping files through a manifest.

## Repository guide

| Entry | Contents |
| --- | --- |
| [Input/output contract](SKILL.md) | Formula parsing, semantic checks and output requirements |
| [Development notes](README_DEV.md) | Implementation background and future directions |
| [Artifact examples](作品渲染图/) | Input, check, SVG, PNG and manifest previews |
| [Workflow cover](作品封面图.png) | Stored presentation image |

## Getting started

1. Prepare Markdown containing `$$...$$` formula blocks and define input files and output directories according to the skill.
2. Perform the documented TeX semantic checks, generate SVG with MathJax, and convert PNG with Python/cairosvg.
3. Write separate files per formula and map identifiers to filenames in a manifest; verify completeness and counts individually.

## Scope and limitations

- The repository contains documents and images, without directly runnable rendering scripts, dependency lists or an installer.
- The operator must still prepare Node.js/MathJax and Python/cairosvg environments and the required implementation.
- Inline math, PDF output and multi-Markdown batches are development directions rather than delivered capabilities.
- Screenshots illustrate output forms and do not establish correct rendering of arbitrary LaTeX input.

## Sources and existing licenses

MathJax, cairosvg and other tools retain their own provenance and licenses. The original repository has no LICENSE/NOTICE; reuse rights for existing documents, formula examples and images are unspecified.

---

Documentation maintained by **✦ EricMingle69** · [Ming-Sir-69](https://github.com/Ming-Sir-69)  
[Personal identity, licensing and permissions](PERSONAL-NOTICE.md) · The header follows your GitHub theme.
