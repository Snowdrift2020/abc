# LaTeX 使用教程

LaTeX 是一种专业排版系统，常用于论文、实验报告、数学公式、简历、书籍和算法文档。它不是像 Word 那样直接拖拽排版，而是通过写源码，再编译生成 PDF。

## 1 LaTeX 是什么

LaTeX 的核心思想是：你负责写内容和结构，LaTeX 负责排版。

例如你写：

```latex
\section{引言}
这是第一段文字。
```

LaTeX 会自动把它排成一级标题和正文。

它特别适合：

- 数学公式很多的文档
- 论文、课程报告、毕业设计
- 需要自动目录、编号、交叉引用的文档
- 需要统一格式的长文档

## 2 安装方式

### 2.1 在线使用 Overleaf

最简单的方式是使用 Overleaf：

```text
https://www.overleaf.com/
```

优点：

- 不需要本地安装
- 打开浏览器就能写
- 自动编译 PDF
- 适合多人协作

缺点：

- 需要网络
- 免费版编译时间有限

### 2.2 本地安装 TeX Live

Windows、macOS、Linux 都可以安装 TeX Live。

安装完成后，常用命令有：

```bash
pdflatex main.tex
xelatex main.tex
bibtex main
```

如果写中文，推荐使用 `xelatex` 编译。

### 2.3 编辑器推荐

常见编辑器：

- VS Code + LaTeX Workshop 插件
- TeXstudio
- Overleaf

如果你用 VS Code，建议安装插件：

```text
LaTeX Workshop
```

## 3 第一个 LaTeX 文档

新建文件 `main.tex`：

```latex
\documentclass{article}

\begin{document}

Hello, LaTeX!

\end{document}
```

编译：

```bash
pdflatex main.tex
```

会生成：

```text
main.pdf
```

## 4 中文文档

写中文推荐使用 `ctex` 文档类：

```latex
\documentclass[UTF8]{ctexart}

\begin{document}

\section{引言}
这是一个中文 LaTeX 文档。

\end{document}
```

编译时使用：

```bash
xelatex main.tex
```

不要用 `pdflatex` 编译中文文档，否则容易乱码。

## 5 文档基本结构

一个完整 LaTeX 文档通常长这样：

```latex
\documentclass[UTF8]{ctexart}

\title{LaTeX 使用教程}
\author{你的名字}
\date{\today}

\begin{document}

\maketitle

\tableofcontents

\section{第一章}
正文内容。

\subsection{小节}
更多内容。

\end{document}
```

说明：

| 命令 | 作用 |
|---|---|
| `\documentclass{}` | 设置文档类型 |
| `\title{}` | 设置标题 |
| `\author{}` | 设置作者 |
| `\date{}` | 设置日期 |
| `\begin{document}` | 正文开始 |
| `\end{document}` | 正文结束 |
| `\maketitle` | 生成标题 |
| `\tableofcontents` | 生成目录 |

## 6 常用文档类型

| 类型 | 用途 |
|---|---|
| `article` | 英文短文、论文、报告 |
| `report` | 长报告 |
| `book` | 书籍 |
| `ctexart` | 中文短文、论文、报告 |
| `ctexrep` | 中文长报告 |
| `ctexbook` | 中文书籍 |
| `beamer` | 幻灯片 |

中文推荐：

```latex
\documentclass[UTF8]{ctexart}
```

## 7 标题层级

LaTeX 常用标题命令：

```latex
\section{一级标题}
\subsection{二级标题}
\subsubsection{三级标题}
\paragraph{段落标题}
```

如果不想编号，可以加星号：

```latex
\section*{无编号标题}
```

## 8 段落与换行

LaTeX 中，一个空行表示新段落。

```latex
这是第一段。

这是第二段。
```

强制换行：

```latex
第一行\\
第二行
```

一般正文里不建议频繁使用 `\\`，让 LaTeX 自动排版更好。

## 9 文字格式

```latex
\textbf{加粗}
\textit{斜体}
\underline{下划线}
\texttt{等宽字体}
```

效果：

- `\textbf{}`：加粗
- `\textit{}`：斜体
- `\underline{}`：下划线
- `\texttt{}`：代码风格字体

## 10 列表

### 10.1 无序列表

```latex
\begin{itemize}
  \item 第一项
  \item 第二项
  \item 第三项
\end{itemize}
```


### 10.2 有序列表

```latex
\begin{enumerate}
  \item 第一步
  \item 第二步
  \item 第三步
\end{enumerate}
```

### 10.3 描述列表

```latex
\begin{description}
  \item[LaTeX] 专业排版系统
  \item[TeX] LaTeX 的底层排版引擎
\end{description}
```

## 11 数学公式

LaTeX 最强大的地方之一就是数学公式。

### 11.1 行内公式

```latex
这是行内公式：$a^2 + b^2 = c^2$。
```

### 11.2 单独公式

```latex
\[
a^2 + b^2 = c^2
\]
```

### 11.3 带编号公式

