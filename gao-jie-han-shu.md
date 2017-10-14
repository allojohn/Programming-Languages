> The importance of this is that there are
>
> powerful program-design techniques that rely on the ability to blur the
>
> traditional distinction between "passive" data and "active" processes.

### 高阶函数的意义

高阶函数（first-class functions）的意义，在于通过增强函数的功能，使程序可以做到更好的抽象。函数有以下的特点：

1. 可以被变量命名
2. 将函数作为参数传入函数
3. 将函数作为返回值
4. 可以被包含在数据结构当中

现在回过头来再看，还是觉得，C、早期的Java等语言，它们抽象机制还是太弱了。传入函数、返回函数应该是语言的标配才对。最早实现函数的第一等公民特点的，是Lisp，欣慰的是，虽然Lisp这门语言已经成为历史，但是它使用的诸多语言特性，已经逐渐被许多主流语言引用，像Java目前的函数式、垃圾回收机制，均师承Lisp。

### 例子

#### 将函数作为传入参数

```ML
fun addition function x =
   1 + (function x)
```

经编译后，REPL返回如下信息：

> val addition = fn : \('a -&gt; int\) -&gt; 'a -&gt; int

在该程序中，函数接受两个参数，其中function 也是函数，于是，传入的functin函数实际上可以有不同方式，该函数满足的条件是：以x参数返回一个int类型。这便做到了**过程抽象（procedural abstraction）**。

#### 将函数作为返回值

```racket
(define (judge x)
  (cond ((< x 0)
         (lambda (y)
           (+ y x)))
        ((>= x 0)
         (lambda (y)
           (- y x)))))
```

在上述例子中，最终返回的是lambda形式的函数，该函数接收一个参数对原参数x进行计算。REPL中的显示如下：

```
> (define a (judge 9))
> a
#<procedure>
> (a 8)
-1
```

#### 在函数中创建函数

在学校里学C、java的时候，老师们强调的一点，是在函数中千万不能创建新的函数，需要在函数体外创建才对。这一点被强调了太多，以至于，我们甚至认为这是一条普遍原理。

在ML中，我们可以看到，完全可以在函数体内部创建一个辅助函数\(auxiliary function\)来写函数：

```
fun operate x =
  let
      fun Add x =  x + 1
      fun Minus x = x -1
  in
      if (x >= 0)
      then Add x
      else Minus x
  end
```

Add、Minus 均为辅助函数，它们在函数体内创建，在函数外则无法调用。



### 函数被包含于数据结构

最经典的将函数与数据结构结合的，恐怕就是map、filter了，其实他们的实现也很简单：

```ML
fun mymap  f x =
  case x of
      [] => []
   | xs :: xs' => (f xs) :: (mymap f xs')
   
fun myfilter f x =
  case x of
      [] => []
    | xs :: xs => if (f xs)
		  then xs :: (myfilter f xs')
		  else (myfilter f xs')   
```







