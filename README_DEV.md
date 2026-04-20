# Formula Render & Export · 开发文档

## Skill 说明

本 Skill 实现数学公式从 LaTeX 到 SVG/PNG 的工程化渲染管线，确保每条公式输出为独立文件，便于复用与精确引用。

> 完整 Skill 定义请参阅 [SKILL.md](SKILL.md)。

## 输入/输出契约

### 输入
- Markdown 文件，包含 `$$ ... $$` display math 公式块
- 推荐一个 md 文件包含多条公式

### 输出
- `svg_out_clean/` — 每条公式一张 `.svg`（矢量，放大不糊）
- `png_out_clean/` — 每条公式一张 `.png`（位图，便于插入PPT）
- `svg_out_clean/manifest.json` — `index → tex → filename` 映射

## TeX 编写规范

### 禁止误用 `\\text{...}`
- ✅ 正确：`\text{defect}`
- ❌ 错误：`\\text{defect}`（`\\` 是强制换行，会破坏语义）

### 需要分行请用对齐环境
```tex
$$
\begin{aligned}
a &= b + c \\
  &= d + e
\end{aligned}
$$
```

### 禁止 `$$` 嵌套
- 不在 `$$...$$` 内部再使用 `$$`

## 技术实现

1. **MathJax 直出 SVG**：Node.js + mathjax-full（TeX input → SVG output）
2. **SVG 转 PNG**：Python cairosvg（DPI 可控）
3. **质量门禁**：SVG 可在浏览器打开、PNG 无截断、manifest 条目数一致

## 后续开发方向

- [ ] 支持行内公式（`$...$`）
- [ ] 增加 PDF 输出格式
- [ ] 支持自定义 DPI 和尺寸
- [ ] 批量处理多个 Markdown 文件
