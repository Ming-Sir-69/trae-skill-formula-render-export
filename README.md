# Trae Skill: Formula Render & Export

## 概述

一个面向 **数学公式（Markdown/LaTeX）→ 渲染 → 导出视觉资产** 的工程化流程 Skill，专为 [Trae SOLO](https://solo.trae.ai/) AI 编程助手设计。

## 核心能力

- **TeX 语义体检**：自动检测 `\\text{}` 等常见 LaTeX 误用，确保渲染结果正确
- **逐公式渲染**：每条 `$$...$$` 公式独立输出为 SVG（矢量）和 PNG（位图），支持精确引用和复用
- **可追溯 manifest**：自动生成 `manifest.json`，记录 `index → TeX → filename` 的完整映射
- **质量门禁**：内置 DoD 检查，确保每个输出文件完整、无截断、无乱码

## 工作原理

```
输入 Markdown（含 $$...$$ 公式块）
        │
        ▼
  Step 1: TeX 语义体检
  （检测并修复常见 LaTeX 误写）
        │
        ▼
  Step 2: MathJax 直出 SVG
  （Node.js mathjax-full，TeX→SVG）
        │
        ▼
  Step 3: SVG → PNG 转换
  （Python cairosvg，dpi 可控）
        │
        ▼
  输出：svg_out_clean/ + png_out_clean/ + manifest.json
```

## 使用方式

1. 将 `SKILL.md` 放入 Trae SOLO 的 `.trae/skills/` 目录下
2. 在对话中提及「渲染公式」「LaTeX」「MathJax」「公式导出」等关键词即可触发
3. 提供包含 `$$...$$` 公式块的 Markdown 文件，Skill 将自动完成渲染与导出

## 技术栈

| 环节 | 工具 | 说明 |
|------|------|------|
| TeX → SVG | `mathjax-full` (Node.js) | TeX input + SVG output，不走浏览器 |
| SVG → PNG | `cairosvg` (Python) | 高质量位图转换，dpi 可控 |
| 清理 | 自动化脚本 | 只保留最终交付物，删除临时文件 |

## 文件结构

```
.trae/skills/formula-render-export/
└── SKILL.md          # Skill 定义文件（触发规则 + 执行协议）
```

## License

MIT
