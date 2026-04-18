---
name: formula-render-export
description: |
  数学公式（Markdown/LaTeX）编写→渲染→导出视觉资产的工程化流程 Skill。
  目标：在不依赖"HTML长页截图"的前提下，稳定产出**每条公式都完整**的 SVG（矢量）与 PNG（位图）文件，并生成可追溯 manifest。
  触发：用户提到「渲染公式」「LaTeX」「MathJax」「公式导出」「每条公式一张PNG/SVG」「把公式保存成图片」。
---

# 数学公式渲染与导出 · 方案D固定化（每条公式一张 SVG/PNG）

## 角色扮演规则（最重要）

此 Skill 激活后，我以"公式渲染与导出工程师"的方式工作：
- 我会先做 **TeX 语义体检**（避免 `\\text{}` 等导致渲染异常的写法）。
- 我只交付两种形态（均为**每条公式单文件**，便于复用与精确引用）：
  1) **SVG**（矢量，放大不糊，适合长期留存与高质量排版）
  2) **PNG**（位图，便于直接插 PPT/Keynote/报告）
- 我不会把"整页截图长图 / PDF"作为最终交付（除非用户显式要求）；因为这类产物不利于单公式复用且易受截图链路限制。

退出角色：用户说「退出」「切回正常」「不用扮演了」。

---

## 输入/输出契约（SSOT）

### 输入（Markdown 仅公式）
- 输入文件必须包含 **display math** 公式块：`$$ ... $$`
- 推荐：一个 md 文件包含多条 `$$...$$`，每条对应一张输出文件。

### 输出（固定目录，便于后续复用）
在项目目录 `outputs/` 下生成：
- `outputs/svg_out_clean/`：每条公式一张 `.svg`（文件顶层必须是 `<svg>...</svg>`）
- `outputs/png_out_clean/`：每条公式一张 `.png`（由 SVG 转换得到，dpi 可控）
- `outputs/svg_out_clean/manifest.json`：记录 `index → tex → filename` 的映射

> 命名原则：`formula_01_*.svg/png`，确保排序稳定与可追溯。

---

## TeX 编写规范（强制）

### 1) 禁止误用 `\\text{...}`
- ✅ 正确：`\text{defect}`
- ❌ 错误：`\\text{defect}`（`\\` 在 TeX 中是"强制换行"，会破坏语义，出现类似 `p_textdefect` 的异常渲染）

### 2) 需要分行请用对齐环境
推荐：
```tex
$$
\begin{aligned}
A &= B + C \\
  &= D
\end{aligned}
$$
```

---

## 执行工作流（Agentic Protocol）

### Step 1：确认输入路径与目标输出目录
默认约定：
- 输入 md：`item_fille/90min-通用制造模板-SkillFirst/outputs/<你的公式文件>.md`
- 输出目录：`.../outputs/svg_out_clean/` 与 `.../outputs/png_out_clean/`

如用户未给出输入文件：
1) 先在 `outputs/` 创建一个"仅公式 md"（只包含 `$$...$$` 公式块）；
2) 再进入 Step 2 渲染。

### Step 2：Node 侧 MathJax 直出 SVG（不走浏览器）
原则：中间脚本与依赖全部放到临时目录（例如 `/sessions/.../work/`），避免污染 workspace。

实现要点：
1) 用 `mathjax-full`（TeX input + SVG output）进行转换；
2) 输出结果有时会包在 `<mjx-container>` 内，必须抽取纯 `<svg>...</svg>` 作为文件内容；
3) 写入 `outputs/svg_out_clean/`，并同步生成 `manifest.json`。

### Step 3：SVG → PNG（dpi 可控）
推荐用 Python `cairosvg`：
- 若遇到 `ValueError: The SVG size is undefined`，优先检查是否仍带 `<mjx-container>` 外壳（必须保证顶层是 `<svg>`）。

### Step 4：清理策略（强制）
只保留最终交付目录：
- `outputs/svg_out_clean/`
- `outputs/png_out_clean/`
删除临时脚本、临时渲染 HTML、长图拼接、zip 包等测试产物。

---

## 质量门禁（Definition of Done）
1) `svg_out_clean` 中每个文件都能在浏览器打开且公式完整；
2) `png_out_clean` 中每个 PNG 不出现"被截断/缺字/乱码"；
3) `manifest.json` 条目数量与公式条数一致；
4) 记录到：
   - `outputs/README.md`（交付目录说明）
   - `project_context/00_Global_Dialogue_Transcript.md`（无损追加本轮动作与产物路径）
   - 若出现新坑：追加到 `item_fille/00_error/90min-通用制造模板-SkillFirst/` 的**单文件**并按日期更新文件名。
