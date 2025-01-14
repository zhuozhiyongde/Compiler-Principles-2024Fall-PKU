# 编译原理第九次作业

<center>
  2110306206 卓致用
</center>
## Ex 5.3.2

给出一个 SDD，将一个带有 $+$ 和 $*$ 的中缀表达式翻译成没有冗余括号的表达式。例如，因为两个运算符都是左结合的，并且 $*$ 的优先级高于 $+$，所以 $((a * (b + c)) * (d))$ 可翻译为 $a * (b + c) * d$。

### 解答

$$
\begin{array}{|c|l|l|}
\hline
& \text{产生式} & \text{语义规则} \\
\hline
1 & E \to E_1 + T &
\begin{array}{l}
E.\text{wrapped} &=& \text{false} \\
E.\text{priority} &=& 0 \\
E.\text{expr} &=& E_1.\text{expr} || \text{“+”} || T.\text{expr} \\
E.\text{cleanExpr} &=& (E_1.\text{wrapped} ? E_1.\text{cleanExpr} : E_1.\text{expr}) ||\\
&&\text{“+”} ||\\
&&(T.\text{wrapped} ? T.\text{cleanExpr} : T.\text{expr})
\end{array} \\
\hline
2 & E \to T &
\begin{array}{l}
E.\text{wrapped} &=& T.\text{wrapped} \\
E.\text{priority} &=& T.\text{priority} \\
E.\text{expr} &=& T.\text{expr} \\
E.\text{cleanExpr} &=& T.\text{cleanExpr}
\end{array} \\
\hline
3 & T \to T_1 * F &
\begin{array}{l}
T.\text{wrapped} &=& \text{false} \\
T.\text{priority} &=& 1 \\
T.\text{expr} &=& T_1.\text{expr} || \text{“*”} || F.\text{expr} \\
T.\text{cleanExpr} &=& (T_1.\text{wrapped} \&\& T_1.\text{priority} \ge 1 ? T_1.\text{cleanExpr} : T_1 ) || \\
&&\text{“*”} || \\
&&(F.\text{wrapped} \&\& F.\text{priority} \ge 1 ? F.\text{cleanExpr} : F.\text{expr})
\end{array} \\
\hline
4 & T \to F &
\begin{array}{l}
T.\text{wrapped} &=& F.\text{wrapped} \\
T.\text{priority} &=& F.\text{priority} \\
T.\text{expr} &=& F.\text{expr} \\
T.\text{cleanExpr} &=& F.\text{cleanExpr}
\end{array} \\
\hline
5 & F \to (E) &
\begin{array}{l}
F.\text{wrapped} &=& \text{true} \\
F.\text{priority} &=& E.\text{priority} \\
F.\text{expr} &=& \text{“(”} || E.\text{expr} || \text{“)”} \\
F.\text{cleanExpr} &=& E.\text{expr}
\end{array} \\
\hline
6 & F \to \text{digit} &
\begin{array}{l}
F.\text{wrapped} &=& \text{false} \\
F.\text{priority} &=& 3 \\
F.\text{expr} &=& \text{digit} \\
F.\text{cleanExpr} &=& \text{digit}
\end{array} \\
\hline
\end{array}
$$

根节点的 $E.\text{cleanExpr}$ 即为最终结果。

## Ex 5.4.3

下面的 SDT 计算了一个由 0 和 1 组成的串的值。它把输入的符号串当作按照正二进制数来解释。

$$
\begin{align}
B &\to B_1 \ 0 \ \{ B.val = 2 \times B_1.val \} \\
  &\quad \mid \ B_1 \ 1 \ \{ B.val = 2 \times B_1.val + 1 \} \\
  &\quad \mid \ 1 \ \{ B.val = 1 \}
\end{align}
$$

改写这个 SDT，使得基础文法不再是左递归的，但仍然可以计算出整个输入串的相同的 $B.val$ 的值。

### 解答

$$
\begin{array}{|c|l|l|}
\hline
& \text{产生式} & \text{语义规则} \\
\hline
1 & S \to 1 B & S.val = B.val + 2^{B.len} \\
\hline
2 & B \to A B_1 & B.val = B_1.val + A.val \times 2^{B_1.len} \\
  & & B.len = B_1.len + 1 \\
\hline
3 & B \to \varepsilon & B.val = 0 \\
  & & B.len = 0 \\
\hline
4 & A \to 0 & A.val = 0 \\
\hline
5 & A \to 1 & A.val = 1 \\
\hline
\end{array}
$$

根节点的 $S.\text{val}$ 即为最终结果。
