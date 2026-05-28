# Markdown 中使用 LaTeX 教程

Markdown 文档里可以使用 LaTeX 写数学公式。常见用途包括：算法笔记、数学推导、课程报告、论文草稿、题解文档。

注意：Markdown 本身不一定支持 LaTeX，是否能正常显示取决于你的预览工具。VS Code 推荐使用 `Markdown Preview Enhanced` 插件。

## 1 行内公式

行内公式写在一对 `$` 中间。

写法：

```md
这是一个行内公式：$a^2 + b^2 = c^2$。
```

显示效果：

这是一个行内公式：$a^2 + b^2 = c^2$。

适合放在普通句子中，例如：

```md
当 $n \leq 10^5$ 时，可以使用 $O(n \log n)$ 的算法。
```

## 2 独立公式

独立公式写在一对 `$$` 中间。

写法：

```md
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$
```

显示效果：

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

独立公式适合比较长的推导。

## 3 上标和下标

### 3.1 上标

```md
$x^2$
```

效果：

$x^2$

多个字符作为上标时，要用 `{}`：

```md
$x^{n+1}$
```

效果：

$x^{n+1}$

### 3.2 下标

```md
$a_i$
```

效果：

$a_i$

多个字符作为下标：

```md
$a_{i+1}$
```

效果：

$a_{i+1}$

## 4 分数

写法：

```md
$\frac{a}{b}$
```

效果：

$\frac{a}{b}$

独立公式：

```md
$$
\frac{x^2 + 1}{x + 1}
$$
```

效果：

$$
\frac{x^2 + 1}{x + 1}
$$

## 5 根号

平方根：

```md
$\sqrt{x}$
```

效果：

$\sqrt{x}$

n 次根：

```md
$\sqrt[n]{x}$
```

效果：

$\sqrt[n]{x}$

## 6 求和、乘积、极限

求和：

```md
$$
\sum_{i=1}^{n} i
$$
```

效果：

$$
\sum_{i=1}^{n} i
$$

连乘：

```md
$$
\prod_{i=1}^{n} i
$$
```

效果：

$$
\prod_{i=1}^{n} i
$$

极限：

```md
$$
\lim_{x \to 0} \frac{\sin x}{x} = 1
$$
```

效果：

$$
\lim_{x \to 0} \frac{\sin x}{x} = 1
$$

## 7 积分

不定积分：

```md
$$
\int f(x)\,dx
$$
```

效果：

$$
\int f(x)\,dx
$$

定积分：

```md
$$
\int_{a}^{b} f(x)\,dx
$$
```

效果：

$$
\int_{a}^{b} f(x)\,dx
$$

其中 `\,` 表示一个小空格，常用于 `dx` 前。

## 8 常用比较符号

| 含义 | 写法 | 效果 |
|---|---|---|
| 小于等于 | `\leq` | $\leq$ |
| 大于等于 | `\geq` | $\geq$ |
| 不等于 | `\neq` | $\neq$ |
| 约等于 | `\approx` | $\approx$ |
| 恒等于 | `\equiv` | $\equiv$ |
| 正比于 | `\propto` | $\propto$ |

示例：

```md
$a \leq b,\quad x \neq y,\quad m \approx n$
```

效果：

$a \leq b,\quad x \neq y,\quad m \approx n$

## 9 希腊字母

| 字母 | 写法 | 效果 |
|---|---|---|
| alpha | `\alpha` | $\alpha$ |
| beta | `\beta` | $\beta$ |
| gamma | `\gamma` | $\gamma$ |
| delta | `\delta` | $\delta$ |
| epsilon | `\epsilon` | $\epsilon$ |
| theta | `\theta` | $\theta$ |
| lambda | `\lambda` | $\lambda$ |
| mu | `\mu` | $\mu$ |
| pi | `\pi` | $\pi$ |
| sigma | `\sigma` | $\sigma$ |
| omega | `\omega` | $\omega$ |

大写希腊字母：

```md
$\Delta,\ \Theta,\ \Lambda,\ \Pi,\ \Sigma,\ \Omega$
```

效果：

$\Delta,\ \Theta,\ \Lambda,\ \Pi,\ \Sigma,\ \Omega$

## 10 括号

普通括号：

```md
$(a + b)$
```

自动变大的括号：

```md
$$
\left( \frac{a}{b} \right)
$$
```

效果：

$$
\left( \frac{a}{b} \right)
$$

中括号：

```md
$$
\left[ \frac{a}{b} \right]
$$
```

大括号：

```md
$$
\left\{ \frac{a}{b} \right\}
$$
```

## 11 多行公式

多行公式常用 `aligned`。

写法：

```md
$$
\begin{aligned}
a + b &= c \\
x + y &= z
\end{aligned}
$$
```

