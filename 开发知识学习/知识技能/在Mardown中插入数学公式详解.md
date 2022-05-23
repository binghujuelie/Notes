**本文搬运自**：<https://oysz2016.github.io/post/8611e6fb.html>

[toc]

---

# Mathjax
Mathjax 是一款运行在浏览器中的开源数学符号渲染引擎，通过使用Mathjax可以方便地在浏览器中显示数学公式。

---

# 基本语法
- inline 型公式可以通过` $...$ `定义
  - 例如：` $\sum_{i=0}^n\int_{a}^{b}g(t,i)\text{d}t$ `
  - 显示：`$\sum_{i=0}^n\int_{a}^{b}g(t,i)\text{d}t$`
- 而公式块可以通过 ` $$...$$ ` 来定义
  - 例如：` $$W_G^{mn}=max\{0, W_G.\xi_G(f_G^m, f_G^n)\}$$ `
  - 显示：$$W_G^{mn}=max\{0, W_G.\xi_G(f_G^m, f_G^n)\}$$
- **调试博客时发现：想要将LaTex代码做成inline源码显示最好将代码与inline代码标识符``` ` ```之间留一个空格，有时公式不能正常显示则添加一对紧贴LaTex公式代码的标识符``` ` ```来解决**

---

# 希腊字母


|     显示     |    命令    |    显示    |   命令   |
| :--------: | :------: | :------: | :----: |
|  `$\alpha$`  |  \alpha  | `$\beta$`  | \beta  |
|  `$\gamma$`  |  \gamma  | `$\delta$` | \delta |
| `$\epsilon$` | \epsilon | `$\zeta$`  | \zeta  |
|   `$\eta$`   |   \eta   | `$\theta$` | \theta |
|  `$\iota$`   |  \iota   | `$\kappa$` | \kappa |
| `$\lambda$`  | \lambda  |  `$\mu$`   |  \mu   |
|   `$\nu$`    |   \nu    |  `$\xi$ `  |  \xi   |
|   `$\pi$`    |   \pi    |  `$\rho$`  |  \rho  |
|  `$\sigma$`  |  \sigma  |  `$\tau$`  |  \tau  |
| `$\upsilon$` | \upsilon |  `$\phi$`  |  \phi  |
|   `$\chi$`   |   \chi   |  `$\psi$`  |  \psi  |
|  `$\omega$`  |  \omega  |          |        |
- 如果要书写大写希腊字母，只需要将命令首字母大写即可。
  - 例如：` $\gamma$ $\Gamma$ `
  - 显示： `$\gamma$` `$\Gamma$`
- 若需要斜体希腊字母，将命令前加上var前缀即可。
  - 例如：` $\Gamma$ $\varGamma$ `
  - 显示：`$\Gamma$` `$\varGamma$`

---

# 关系运算符
|     显示      |    命令     |      显示      |     命令     |
| :---------: | :-------: | :----------: | :--------: |
|   `$\mid$`    |   \mid    |   `$\nmid$`    |   \nmid    |
|   `$\cdot$`   |   \cdot   |    `$\leq$`    |    \leq    |
|   `$\geq$`   |   \geq    |    `$\neq$`    |    \neq    |
|  `$\approx$`  |  \approx  |   `$\equiv$`   |   \equiv   |
|   `$\prec$`   |   \prec   |  `$\preceq$`   |  \preceq   |
|  `$\succeq$`  |  \succeq  |    `$\gg$`     |    \gg     |
|   `$\sim$`    |   \sim    |   `$\simeq$`   |   \simeq   |
| `$\asymp$`   |  \asymp   |   `$\cong$`    |   \cong    |
|  `$\doteq$`   |  \doteq   |  `$\propto$`   |  \propto   |
|  `$\models$`  |  \models  | `$\parallel$`  | \parallel  |
|  `$\bowtie$`  |  \bowtie  |   `$\perp$`    |   \perp    |
|   `$\circ$`   |   \circ   |    `$\ast$`    |    \ast    |
| `$\bigodot$`  | \bigodot  | `$\bigotimes$` | \bigotimes |
| `$\bigoplus$` | \bigoplus |              |            |

---

# 算术运算符
|         显示         |        命令        |        显示        |       命令       |
| :----------------: | :--------------: | :--------------: | :------------: |
|       `$\pm$`        |       \pm        |      `$\mp$`       |      \mp       |
|      `$\times$`      |      \times      |      `$\ast$`      |      \ast      |
|      `$\star$`       |      \star       |     `$\circ$`      |     \circ      |
|     `$\bullet$`      |     \bullet      |     `$\cdot$`      |     \cdot      |
|      ` $\div$`       |       \div       |      `$\sum$`      |      \sum      |
|      `$\prod$`       |      \prod       |    `$\coprod$`     |    \coprod     |
|      `$\oplus$`      |      \oplus      |   `$\bigoplus$`    |   \bigoplus    |
|     `$\ominus$`      |     \ominus      |    `$\otimes$`    |    \otimes     |
|    `$\bigotimes$`    |    \bigotimes    |    `$\oslash$`    |    \oslash     |
|     ` $\odot$`       |      \odot       |    `$\bigodot$`    |    \bigodot    |
|     `$\diamond$`     |     \diamond     | `$\bigtriangleup$` | \bigtriangleup |
| `$\bigtriangledown$` | \bigtriangledown | `$\triangleleft$`  | \triangleleft  |
| ` $\triangleright$`  |  \triangleright  |    `$\bigcirc$`    |    \bigcirc    |

