## 递归

### 语感

我还记得自己一开始大量写递归函数的经历，那段时间我分别用ML和Scheme来写递归，烧脑，写了不少，但是还是不得其法。后来看了The Little Schemer，诶，我后悔没有早点看这本书，书中介绍的“十诫”\(ten commandments\)，启发极大。尤其是其中的了一个写递归的技巧：

> The fifth Commandment:
>
> When building a value with **+**,always use0for the value of the terminating line, for adding0does not change the value of an addition.
>
> When building a value with **x**,always use1for the value of the terminating line, for multiplying by1does not change the value of a multiplication.
>
> When building a value with **cons**,always consider\(\)for the value of the terminating line.

遵照这个技巧继续敲代码，慢慢地，递归就通了。后来再去写Essentials of Programming Languages中的递归作业，就很轻松了。

\#\#\#规则

递归的核心，是用某个规则不断地应用于数据，直至该数据满足结束的条件。因此，在一个递归算法中，必然需要先写判断函数，决定是否结束该递归。如下面这段代码：

\`\`\` 

\(define \(PlusTwo x\)

  \(cond \(\(&gt;= x 0\)

         x\)

        \(\(&lt; x 0\)

         \(PlusTwo\(+ x 2\)\)\)\)\)

\`\`\`





