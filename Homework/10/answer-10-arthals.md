# 编译原理第十次作业

<center>
  2110306206 卓致用
</center>

## Ex 5.4.4(1)

为下面的产生式写出一个和例 5.10 类似的 $L$ 属性 SDD。这里的每个产生式表示一个常见的 C 语言中那样的控制流结构。你可能需要生成一个三地址语句来跳转到某个标号 $L$，此时你可以生成语句 `goto L`。

$$
S \rightarrow \text{if } (C) \text{ then } S_1 \text{ else } S_2
$$

请注意，列表中的任何语句都可能包含一条从它的内部跳转到下一个语句的跳转指令，因此简单地为每个语句按顺序生成代码是不够的。

### 解答

$$
\begin{align*}
&L1 = \text{new()} \\
&L2 = \text{new()} \\
&S_1.\text{next} = S.\text{next} \\
&S_2.\text{next} = S.\text{next} \\
&C.\text{true} = L1 \\
&C.\text{false} = L2 \\
&S.\text{code} = C.\text{code} \ || \ \text{“label”} \ || \ L1 \ || \ S_1.\text{code} \ || \ \text{“label”} \ || \ L2 \ || \ S_2.\text{code}
\end{align*}
$$

## Ex 5.4.5(1)

按照例 5.19 的方法，把在练习 5.4.4 中得到的各个 $SDD$ 转换成一个 $SDT$。

### 解答

$$
\begin{array}{|l|l|}
\hline
\text{产生式片段} & \text{动作} \\
\hline
S \to \text{if} (
  & L1 = \text{new()} \\
  & L2 = \text{new()} \\
  & C.\text{true} = L1 \\
  & C.\text{false} = L2 \\
\hline
C)
  & S_1.\text{next} = S.\text{next} \\
\hline
S_1 \text{ else }
  & S_2.\text{next} = S.\text{next} \\
\hline
S_2
  & S.\text{code} = C.\text{code} \ || \ \text{“label”} \ || \ L1 \ || \ S_1.\text{code} \ || \ \text{“label”} \ || \ L2 \ || \ S_2.\text{code} \\
\hline
\end{array}
$$

## Ex 5.5.1(1)

按照 5.5.1 节的风格，将练习 5.4.4 中得到的每个 $SDD$ 实现为递归下降的语法分析器。

### 解答

```cpp
void check_token_is(string token){
  assert(cur_input == token);
  get_next_token();
}

string S(label next){
  if (cur_input == "if"){
    get_next_token();
    check_token_is("(");
    label L1 = new_label();
    label L2 = new_label();
    string C_code = C(L1, L2);
    check_token_is(")");
    string S1_code = S1(next);
    check_token_is("else");
    string S2_code = S2(next);
    // 使用 + 代替原文的 || 作为字符串拼接
    string S_code = C_code + "label" + L1 + S1_code + "label" + L2 + S2_code;
    return S_code;
  }
  else{
    // TODO
  }
}
```

## Ex 5.5.2(1)

按照 5.5.2 节的风格，将练习 5.4.4 中得到的每个 $SDD$ 实现为递归下降的语法分析器。

### 解答

```cpp
void check_token_is(string token){
  assert(cur_input == token);
  get_next_token();
}

void S(label next){
  if (cur_input == "if"){
    get_next_token();
    check_token_is("(");
    label L1 = new_label();
    label L2 = new_label();
    C(L1, L2);
    check_token_is(")");
    printf("label %s\n", L1);
    S1(next);
    printf("label %s\n", L2);
    S2(next);
  }
  else{
    // TODO
  }
}
```

## Ex 5.5.5(1)

按照 5.5.4 节的风格，将练习 5.4.4 中得到的每个 $SDD$ 和一个 $LR$ 语法分析器一起实现。

### 解答

$$
\begin{array}{|l|l|}
\hline
\text{产生式} & \text{规约时动作} \\
\hline
S \to \text{if} (XC) Y S_1 \text{ else } Z S_2
  & \text{tempCode} = \text{stack[top -6].code} \, || \\
  & \quad \text{“label”}\, || \text{stack[top - 7].L1} \, || \text{stack[top - 3].code} \, || \\
  & \quad \text{“goto”}\, || \text{stack[top - 10].next} \\
  & \quad \text{“label”}\, || \text{stack[top - 7].L2} \, || \text{stack[top].code} \, || \\
  & \text{top -= 9} \\
  & \text{stack[top].code = tempCode} \\
\hline
X \to \varepsilon
  & \text{top += 1} \\
  & \text{stack[top].L1 = new()} \\
  & \text{stack[top].L2 = new()} \\
  & \text{stack[top].true = stack[top].L1} \\
  & \text{stack[top].false = stack[top].L2} \\
\hline
Y \to \varepsilon
  & \text{top += 1} \\
  & \text{stack[top].next = stack[top - 6].next} \\
\hline
Z \to \varepsilon
  & \text{top += 1} \\
  & \text{stack[top].next = stack[top - 9].next} \\
\hline
\end{array}
$$

栈结构：

$$
\begin{array}{c}
\hline
0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 \\
\hline
? & if & ( & X & C & ) & Y & S_1 & else & Z & S_2 \\
\hline
S.\text{next} & & & L_1 & C.\text{code} & & S_1.\text{next} & S_1.\text{code} & & S_2.\text{next} & S_2.\text{code} \\
 & & & L_2 & & & & & & & \\
 & & & C.\text{true} & & & & & & & \\
 & & & C.\text{false} & & & & & & & \\
\hline
\end{array}
$$
