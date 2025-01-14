# 编译原理第三次作业

<center>
  2110306206 卓致用
</center>
## Ex 3.7.1

将下列图中的 NFA 转换为 DFA。

### 解答

#### 第 2 问

NFA：

![image-20250114121218931](./answer-03-arthals.assets/image-20250114121218931.png)

答案：

![3.7.1-1](./answer-03-arthals.assets/3.7.1-1.svg)

#### 第 3 问

NFA：

![image-20250114121242788](./answer-03-arthals.assets/image-20250114121242788.png)

答案：

![3.7.1-2](./answer-03-arthals.assets/3.7.1-2.svg)

## Ex3.7.3

使用算法 3.23 和 3.20 将下列正则表达式转换成 DFA。

### 第三问

$$
((ε|a)b*)*
$$

#### 解答

##### NFA

![3.7.3-1-NFA](./answer-03-arthals.assets/3.7.3-1-NFA.svg)

##### DFA

![3.7.3-1-DFA](./answer-03-arthals.assets/3.7.3-1-DFA.svg)

##### DFA 化简

![3.7.3-1-DFA-Simple](./answer-03-arthals.assets/3.7.3-1-DFA-Simple.svg)

### 第 4 问

$$
(a|b)*abb(a|b)*
$$

#### 解答

##### NFA

![3.7.3-2-NFA](./answer-03-arthals.assets/3.7.3-2-NFA.svg)

##### DFA

![3.7.3-2-DFA](./answer-03-arthals.assets/3.7.3-2-DFA.svg)

注：由于状态太多，略去集合括号、逗号，并以“-”标记序号相连的多个状态

##### DFA 化简

![3.7.3-2-DFA-Simple](./answer-03-arthals.assets/3.7.3-2-DFA-Simple.svg)