```latex
\begin{equation}
E = mc^2
\end{equation}
```

### 11.4 常用数学写法

| 效果 | LaTeX |
|---|---|
| 上标 | `x^2` |
| 下标 | `a_i` |
| 分数 | `\frac{a}{b}` |
| 根号 | `\sqrt{x}` |
| n 次根 | `\sqrt[n]{x}` |
| 求和 | `\sum_{i=1}^{n} i` |
| 积分 | `\int_a^b f(x)\,dx` |
| 极限 | `\lim_{x\to 0}` |
| 无穷 | `\infty` |
| 小于等于 | `\leq` |
| 大于等于 | `\geq` |
| 不等于 | `\neq` |
| 约等于 | `\approx` |

示例：

```latex
\[
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
\]
```

## 12 多行公式

使用 `amsmath` 宏包：

```latex
\usepackage{amsmath}
```

对齐公式：

```latex
\begin{align}
a + b &= c \\
x + y &= z
\end{align}
```

不编号：

```latex
\begin{align*}
a + b &= c \\
x + y &= z
\end{align*}
```

## 13 矩阵

需要 `amsmath`：

```latex
\usepackage{amsmath}
```

矩阵：

```latex
\[
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
\]
```

常见矩阵环境：

| 环境 | 括号样式 |
|---|---|
| `matrix` | 无括号 |
| `pmatrix` | 小括号 |
| `bmatrix` | 中括号 |
| `Bmatrix` | 大括号 |
| `vmatrix` | 单竖线 |
| `Vmatrix` | 双竖线 |

## 14 插入图片

使用图片需要宏包：

```latex
\usepackage{graphicx}
```

插入图片：

```latex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.6\textwidth]{image.png}
  \caption{图片标题}
  \label{fig:example}
\end{figure}
```

说明：

| 参数 | 含义 |
|---|---|
| `width=0.6\textwidth` | 图片宽度为正文宽度的 60% |
| `\caption{}` | 图片标题 |
| `\label{}` | 图片标签，用于引用 |
| `[htbp]` | 建议 LaTeX 放置图片的位置 |

常用位置参数：

| 参数 | 含义 |
|---|---|
| `h` | here，当前位置 |
| `t` | top，页面顶部 |
| `b` | bottom，页面底部 |
| `p` | 独立浮动页 |

## 15 表格

基础表格：

```latex
\begin{tabular}{|c|c|c|}
\hline
姓名 & 年龄 & 成绩 \\
\hline
张三 & 18 & 95 \\
李四 & 19 & 90 \\
\hline
\end{tabular}
```

列格式说明：

| 符号 | 含义 |
|---|---|
| `l` | 左对齐 |
| `c` | 居中 |
| `r` | 右对齐 |
| `|` | 竖线 |

带标题的表格：

```latex
\begin{table}[htbp]
  \centering
  \caption{学生成绩表}
  \begin{tabular}{|c|c|c|}
    \hline
    姓名 & 年龄 & 成绩 \\
    \hline
    张三 & 18 & 95 \\
    李四 & 19 & 90 \\
    \hline
  \end{tabular}
  \label{tab:score}
\end{table}
```

## 16 交叉引用

LaTeX 可以自动引用章节、公式、图片、表格。

设置标签：

```latex
\section{实验结果}
\label{sec:result}
```

引用：

```latex
见第 \ref{sec:result} 节。
```

图片引用：

```latex
如图 \ref{fig:example} 所示。
```

表格引用：

```latex
如表 \ref{tab:score} 所示。
```

公式引用：

```latex
由公式 \eqref{eq:energy} 可知。
```

公式标签：

```latex
\begin{equation}
E = mc^2
\label{eq:energy}
\end{equation}
```

## 17 参考文献

### 17.1 简单写法

```latex
\begin{thebibliography}{99}

\bibitem{knuth}
Donald E. Knuth.
\textit{The TeXbook}.
Addison-Wesley, 1984.

\end{thebibliography}
```

引用：

```latex
这是一个引用 \cite{knuth}。
```

### 17.2 BibTeX 写法

新建 `refs.bib`：

```bibtex
@book{knuth1984texbook,
  author    = {Donald E. Knuth},
  title     = {The TeXbook},
  year      = {1984},
  publisher = {Addison-Wesley}
}
```

在 `.tex` 中写：

```latex
\bibliographystyle{plain}
\bibliography{refs}
```

引用：

```latex
\cite{knuth1984texbook}
```

编译顺序通常是：

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

## 18 代码块

插入代码可以用 `listings` 宏包：

```latex
\usepackage{listings}
\usepackage{xcolor}
```

示例：

```latex
\begin{lstlisting}[language=C++]
#include <iostream>
using namespace std;

int main() {
    cout << "Hello" << endl;
    return 0;
}
\end{lstlisting}
```

可以配置样式：

