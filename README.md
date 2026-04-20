# Formula Render & Export

LaTeX → SVG → PNG 工程化渲染管线 —— 每条公式单文件，可追溯、可复用。

![封面](作品封面图.png)

## 作品渲染

| Markdown输入 | TeX语义体检 | SVG矢量输出 |
|:---:|:---:|:---:|
| ![01](作品渲染图/01_Markdown输入.P.A.png) | ![02](作品渲染图/02_TeX语义体检.P.A.png) | ![03](作品渲染图/03_SVG输出.P.A.png) |

| PNG位图输出 | Manifest索引 |
|:---:|:---:|
| ![04](作品渲染图/04_PNG输出.P.A.png) | ![05](作品渲染图/05_Manifest结构.P.A.png) |

## 工作流

1. **Markdown输入** — `$$...$$` 公式块解析
2. **TeX语义体检** — 自动检测修复LaTeX错误
3. **双格式输出** — SVG矢量 + PNG位图
4. **Manifest索引** — 可追溯公式编号映射

## 技术栈

`MathJax` `Node.js` `cairosvg` `Python` `LaTeX` `SVG` `PNG`
