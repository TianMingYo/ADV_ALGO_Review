# LaTeX 项目结构

- `main.tex`：主文件，包含导言区并通过 `\input` 引入各部分。
- `frontmatter.tex`：标题与摘要。
- `sections/01_problem.tex` 至 `sections/08_conclusion.tex`：正文各 section。
- `references.tex`：参考文献。
- `main.pdf`：由 `main.tex` 使用 XeLaTeX 编译得到的渲染版。

## 编译方式

在项目根目录执行：

```bash
xelatex main.tex
xelatex main.tex
```

也可使用：

```bash
latexmk -xelatex main.tex
```
