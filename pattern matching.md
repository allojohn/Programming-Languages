模式匹配(pattenr-matching)是函数式语言分解(decompose)程序的方式。它在不同的语言中都有使用，如ML、Haskell、Scala、C#。OOP以对象为单位，而函数式则以函数为单位，函数处理的，则是不同类型的数据。这便引出了模式匹配。以ML为例，模式匹配的写法如下所示：

```
datatype mytype = TwoInts of int * int 
                | Str of string 
                | Pizza

fun f x = 
    case x of 
	Pizza => 3 
      | Str s => 8
      | TwoInts(i1,i2) => i1 + i2
	  
```

在第一个例子中，我们首先定义了3种数据类型：TwoInts,Str,Pizza，而之后的函数，则可以对上述的类型做模式匹配。如果是Pizza类型，则结果为3，TwoInts，则将两个数相加。

#### 类型检查与访问同时进行
在上述的函数f 中，`| TwoInts(i1, i2) => i1 + i2` 其实做了两件事：

1. 将传入的参数x匹配为TwoInts类型。
2. 将x分为两部分：i1,i2。这样，之后`i1 + i2` 操作就显得很容易了。

另一个例子，这样的特点更加明显：
```
fun zip3 list_triple =
    case list_triple of 
	([],[],[]) => []
      | (hd1::tl1,hd2::tl2,hd3::tl3) => (hd1,hd2,hd3)::zip3(tl1,tl2,tl3)
      | _ => raise ListLengthMismatch

```

上述函数实现的功能，是将3元数组`[a1,a2,a3...] [b1,b2,b3...] [c1,c2,c3]`编码为`[a1,b1,c1] [a2,b2,c2]...`。
使用`(hd1::tl1,hd2::tl2,hd3::tl3)`操作，实际上，在判断类型的同时，已经完成了对数据的进一步操作。所以在`=>`之后的操作，也就容易了。