效果：

$$
\begin{aligned}
a + b &= c \\
x + y &= z
\end{aligned}
$$

说明：

- `&` 表示对齐位置
- `\\` 表示换行

## 12 分段函数

写法：

```md
$$
f(x)=
\begin{cases}
x^2, & x \geq 0 \\
-x, & x < 0
\end{cases}
$$
```

效果：

$$
f(x)=
\begin{cases}
x^2, & x \geq 0 \\
-x, & x < 0
\end{cases}
$$

## 13 矩阵

写法：

```md
$$
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$
```

效果：

$$
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$

常见矩阵：

```md
$$
\begin{pmatrix}
1 & 2 \\
3 & 4
\end{pmatrix}
$$
```

说明：

| 环境 | 效果 |
|---|---|
| `matrix` | 无括号 |
| `pmatrix` | 小括号 |
| `bmatrix` | 中括号 |
| `Bmatrix` | 大括号 |
| `vmatrix` | 单竖线 |
| `Vmatrix` | 双竖线 |

## 14 向量

箭头向量：

```md
$\vec{a}$
```

效果：

$\vec{a}$

粗体向量：

```md
$\mathbf{a}$
```

效果：

$\mathbf{a}$

## 15 集合符号

| 含义 | 写法 | 效果 |
|---|---|---|
| 属于 | `\in` | $\in$ |
| 不属于 | `\notin` | $\notin$ |
| 子集 | `\subset` | $\subset$ |
| 子集或相等 | `\subseteq` | $\subseteq$ |
| 并集 | `\cup` | $\cup$ |
| 交集 | `\cap` | $\cap$ |
| 空集 | `\emptyset` | $\emptyset$ |

示例：

```md
$x \in A,\quad A \subseteq B,\quad A \cup B$
```

效果：

$x \in A,\quad A \subseteq B,\quad A \cup B$

## 16 逻辑符号

| 含义 | 写法 | 效果 |
|---|---|---|
| 且 | `\land` | $\land$ |
| 或 | `\lor` | $\lor$ |
| 非 | `\neg` | $\neg$ |
| 推出 | `\Rightarrow` | $\Rightarrow$ |
| 等价 | `\Leftrightarrow` | $\Leftrightarrow$ |
| 任意 | `\forall` | $\forall$ |
| 存在 | `\exists` | $\exists$ |

示例：

```md
$a > 0 \Rightarrow a^2 > 0$
```

效果：

$a > 0 \Rightarrow a^2 > 0$

## 17 空格

LaTeX 公式里多个普通空格通常不会显示出来，需要使用专门命令。

| 写法 | 含义 |
|---|---|
| `\,` | 小空格 |
| `\ ` | 普通空格 |
| `\quad` | 较大空格 |
| `\qquad` | 更大空格 |

示例：

```md
$a \quad b \qquad c$
```

效果：

$a \quad b \qquad c$

## 18 算法复杂度示例

```md
快速排序的平均时间复杂度为 $O(n \log n)$，
最坏时间复杂度为 $O(n^2)$。
```

效果：

快速排序的平均时间复杂度为 $O(n \log n)$，
最坏时间复杂度为 $O(n^2)$。

## 19 组合数学示例

```md
$$
C_n^k = \frac{n!}{k!(n-k)!}
$$
```

效果：

$$
C_n^k = \frac{n!}{k!(n-k)!}
$$

## 20 数列求和示例

```md
$$
1 + 2 + \cdots + n = \frac{n(n+1)}{2}
$$
```

效果：

$$
1 + 2 + \cdots + n = \frac{n(n+1)}{2}
$$

## 21 概率公式示例

```md
$$
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
$$
```

效果：

$$
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
$$

## 22 常见问题

### 22.1 为什么公式没有渲染

可能原因：

- Markdown 预览器不支持 LaTeX
- 没有安装支持公式的插件
- `$` 没有成对出现
- 公式语法写错

VS Code 推荐安装：

```text
Markdown Preview Enhanced
```

### 22.2 行内公式里有美元符号怎么办

如果你要显示美元符号 `$`，可以转义：

```md
\$
```

### 22.3 独立公式不要和正文挤在一起

推荐这样写：

```md
下面是公式：

$$
a^2 + b^2 = c^2
$$

公式结束后继续写正文。
```

不要这样写：

```md
下面是公式：$$a^2+b^2=c^2$$继续写正文。
```

## 23 最常用模板

你平时写笔记，记住这些就够用了：

```md
行内公式：$x_i^2 + y_i^2$

独立公式：

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

多行公式：

$$
\begin{aligned}
a + b &= c \\
x + y &= z
\end{aligned}
$$

矩阵：

$$
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$
```

