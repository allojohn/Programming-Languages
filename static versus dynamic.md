> 静态类型检查类似“疑罪从有”的有罪推定制——再被证明合法之前是非法的，动态类型检查遵循"疑罪从无"的无罪推定制——在被证明合法之前是非法的。

### 概述
静态类型语言与动态类型语言的最大区别，在于类型检查的不同。前者在编译期间完成类型检查，后者在程序运行的过程中才完成对类型的推断。
《十二公民》
![alt text](/Users/mac/www.jiangxinwen.com/gitbook.jiangxinwen.com/Programming-Languages/assets/static and dynamic checking.png "eagerness")

#### duck typing

> If I walk like a duck and quack like a duck ,then I am a duck.
                                         -- fake duck

duck typing 是动态语言的典型特征，或者说，动态类型天然地适合duck typing。下面是duck typing的例子：
```
class Duck                     #会叫会游的鸭
  def shout
    puts '呷呷呷'
  end
  def swim
    puts '鸭泳'
  end
end

class Frog                     #会叫会游的蛙
  def shout
    puts '呱呱呱'
  end
  def swim
    puts '蛙泳'
  end
end

def shoutAndSwim(duck)       #让一只会叫会游的家伙边叫边游
  duck.shout
  duck.swim
end

shoutAndSwim(Duck.new)        #让一只鸭边叫边游
shoutAndSwim(Frog.new)        #让一只蛙边叫边游
  ````
  
  原本定义的 `shoutAndSwim` 只是为了让Duck使用，Frog具有同样的功能，因此同样可以调用该方法。换句话说，duck typing不看重名称，而是关注能力。它并不严格要求传入的参数必须是指定的类，只要其他类的对象同样可以满足该函数要去即可。因此动态语言天然地适合这种实现，因为它没有从一开始就限定类型。
  然而duck typign虽然看起来非常灵活的，但它同样也是bug的温床。因为程序设计的初衷，并非为了其他类考虑。如果实现者需要修改原来的设计，就需要**额外考虑**已经使用了duck typing的其他类，因此多了几分顾忌与难度。另一方面，使用者(client)本身也需要小心，防止误用。使用者很多时候只能看到interface，而具体的细节看不到，所以很可能出错。
  
#### generics
动态语言天然地更容易做到函数抽象，写出的函数可以接受多种类型。对于像Java这样的静态语言，语言表现力就没有那么强了。限定类型意味着抽象能力减弱，写出的函数只能适用于特定类型。早期的解决办法，是将函数接受的参数为Object，而所有的其他类型都要继承Object。如下所示：
```
StackOfObject s = new StackOfObjects();//a stack take only one explicit type.
Apple a = new Apple();
Orange b = new Orange();
s.push(a);
s.push(b);               // the bug can't be detected on **compile** time.
a = (Apple) (s.pop());   // runtime error,not compile time error.
```

堆栈s可以接受任意类型的变量，但是它的问题是，传入的类型可以不统一，所以如果作者的本义是只将Apple类型传入s，那么其他类型也可以传入，如Orange类。这违反了实现者的初衷。试看一个ML的例子：
```
- map;
val it = fn : ('a -> 'b) -> 'a list -> 'b list
```
虽然我们笼统地称map可以接受任意类型，但是映射的函数并非可以任意选择。传入映射函数的数据类型需要与数组中元素的类型系统。这在REPL中的现实也很清楚: `fn : ('a -> 'b) -> 'a list -> 'b list `
另一方面，即使这样的事情发生了，也无法在编译时发现错误，因为他们都是Object类，所以不会报错。而这样就丧失了静态语言的优势。最终，Java引入泛型(generics)，用来抽象函数。以上关于堆栈的实现可写成：
```
Stack Apple s = new Stack<Apple>;
Apple a = new Apple();
Orange b = new Orange();
s.push(a);
s.push (b); //error at run-time
```
Stack被限制为Apple类型，所以当传入Orange类型时，编译器报错。

#### type inference
静态语言并不必然需要需要类型声明。type inference便是典型。如下的例子说明，静态语言并不必然需要显式声明类型，因为编译过程总可以通过类型推导(type inference)来发现变量的类型。
```
fun plus x y =
  x + y
  ```
  
王垠在[文章]中曾经说道，如果一个程序员没有在Haskell、ML等语言中写过代码，那么它根本不可能意思到类型的重要意义。我觉得类型推断对于使用动态语言有很大帮助。程序员在写动态语言时尝试去推导变量的类型，因此多了几分小心。像Java这样的静态语言，必须要显示地声明类型，类型推导在此则没有意义。

#### 强弱类型
语言的类型强弱，与语言是静态还是动态类型，没有对应关系。类型的强弱以类型的**约束强度**来划分，而类型的动静以类型的**绑定时间**划分，二者是正交的。以弱类型的C语言为例，虽然它是静态类型语言，但是在下面的例子中，类型的约束很弱：`int a = 1 + "2";` 在运行时，编译器会将“2”隐式转化为整形，最终这个运行可以通过。正是这种“有趣灵活”的转换方式，带来的隐含的bug。对于强类型语言，如Java，以上的代码则无法通过，必须要加入类型转换，将String 类型转化为Integer 类型。
