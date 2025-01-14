# 编译原理第五次作业

<center>
  2110306206 卓致用
</center>
## Ex 4.3.1

下面是一个只包含符号 $a$ 和 $b$ 的正则表达式的文法。它使用 $+$ 替代表示并运算的字符 $|$，以避免和文法中作为元符号使用的竖线相混淆：

```
rexpr → rexpr + rterm | rterm
rterm → rterm rfactor | rfactor
rfactor → rfactor * | rprimary
rprimary → a | b
```

1) 对这个文法提取左公因子。

2) 提取左公因子的变换能使这个文法适用于自顶向下的语法分析技术吗？

3) 提取左公因子之后，从原文法中消除左递归。

4) 得到的文法适用于自顶向下的语法分析吗？

### 解答

#### 第一问

无左公因子，不改

#### 第二问

此文法具有二义性，不行

#### 第三问

$$
\begin{aligned}
\text{rexpr} &\rightarrow \text{rterm} \, \text{rexpr}' \\
\text{rexpr}' &\rightarrow + \text{rterm} \, \text{rexpr}' \mid \varepsilon \\
\text{rterm} &\rightarrow \text{rfactor} \, \text{rterm}' \\
\text{rterm}' &\rightarrow \text{rfactor} \, \text{rterm}' \mid \varepsilon \\
\text{rfactor} &\rightarrow \text{rprimary} \, \text{rfactor}' \\
\text{rfactor}' &\rightarrow * \, \text{rfactor}' \mid \varepsilon \\
\text{rprimary} &\rightarrow a \mid b
\end{aligned}
$$

#### 第四问

此时，文法不再具有二义性，可以。

## Ex 4.3.3

下面文法的目的是消除 4.3.2 节中讨论的悬空-else 二义性

$$
\begin{aligned}
&\text{stmt} \rightarrow \text{if } \text{expr } \text{then } \text{stmt} \mid \text{matchedStmt} \\
&\text{matchedStmt} \rightarrow \text{if } \text{expr } \text{then } \text{matchedStmt else } \text{stmt} \mid \text{other}
\end{aligned}
$$

说明这个文法仍然是二义性的。

### 解答

给定文法的产生式如下：
$$
\begin{aligned}
&\text{stmt} \rightarrow \text{if } \text{expr } \text{then } \text{stmt} \\
&\text{stmt} \rightarrow \text{matchedStmt} \\
&\text{matchedStmt} \rightarrow \text{if } \text{expr } \text{then } \text{matchedStmt else } \text{stmt} \\
&\text{matchedStmt} \rightarrow \text{other}
\end{aligned}
$$

考虑以下输入：

```
if expr
then
    if expr
    then matchedStmt
    else
        if expr
        then matchedStmt
else stmt
```

```
if expr
then
    if expr
    then matchedStmt
    else
        if expr
        then matchedStmt
        else stmt
```

所以这个文法是二义性的，因为同一个输入可以有两种不同的解析树。

## Ex 4.4.1

为下面的每一个文法设计一个预测分析器，并给出预测分析表。你可能先要对文法进行提取左公因子或消除左递归的操作。

### 第一问

$$
S \rightarrow 0S1 | 01
$$

#### 解答

提取左公因子：
$$
\begin{aligned}
S &\rightarrow 0S' \\
S' &\rightarrow 0S'1 | 1
\end{aligned}
$$

预测分析表：

-   $\text{First}(S) = \{0\}$，$\text{Follow}(S) = \{\$, 1\}$
-   $\text{First}(S') = \{0, 1\}$，$\text{Follow}(S') = \{ 1, \$\}$

<table>
    <thead>
        <tr>
            <th rowspan="2">非终结符号</th>
            <th colspan="3">输入符号</th>
        </tr>
        <tr>
            <th>0</th>
            <th>1</th>
            <th>$</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>S</th>
            <td>S→0S'</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th>S'</th>
            <td>S'→0S'1</td>
            <td>S'→1</td>
            <td></td>
        </tr>
    </tbody>
</table>

### 第三问

$$
S \rightarrow S(S)S | \varepsilon
$$

#### 解答

消除左递归：
$$
\begin{aligned}
S &\rightarrow S' \\
S' &\rightarrow (S)SS' | \varepsilon
\end{aligned}
$$

预测分析表：

-   $\text{First}(S) = \{(\}$，$\text{Follow}(S) = \{\$, )\}$
-   $\text{First}(S') = \{(, \varepsilon\}$，$\text{Follow}(S') = \{\$)\}$

<table>
    <thead>
        <tr>
            <th rowspan="2">非终结符号</th>
            <th colspan="3">输入符号</th>
        </tr>
        <tr>
            <th>(</th>
            <th>)</th>
            <th>$</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>S</th>
            <td>S→S'</td>
            <td>S→S'</td>
            <td>S→S'</td>
        </tr>
        <tr>
            <th>S'</th>
            <td>S'→(S)SS'<br/>S'→ε</td>
            <td>S'→ε</td>
            <td>S'→ε</td>
        </tr>
    </tbody>
</table>

### 第五问

$$
\begin{aligned}
S &\rightarrow (L) | a \\
L &\rightarrow L,S | S
\end{aligned}
$$

#### 解答

消除左递归：
$$
\begin{aligned}
S &\rightarrow (L) | a \\
L &\rightarrow SL' \\
L' &\rightarrow ,SL' | \varepsilon
\end{aligned}
$$

预测分析表：

-   $\text{First}(S) = \{(, a\}$，$\text{Follow}(S) = \{\$, \text{\\,} \}$
-   $\text{First}(L) = \{(, a\}$，$\text{Follow}(L) = \{ )\}$
-   $\text{First}(L') = \{\text{\\,}{~}, \varepsilon\}$，$\text{Follow}(L') = \{ )\}$

预测分析表：

<table>
    <thead>
        <tr>
            <th rowspan="2">非终结符号</th>
            <th colspan="5">输入符号</th>
        </tr>
        <tr>
            <th>(</th>
            <th>)</th>
            <th>a</th>
            <th>,</th>
            <th>$</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>S</th>
            <td>S→(L)</td>
            <td></td>
            <td>S→a</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th>L</th>
            <td>L→SL'</td>
            <td></td>
            <td>L→SL'</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th>L'</th>
            <td></td>
            <td>L'→ε</td>
            <td></td>
            <td>L'→,SL'</td>
            <td></td>
        </tr>
    </tbody>
</table>
