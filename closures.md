#### 定义

闭包的意义在复用。如果变量、函数能够保存当时求值的环境，那之前的代码就没有白费。

```Racket
val x = 1
fun f y = x + y
val x = 2
val y = 3
val z = f (x + y)
```

在函数f定义的时候，它保存了当时x的值，将函数内的x映射为1。在之后调用该函数时，虽然又重新定义了x与y，但是原函数并不受其干扰，仍然**优先调用**早期环境中的内容。因此z的值为6。这样的一种优先调用早期环境的方式，成为lexical scope，而与之对应的，是dynaic scope。

总结一下，一个函数应该分为2个部分：

* 代码
* 储存变量的环境

#### 与高阶函数结合

在高阶函数的辅助下，闭包的威力进一步增强。

* partial evaluation，做到进一步抽象
* \*\*remain to be done\*\*
* with closures,we can avoid repeating computations that don't depend on function arguments.

##### 避免重复计算

```
fun allShorterThan1 (xs,s) = 
    filter (fn x => String.size x < (print "!"; String.size s), xs)

fun allShorterThan2 (xs,s) =
    let 
    val i = (print "!"; String.size s)
    in
    filter(fn x => String.size x < i, xs)
    end
```

以上两个函数完成相同的功能：筛选列表中长度小于s的元素。第一个函数的问题在于重复了太多计算：`String.size s`  在每个元素运算的时候，它都要被计算一次。第二个函数式在此基础上的改进：将本可以避免重复计算的`String.size s` 用let存入到环境中。因为lexical scope特点，所以不会被外面的同名变量覆盖。

##### 组合函数

```
fun compose (f,g) = fn x => f (g x)
```

这个例子中，f,g均为函数，做到很好的抽象：我们可以暂时只传入f函数，如`fun temp = compose (fn y => y + 1)` 于是，我们可以将temp传递给其他函数。它不需要完全传入f 与g参数后，才能做操作。

Currying便是其中的典型用法。Currying 的例子：

```ML
- val sorted3 = fn x=> fn y => fn z => (x > y) andalso (y > z);
val sorted3 = fn : int -> int -> int -> bool

- sorted3 1 2;
val it = fn : int -> bool
```

这样，即使只传入0-2个参数，由于closure可以保存环境，所以仍然可行。换成Racket更直观一些：

```
(define sorted3
  (lambda (a)
    (lambda (b)
      (lambda (c)
        (and (> a b)
             (> b c))))))
```

在REPL中测试如下：

```
> ((sorted3 1) 2)
#<procedure>
> (define x ((sorted3 1) 2))
> x
#<procedure>
> (x 3)
#f
```



