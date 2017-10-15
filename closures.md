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





