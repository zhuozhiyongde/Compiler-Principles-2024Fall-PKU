# 编译原理第十一次作业

<center>
  2110306206 卓致用
</center>
## Ex. 6.1.1

为下面的表达式构造 DAG

$$((x+y) - ((x+y) \times (x-y))) + ((x+y) \times (x-y))$$

### 解答

![ex_6_1_1](./answer-11-arthals.assets/ex_6_1_1.svg)

## Ex. 6.2.1

将算术表达式 $a + -(b + c)$ 翻译成

1. 抽象语法树
2. 四元式序列
3. 三元式序列
4. 间接三元式序列

### 解答

#### AST

![ex_6_2_1_ast](./answer-11-arthals.assets/ex_6_2_1_ast.svg)

#### 四元式序列

$$
\begin{array}{|c|c|c|c|}
\hline
\text{op} & \text{arg1} & \text{arg2} & \text{result} \\
\hline
\text{+} & b & c & t_1 \\
\text{minus} & t_1 & & t_2 \\
\text{+} & a & t_2 & t_3 \\
\hline
\end{array}
$$

#### 三元式序列

$$
\begin{array}{|c|c|c|c|}
\hline
\text{address} & \text{op} & \text{arg1} & \text{arg2} \\
\hline
\text{0} & \text{+} & b & c \\
\text{1} & \text{minus} & (0) & \\
\text{2} & \text{+} & a & (1) \\
\hline
\end{array}
$$

#### 间接三元式

$$
\begin{array}{|c|c|c|c|c|}
\hline
\text{instruction} & \text{address} & \text{op} & \text{arg1} & \text{arg2} \\
\hline
\text{(0)} & 0 & \text{+} & b & c \\
\text{(1)} & 1 & \text{minus} & (0) & \\
\text{(2)} & 2 & \text{+} & a & (1) \\
\hline
\end{array}
$$

## Ex. 6.2.2(2)

对下列赋值语句重复练习 6.2.1。

1. $a = b[i] + c[j]$
2. $a[i] = b \times c - b \times d$

### 解答

#### AST

![ex_6_2_2_ast](./answer-11-arthals.assets/ex_6_2_2_ast.svg)

#### 四元式序列

$$
\begin{array}{|c|c|c|c|}
\hline
\text{op} & \text{arg1} & \text{arg2} & \text{result} \\
\hline
\text{*} & b & c & t_1 \\
\text{*} & b & d & t_2 \\
\text{-} & t_1 & t_2 & t_3 \\
\text{*} & i & \text{element\_width} & t_4 \\
\text{=} & t_3 & & a[t_4] \\
\hline
\end{array}
$$

#### 三元式序列

$$
\begin{array}{|c|c|c|c|}
\hline
\text{address} & \text{op} & \text{arg1} & \text{arg2} \\
\hline
\text{0} & \text{*} & b & c \\
\text{1} & \text{*} & b & d \\
\text{2} & \text{-} & (0) & (1) \\
\text{3} & \text{*} & i & \text{element\_width} \\
\text{4} & \text{=} & (2) & a[3] \\
\hline
\end{array}
$$

#### 间接三元式

$$
\begin{array}{|c|c|c|c|c|}
\hline
\text{instruction} & \text{address} & \text{op} & \text{arg1} & \text{arg2} \\
\hline
\text{(0)} & 0 & \text{*} & b & c \\
\text{(1)} & 1 & \text{*} & b & d \\
\text{(2)} & 2 & \text{-} & (0) & (1) \\
\text{(3)} & 3 & \text{*} & i & \text{element\_width} \\
\text{(4)} & 4 & \text{=} & (2) & a[3] \\
\hline
\end{array}
$$

## Ex. 6.3.1

确定下列声明序列中各个标识符的类型和相对地址。

```c
float x;
record { float x; float y; } p;
record { int tag; float x; float y; } q;
```

### 解答

| 标识符 | 类型 | 相对地址|
| :----: | :----: | :----: |
| $x$ | $\text{float}$ | 0 |
| $p.x$ | $\text{float}$ | 4 |
| $p.y$ | $\text{float}$ | 8 |
| $p$ | $\text{record((x} \times \text{float)} \times \text{(y} \times \text{float))}$ | 4 |
| $q.tag$ | $\text{int}$ | 12 |
| $q.x$ | $\text{float}$ | 16 |
| $q.y$ | $\text{float}$ | 20 |
| $q$ | $\text{record((tag} \times \text{int)} \times \text{(x} \times \text{float)} \times \text{(y} \times \text{float))}$ | 12 |