# 编译原理第七次作业

<center>
  2110306206 卓致用
</center>
## Ex 4.7.1

为下述文法构造规范 LR 项集族和 LALR 项集族。
$$
S \to SS+ \mid SS* \mid a
$$

### 解答

准备：添加新起始符号 $S'$ ，提取左公因子和消除左递归：
$$
\begin{aligned}
S' &\to S \\
S &\to aB \\
B &\to aBAB \\
B &\to \varepsilon \\
A &\to + \\
A &\to * \\
\end{aligned}
$$

计算 First 集合：

$$
\begin{aligned}
\text{First}(S) &= \{a\} \\
\text{First}(B) &= \{a, \varepsilon\} \\
\text{First}(A) &= \{+, *\} \\
\end{aligned}
$$

计算 Follow 集合：

$$
\begin{aligned}
\text{Follow}(S') &= \{\$ \} \\
\text{Follow}(S) &= \{\$\} \\
\text{Follow}(B) &= \{+, *, \$\} \\
\text{Follow}(A) &= \{a\} \\
\end{aligned}
$$

### 第一问

0. $I_0$

    $$
    \begin{aligned}
    S' &\to \cdot S, \$ \\
    S &\to \cdot aB, \$ \\
    \end{aligned}
    $$

1. $I_1$，对于 $I_0$ 移进 $S$：
    $$
    \begin{aligned}
    S' &\to S \cdot, \$ \\
    \end{aligned}
    $$
2. $I_2$，对于 $I_0$ 移进 $a$：

    $$
    \begin{aligned}
    S &\to a \cdot B, \$ \\
    B &\to \cdot aBAB, \$ \\
    B &\to \cdot \varepsilon, \$ \\
    \end{aligned}
    $$

3. $I_3$，对于 $I_2$ 移进 $B$：

    $$
    \begin{aligned}
    S &\to aB \cdot, \$ \\
    \end{aligned}
    $$

4. $I_4$，对于 $I_2$ 移进 $a$：

    $$
    \begin{aligned}
    B &\to a \cdot BAB, \$ \\
    B &\to \cdot aBAB, +/* \\
    B &\to \cdot \varepsilon, +/* \\
    \end{aligned}
    $$

    这里由于 LR(1) 语法的 $\text{Closure}$ 操作发生自 $B \to a \cdot BAB, \$$，所以展望符计算为 $\text{First}(A\$) = \{+, *\}$

5. $I_5$，对于 $I_4$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aB \cdot AB, \$ \\
    A &\to \cdot +, a/\$ \\
    A &\to \cdot *, a/\$ \\
    \end{aligned}
    $$

6. $I_6$，对于 $I_4$ 移进 $a$：

    $$
    \begin{aligned}
    B &\to a \cdot BAB, +/* \\
    B &\to \cdot aBAB, +/* \\
    B &\to \cdot \varepsilon, +/* \\
    \end{aligned}
    $$

7. $I_7$，对于 $I_5$ 移进 $A$：

    $$
    \begin{aligned}
    B &\to aBA \cdot B, \$ \\
    B &\to \cdot aBAB, \$ \\
    B &\to \cdot \varepsilon, \$ \\
    \end{aligned}
    $$

8. $I_8$，对于 $I_5$ 移进 $+$：

    $$
    \begin{aligned}
    A &\to + \cdot, a/\$ \\
    \end{aligned}
    $$

9. $I_9$，对于 $I_5$ 移进 $*$：

    $$
    \begin{aligned}
    A &\to * \cdot, a/\$ \\
    \end{aligned}
    $$

10. $I_{10}$，对于 $I_6$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aB \cdot AB, +/* \\
    A &\to \cdot +, +/*/a \\
    A &\to \cdot *, +/*/a \\
    \end{aligned}
    $$

11. $I_{11}$，对于 $I_6$ 移进 $a$：

    $$
    \begin{aligned}
    B &\to a \cdot BAB, a/+/* \\
    B &\to \cdot aBAB, +/* \\
    B &\to \cdot \varepsilon, +/* \\
    \end{aligned}
    $$

12. $I_{12}$，对于 $I_7$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aBAB \cdot, \$ \\
    \end{aligned}
    $$

13. $I_{13}$，对于 $I_{10}$ 移进 $A$：

    $$
    \begin{aligned}
    B &\to aBA \cdot B, +/* \\
    B &\to \cdot aBAB, +/* \\
    B &\to \cdot \varepsilon, +/* \\
    \end{aligned}
    $$

14. $I_{14}$，对于 $I_{10}$ 移进 $+$：

    $$
    \begin{aligned}
    A &\to + \cdot, a/+/* \\
    \end{aligned}
    $$

15. $I_{15}$，对于 $I_{10}$ 移进 $*$：

    $$
    \begin{aligned}
    A &\to * \cdot, a/+/* \\
    \end{aligned}
    $$

16. $I_{16}$，对于 $I_{11}$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aB \cdot AB, a/+/* \\
    A &\to \cdot +, a/+/* \\
    A &\to \cdot *, a/+/* \\
    \end{aligned}
    $$

17. $I_{17}$，对于 $I_{13}$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aBAB \cdot, +/* \\
    \end{aligned}
    $$

18. $I_{18}$，对于 $I_{16}$ 移进 $A$：

    $$
    \begin{aligned}
    B &\to aBA \cdot B, a/+/* \\
    B &\to \cdot aBAB, a/+/* \\
    B &\to \cdot \varepsilon, a/+/* \\
    \end{aligned}
    $$

19. $I_{19}$，对于 $I_{18}$ 移进 $B$：
    $$
    \begin{aligned}
    B &\to aBAB \cdot, a/+/* \\
    \end{aligned}
    $$

### 第二问

0. $I_0$

    $$
    \begin{aligned}
    S' &\to \cdot S, \$ \\
    S &\to \cdot aB, \$ \\
    \end{aligned}
    $$

1. $I_1$，对于 $I_0$ 移进 $S$：
    $$
    \begin{aligned}
    S' &\to S \cdot, \$ \\
    \end{aligned}
    $$
2. $I_2$，对于 $I_0$ 移进 $a$：

    $$
    \begin{aligned}
    S &\to a \cdot B, \$ \\
    B &\to \cdot aBAB, \$ \\
    B &\to \cdot \varepsilon, \$ \\
    \end{aligned}
    $$

3. $I_3$，对于 $I_2$ 移进 $B$：

    $$
    \begin{aligned}
    S &\to aB \cdot, \$ \\
    \end{aligned}
    $$

4. $I_4$，对于 $I_2$ 移进 $a$：

    $$
    \begin{aligned}
    B &\to a \cdot BAB, a/+/*/\$ \\
    B &\to \cdot aBAB, +/* \\
    B &\to \cdot \varepsilon, +/* \\
    \end{aligned}
    $$

    这里由于 LR(1) 语法的 $\text{Closure}$ 操作发生自 $B \to a \cdot BAB, \$$，所以展望符计算为 $\text{First}(A\$) = \{+, *\}$

5. $I_5$，对于 $I_4$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aB \cdot AB, a/+/*/\$ \\
    A &\to \cdot +, a/+/*/\$ \\
    A &\to \cdot *, a/+/*/\$ \\
    \end{aligned}
    $$

6. $I_6$，对于 $I_5$ 移进 $A$：

    $$
    \begin{aligned}
    B &\to aBA \cdot B, a/+/*/\$ \\
    B &\to \cdot aBAB, a/+/*/\$ \\
    B &\to \cdot \varepsilon, a/+/*/\$ \\
    \end{aligned}
    $$

7. $I_7$，对于 $I_6$ 移进 $B$：

    $$
    \begin{aligned}
    B &\to aBAB \cdot, a/+/*/\$ \\
    \end{aligned}
    $$

8. $I_{8}$，对于 $I_5$ 移进 $+$：

    $$
    \begin{aligned}
    A &\to + \cdot, a/+/*/\$ \\
    \end{aligned}
    $$

9. $I_{9}$，对于 $I_5$ 移进 $*$：
    $$
    \begin{aligned}
    A &\to * \cdot, a/+/*/\$ \\
    \end{aligned}
    $$

## Ex 4.7.4

说明下面的文法是 LALR（1）的，但不是 SLR（1）的。

$$
S \to Aa \mid bAc \mid dc \mid bda \\
A \to d
$$

### 解答

准备：添加新起始符号 $S'$ ，提取左公因子和消除左递归：

$$
\begin{aligned}
S' &\to S \\
S &\to Aa \mid bB \mid dc \\
B &\to Ac \mid da \\
A &\to d \\
\end{aligned}
$$

计算得到：

$$
\begin{aligned}
\text{Follow}(S) &= \{\$\} \\
\text{Follow}(A) &= \{a,c\} \\
\text{Follow}(B) &= \{\$\} \\
\end{aligned}
$$

LR(0) 项集族：

$$
\begin{aligned}
I_0 &: \{ S' \to \cdot S, S \to \cdot Aa, S \to \cdot bB, S \to \cdot dc, A \to \cdot d \} \\
I_1 &: \{ S' \to S\cdot \} \\
I_2 &: \{ S \to A\cdot a\} \\
I_3 &: \{ S \to Aa\cdot \} \\
I_4 &: \{ S \to b\cdot B, B \to \cdot Ac, B \to \cdot da, A \to \cdot d \} \\
I_5 &: \{ S \to bB\cdot \} \\
I_6 &: \{ B \to A\cdot c \} \\
I_7 &: \{ B \to d\cdot a, A \to d\cdot \} \\
I_8 &: \{ S \to d\cdot c, A \to d\cdot \} \\
I_9 &: \{ S \to dc\cdot\} \\
\end{aligned}
$$

注意到，对于 $I_8$，有：

$$
\begin{aligned}
I_8 &: \{ S \to d\cdot c, A \to d\cdot \}
\end{aligned}
$$

且，$\text{Follow}(A) = \{a,c\}$，此时，不满足 SLR(1) 文法要求的两两不交的原则，出现规约-移入冲突，所以该文法不是 SLR(1) 文法。

又，可以在 LR(0) 项集族的基础上构造出 LR(1) 项集族：

$$
\begin{aligned}
I_0 &: \{ [S' \to \cdot S, \$], [S \to \cdot Aa, \$], [S \to \cdot bB, \$], [S \to \cdot dc, \$], [A \to \cdot d, a] \} \\
I_1 &: \{ [S' \to S\cdot, \$] \} \\
I_2 &: \{ [S \to A\cdot a, \$] \} \\
I_3 &: \{ [S \to Aa\cdot, \$] \} \\
I_4 &: \{ [S \to b\cdot B, \$], [B \to \cdot Ac, \$], [B \to \cdot da, \$], [A \to \cdot d, c] \} \\
I_5 &: \{ [S \to bB\cdot, \$] \} \\
I_6 &: \{ [B \to A\cdot c, \$] \} \\
I_7 &: \{ [B \to d\cdot a, \$], [A \to d\cdot, c] \} \\
I_8 &: \{ [S \to d\cdot c, \$], [A \to d\cdot, a] \} \\
I_9 &: \{ [S \to dc\cdot, \$] \} \\
\end{aligned}
$$

合并同心集后保持不变，也没有冲突，所以此文法是 LALR(1) 文法。

## Ex 4.7.5

说明下面的文法是 LR（1）的，但不是 LALR（1）的。

$$
\begin{aligned}
S &\to Aa \mid bAc \mid Bc \mid bBa \\
A &\to d \\
B &\to d \\
\end{aligned}
$$

### 解答

准备：添加新起始符号 $S'$ ，提取左公因子和消除左递归：
$$
\begin{aligned}
S' &\to S \\
S &\to Aa \mid bC \mid Bc \\
A &\to d \\
B &\to d \\
C &\to Ac \mid Ba \\
\end{aligned}
$$

计算得到：

$$
\begin{aligned}
\text{Follow}(S) &= \{\$\} \\
\text{Follow}(A) &= \{a,c\} \\
\text{Follow}(B) &= \{a,c\} \\
\text{Follow}(C) &= \{\$\} \\
\end{aligned}
$$

构造出 LR(1) 项集族：

$$
\begin{aligned}
I_0 &: \{ [S' \to \cdot S, \$], [S \to \cdot Aa, \$], [S \to \cdot bC, \$], [S \to \cdot Bc, \$], [A \to \cdot d, a], [B \to \cdot d, c] \} \\
I_1 &: \{ [S' \to S\cdot, \$] \} \\
I_2 &: \{ [S \to A\cdot a, \$] \} \\
I_3 &: \{ [S \to Aa\cdot, \$] \} \\
I_4 &: \{ [S \to b\cdot C, \$], [C \to \cdot Ac, \$], [C \to \cdot Ba, \$], [A \to \cdot d, c], [B \to \cdot d, a] \} \\
I_5 &: \{ [C \to bC\cdot, \$] \} \\
I_6 &: \{ [C \to A\cdot c, \$] \} \\
I_7 &: \{ [C \to Ac\cdot, \$] \} \\
I_8 &: \{ [C \to B\cdot a, \$] \} \\
I_9 &: \{ [C \to Ba\cdot, \$] \} \\
I_{10} &: \{ [A \to d\cdot, c], [B \to d\cdot, a]\} \\
I_{11} &: \{ [S \to B\cdot c, \$] \} \\
I_{12} &: \{ [S \to Bc\cdot, \$] \} \\
I_{13} &: \{ [A \to d\cdot, a], [B \to d\cdot, c] \} \\
\end{aligned}
$$

这里不存在规约-规约冲突，也不存在移入-规约冲突，所以是 LR(1) 文法。

但是，合并同心集后，$I_{10}$ 和 $I_{13}$ 合并为

$$
\begin{aligned}
I_{10\&13} &: \{ [A \to d\cdot, a/c], [B \to d\cdot, a/c] \}
\end{aligned}
$$

此时，存在规约-规约冲突，所以不是 LALR(1) 文法。
