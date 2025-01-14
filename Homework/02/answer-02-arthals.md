# 编译原理第二次作业

<center>
  2110306206 卓致用
</center>
## Ex 3.6.2

为下述每一个语言设计一个 DFA 或 NFA。

### 解答

1. 包含 5 个元音的所有小写字母串，这些串中的元音按顺序出现

    ```
    not_vowel -> [bcdfghjklmnpqrstvwxyz]
    ans -> (not_vowel)*a(a|not_vowel)*e(e|not_vowel)*i(i|not_vowel)*o(o|not_vowel)*u(u|not_vowel)*
    ```

    ![3.6.2-1](./answer-02-arthals.assets/3.6.2-1.svg)

    注：这里规定字符集 $\Sigma$ 为小写字母集合，即不考虑各种空白字符、大写字符啥的（偷懒）

2. 所有由按词典递增排列的小写字母组成的串

    ```
    ans -> a*b*c*d*e*f*g*h*i*j*k*l*m*n*o*p*q*r*s*t*u*v*w*x*y*z*
    ```

    ![3.6.2-2](./answer-02-arthals.assets/3.6.2-2.svg)

3. 注释，即 `/*` 和 `*/` 之间的串，且串中没有不在双引号 ("") 中的 `*/`

    ```
    \/\*(".*"|[^\*"]*|\*+[^\/])*\*\/
    ```
    
    ![3.6.2-3](./answer-02-arthals.assets/3.6.2-3.svg)
    
4. 所有由偶数个 a 和奇数个 b 构成的串

    ```
    (aa|bb)*((ab|ba)(aa|bb)*(ab|ba)(aa|bb)*)*b(aa|bb)*((ab|ba)(aa|bb)*(ab|ba)(aa|bb)*)*
    ```

    第一题第五问复制两遍，中间单走一个 b

    ![3.6.2-4](./answer-02-arthals.assets/3.6.2-4.svg)

5. 所有由 a 和 b 组成且不含子序列 abb 的串

    ```
    b*a*b?
    ```

    ![3.6.2-5](./answer-02-arthals.assets/3.6.2-5.svg)