---

# 字母修饰
## 上下标
- 上标：`^`，下标：`_`
  - 例如：`C_n^2`
  - 显示：`$C_n^2$`
## 矢量
- `\vec a`，显示为 `$\vec a$`
  - 例如：`\overrightarrow{xy}`
  - 显示：`$\overrightarrow{xy}$`
## 字体
- 打印机字体 Typewriter：`\mathtt{A}`显示为 `$\mathtt{A}$`
- 黑板粗体字 Blackboard Bold：`\mathbb{A}`显示为 `$\mathbb{A}$`
- 无衬线字体 Sans Serif：`\mathsf{A}`显示为 `$\mathsf{A}$`
- 手写体：`\mathscr{A}`显示为 `$\mathscr{A}$`
- 罗马字体：`\mathrm{A}`显示为 `$\mathrm{A}$`

---

# 括号
- 小括号：`(a)` 显示为 `$(a)$`
- 中括号：`[a]`显示为 `$[a]$`
- 尖括号：`\langle a \rangle`显示为 `$\langle a \rangle$`
- 自适应括号：`\left(...\right)`能使括号大小与邻近公式相适应，例如：
  - `(\frac{x}{y})`显示为 `$(\frac{x}{y})$`
  - `\left(\frac{x}{y}\right)`则显示为 `$\left(\frac{x}{y}\right)$`

---

# 求和、极限与积分
- 求和：`\sum`
  - 例如：`\sum_{i=1}^n{a_i}`显示为 `$\sum_{i=1}^n{a_i}$`
- 极限：`\lim`
  - 例如：`\lim_{x\to 0}`显示为 `$\lim_{x\to 0}$`
- 积分：`\int`
  - 例如：`\int_0^xf(x)dx`显示为 `$\int_0^xf(x)dx$`

---

# 分式与根式
- 分式：`\frac`
  - 例如：`\frac{x}{y}`显示为 `$\frac{x}{y}$`
- 根式：`\sqrt`
  - 例如：`\sqrt[x]{y}`显示为 `$\sqrt[x]{y}$`

---

# 特殊函数
- `\函数名`
  - 例如：`\sin x`，`\ln x`，`\max(A, B, C)`显示为 `$\sin x$`，`$\ln x$`，`$\max(A, B, C)$`

---

# 空格
- LaTex语法会忽略空格，需要时可以加转义字符`\`
  - 小空格：`a\ b`显示为 `$a\ b$`
  - 制表符：`a\quad b`显示为 `$a\quad b$`

---

# 矩阵
## 基本语法
- 起始标记：`\bigin{matrix}`，结束标记：`\end{matrix}`
- 每一行末尾用`\\`标记，行间元素用`&`分割
  - 例如：
  ```
  $$\begin{matrix}
  1&0&0\\
  0&1&0\\
  0&0&1\\
  \end{matrix}$$
  ```
  - 显示为：
```math
    $$\begin{matrix}
    1&0&0\\
    0&1&0\\
    0&0&1\\
    \end{matrix}$$
```

## 矩阵边框
- 在起始、结束标记处用下列关键字替换matrix
  - pmatrix：小括号边框
  - bmatrix：中括号边框
  - Bmatrix：大括号边框
  - vmatrix：单竖线边框
  - Vmatrix：双竖线边框
## 省略元素
- 横向省略号：`\cdots`，纵向省略号：`\vdots`，斜向省略号：`\ddots`
  - 例如：
  ```
  $$\begin{bmatrix}
  {a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
  {a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
  {\vdots}&{\vdots}&{\ddots}&{\vdots}\\
  {a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
  \end{bmatrix}$$
  ```
  - 显示为：
    $$\begin{bmatrix}
    {a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
    {a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
    {\vdots}&{\vdots}&{\ddots}&{\vdots}\\
    {a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
    \end{bmatrix}$$

---

# 方程组
- 需要cases环境：起始、结束处以{cases}声明
  - 例如：
  ```
  $$\begin{cases}
  a_1x+b_1y+c_1z=d_1\\
  a_2x+b_2y+c_2z=d_2\\
  a_3x+b_3y+c_3z=d_3\\
  \end{cases}$$
  ```
  - 显示为：
  ```math
    $$\begin{cases}
    a_1x+b_1y+c_1z=d_1\\
    a_2x+b_2y+c_2z=d_2\\
    a_3x+b_3y+c_3z=d_3\\
    \end{cases}$$
  ```
---

# 公式编号
- 使用`\tag{n}`标签。
  **注意：标签经测试不适用于inline型公式**
  - 例如：`f(x)=x\tag{1}`显示为
    $$ f(x)=x \tag{1} $$

---

# 总结 
以上是我整理的 Mathjax 语法，通过嵌套组合应该能应对大部分需要进行公式录入的情况。现在开始书写你自己的公式吧！