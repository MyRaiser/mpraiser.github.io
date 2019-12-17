# 使用Markdown写笔记/实验报告的工具链

[TOC]

## 工具
在讨论使用什么工具之前，首先要知道的是我们到底需要些什么工具。

我们需要的工具有：

- ==编辑器==

    用于编辑纯文本的工具，是我们编辑内容的方式。

- ==渲染器==

    将`.md`源文件转化为带样式的，我们阅读内容的方式。

- 转换器

    将`.md`转化为其他格式的文件，包括但不限于`.html`、`.pdf`、`.docx`

- LaTeX引擎

    原生`markdown`只对转化为`html`兼容性好。

编辑样式无关，是纯文本，所以其实拿notepad也能写markdown，但是因为不能渲染，一般没人那么做。

通常的MWeb、Typora都是编辑器和渲染器的整合。

## 需求
- Markdown的轻量、可读性
- LaTeX公式
- 标题、正文、图片样式
- 导出PDF

Markdown的优势：
- 轻量
- 可读性高
- 可以即时显示，对写LaTeX公式时特别有用

LaTeX：
- 自定义样式
- 有完善的生成pdf方案

## 方案
### VSCode + MPE + Chrome


使用KaTeX显示公式。

优点：

- 配置简单，使用简单。

缺点：

- Chrome中`html`转`pdf`丢失格式。
- 不方便自定义样式。

### Sphinx-doc + ReST
对于初学者来说，和LaTeX一样不简单。

### VSCode + Pandoc
Yet best choice

**Workflow**:

1. write markdown
2. write latex template (needed by pandoc)
3. convert markdown -> latex
   `pandoc --template=[template_name].tex --pdf-engine=xelatex [input_file_name].md -o [output_file_name].tex`
4. post-processing: modify image style, e.g., height, centering, ...
5. write back the height in markdown (grammar supported by Pandoc's Markdown)
6. convert latex -> PDF
   `pandoc --template=[template_name].tex --pdf-engine=xelatex [input_file_name].tex -o [output_file_name].pdf`
   or 
   use `TexWorks` to finish this step


[pandoc 中编写图片居中过滤器](https://blog.csdn.net/ding_yingzi/article/details/80140575)


吐槽：

LaTeX相比于markdown实在是太难用了。

写LaTeX模板的方法其实就像手写makefile一样令人头大（而且技术上不必要）。希望有人能开发出更好用的工具吧。

### Markdown + CSS