```latex
\lstset{
  basicstyle=\ttfamily,
  keywordstyle=\color{blue},
  commentstyle=\color{gray},
  stringstyle=\color{red},
  numbers=left,
  numberstyle=\tiny,
  frame=single,
  breaklines=true
}
```

## 19 页面设置

使用 `geometry` 宏包：

```latex
\usepackage[a4paper, margin=2.5cm]{geometry}
```

常见设置：

```latex
\usepackage[
  a4paper,
  left=2.5cm,
  right=2.5cm,
  top=2.5cm,
  bottom=2.5cm
]{geometry}
```

## 20 页眉页脚

使用 `fancyhdr`：

```latex
\usepackage{fancyhdr}
\pagestyle{fancy}

\fancyhead[L]{课程报告}
\fancyhead[C]{}
\fancyhead[R]{\thepage}
\fancyfoot{}
```

## 21 超链接

使用 `hyperref`：

```latex
\usepackage[colorlinks=true, linkcolor=blue, urlcolor=blue]{hyperref}
```

插入链接：

```latex
\href{https://www.latex-project.org/}{LaTeX 官网}
```

显示网址：

```latex
\url{https://www.latex-project.org/}
```

## 22 常用宏包

| 宏包 | 作用 |
|---|---|
| `ctex` | 中文支持 |
| `amsmath` | 数学公式增强 |
| `amssymb` | 数学符号 |
| `graphicx` | 插入图片 |
| `geometry` | 页面边距 |
| `hyperref` | 超链接 |
| `booktabs` | 三线表 |
| `listings` | 插入代码 |
| `xcolor` | 颜色 |
| `fancyhdr` | 页眉页脚 |

## 23 常见完整模板

```latex
\documentclass[UTF8]{ctexart}

\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{geometry}
\usepackage{hyperref}
\usepackage{xcolor}
\usepackage{listings}

\geometry{a4paper, margin=2.5cm}

\title{课程报告}
\author{你的名字}
\date{\today}

\begin{document}

\maketitle
\tableofcontents

\section{引言}
这里写引言。

\section{数学公式}
\[
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
\]

\section{图片}
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.6\textwidth]{image.png}
  \caption{示例图片}
  \label{fig:example}
\end{figure}

\section{表格}
\begin{table}[htbp]
  \centering
  \caption{示例表格}
  \begin{tabular}{ccc}
    \hline
    A & B & C \\
    \hline
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    \hline
  \end{tabular}
  \label{tab:example}
\end{table}

\section{结论}
这里写结论。

\end{document}
```

## 24 编译方式

### 24.1 英文文档

```bash
pdflatex main.tex
```

### 24.2 中文文档

```bash
xelatex main.tex
```

### 24.3 有参考文献

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

### 24.4 使用 latexmk

`latexmk` 可以自动判断需要编译几次：

```bash
latexmk -xelatex main.tex
```

清理辅助文件：

```bash
latexmk -c
```

## 25 常见报错

### 25.1 中文乱码

原因：使用了 `pdflatex` 编译中文。

解决：

```bash
xelatex main.tex
```

并使用：

```latex
\documentclass[UTF8]{ctexart}
```

### 25.2 Undefined control sequence

意思是命令未定义。

常见原因：

- 命令拼错
- 忘记引入宏包

例如用了 `\includegraphics`，但没写：

```latex
\usepackage{graphicx}
```

### 25.3 Missing $ inserted

意思是数学符号写在了普通文本模式里。

错误：

```latex
x_i = 1
```

正确：

```latex
$x_i = 1$
```

或：

```latex
\[
x_i = 1
\]
```

### 25.4 File not found

可能是图片或 `.bib` 文件路径不对。

检查：

- 文件名是否正确
- 后缀是否正确
- 图片是否和 `.tex` 文件在同一目录
- 路径中是否有空格或中文

### 25.5 引用显示问号

例如：

```text
Figure ??
```

解决：多编译几次。

```bash
xelatex main.tex
xelatex main.tex
```

有参考文献时：

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

## 26 学习建议

刚开始学习 LaTeX，不要一上来记所有命令。推荐顺序：

1. 学会基本文档结构
2. 学会中文编译
3. 学会章节、列表、公式
4. 学会图片和表格
5. 学会交叉引用
6. 学会参考文献
7. 最后再学模板和复杂排版

日常写报告时，最常用的是：

```latex
\section{}
\subsection{}
\textbf{}
\[
\]
\begin{figure}
\begin{table}
\ref{}
\cite{}
```

## 27 最小中文论文模板

```latex
\documentclass[UTF8]{ctexart}

\usepackage{amsmath}
\usepackage{graphicx}
\usepackage{geometry}
\usepackage{hyperref}

\geometry{a4paper, margin=2.5cm}

\title{论文标题}
\author{姓名}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
这里写摘要。
\end{abstract}

\tableofcontents

\section{引言}
这里写引言。

\section{方法}
这里写方法。

\section{实验}
这里写实验。

\section{结论}
这里写结论。

\end{document}
```

编译：

```bash
xelatex main.tex
```

