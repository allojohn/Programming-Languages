### 烧脑的递归

我还记得自己一开始大量写递归函数的经历，那段时间我分别用ML和Scheme来写递归，烧脑，写了不少，但是还是不得其法。后来看了The Little Schemer，诶，我后悔没有早点接触此书。书中介绍的“十诫”\(ten commandments\)，启发极大。尤其是其中的几个写递归的技巧，如：

> **The fifth Commandment**:
>
> * When building a value with **+**,always use0for the value of the terminating line, for adding0does not change the value of an addition.
>
> * When building a value with **x**,always use1for the value of the terminating line, for multiplying by1does not change the value of a multiplication.
>
> * When building a value with **cons**,always consider\(\)for the value of the terminating line.
>
> **The Seventh Commandment**:
>
> Recur on the **subparts **that are of the **same nature**:
>
> * On the sublists of a list.
> * On the subexpressions of an arithmetic expression.

遵照这些技巧继续敲代码，慢慢地，递归就通了。后来再去写Essentials of Programming Languages中的递归作业，就很轻松了。

### 规则

递归的核心，上述的第七条戒律描述得很精彩:`recur on the subparts that are of the same nature.`用某个规则不断地应用于数据的子部分\(subpart\)，直至该数据满足结束的条件。因此，在一个递归算法中，必然需要先写判断函数，决定是否结束该递归。如下面这段代码：

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

### 陷阱

在写递归函数的时候，有一些值得关注。

结合eager evaluation，SICP中有一道作业令我印象深刻，大致如下：

小明用Scheme自定义了自己的if 函数：

```racket
(define (my-if my-case a b)
  (if my-case
      a
      b))

(define (factorial x)
  (my-if (= x 1)
         1
         (* x (factorial (- x 1)))))
```

然而如果在REPL中输入`(factorial 10)` ，会发现，程序根本没有停下来。因为在运行的时候，使用的是**eager evaluation**，参数如果是函数，它会将继续evaluate该函数，导致程序不断运行。在原生的if函数中，有着明显的优先级关系：if 条件句**优先调**用，它的结果决定了之后的路线;而在my-if中，my-if的3个参数是并列的，它们之间并没有那么强的优先级关系。于是，在调用`(factorial 0)`的时候，悲剧发生了，因为并列关系，虽然`(= x 1)`为true，但是两个分值都继续运行着，不受其影响：`(my-if true 1 ( 0 (factorial -1)`，于是这个函数便将一直运行着。

#### 矛盾的解决

如果想让my-if 正常运行，那得是另一种形式，使用delayed evaluation。总结一些，其实delayed evaluation解决的是这样的一个**矛盾**：程序语言本身是eager evalution，如果传入参数，那就立即evaluate该参数，而使用者希望该函数能够在**调用该参数时**evalute。解决这一矛盾的，是lambda，既然lambda 可以传入空参数，那么直接给原函数加一个lambda外衣就好了：

```
(define (my-if my-case a b)
  (if my-case
      (a)
      (b)))

(define (factorial x)
  (my-if (= x 1)
         (lambda () 1)
         (lambda ()
           (* x (factorial (- x 1))))))
      
```



