### 烧脑的递归

我还记得自己一开始大量写递归函数的经历，那段时间我分别用ML和Scheme来写递归，烧脑，写了不少，但是还是不得其法。后来看了The Little Schemer，诶，我后悔没有早点接触此书。书中介绍的“十诫”\(ten commandments\)，启发极大。尤其是其中的几个写递归的技巧，如：

> The fifth Commandment:
>
> When building a value with **+**,always use0for the value of the terminating line, for adding0does not change the value of an addition.
>
> When building a value with **x**,always use1for the value of the terminating line, for multiplying by1does not change the value of a multiplication.
>
> When building a value with **cons**,always consider\(\)for the value of the terminating line.
>
> The Seventh Commandment:
>
> Recur on the **subparts **that are of the **same nature**:
>
> •On the sublists of a list.  
> •On the subexpressions of an arithmetic
>
> expression.

遵照这些技巧继续敲代码，慢慢地，递归就通了。后来再去写Essentials of Programming Languages中的递归作业，就很轻松了。

### 规则

递归的核心，上述的第七条戒律描述得很精彩。`recur on the subparts that are of the same nature`是用某个规则不断地应用于数据，直至该数据满足结束的条件。因此，在一个递归算法中，必然需要先写判断函数，决定是否结束该递归。如下面这段代码：

```racket
(define (PlusTwo x)
  (cond ((>= x 0)
         x)
        ((< x 0)
         (+ 1 PlusTwo(+ x 2)))))
```

当x为负数时，则继续调用该函数，只是参数发生了变化。当x大于0时，该递归结束。

### 尾递归

尾递归的意义，在于节省时间空间开销。在上述的例子中\( + 1 PlusTwo\(+ x 2\)\),每次调用新的函数，都不能完全清空上一个函数的内容，必须要等到最后有了一个确定答案之后，才能不断减小程序占用的空间。尾递归的意义，在于试图将函数的最终返回值，只有递归函数存在，这样就不需要那么多的时空开销。

