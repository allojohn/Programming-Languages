### 对Ruby的一些疑惑
在学习Ruby的时候，产生了一些疑惑。列出它们只是作为记录，并不是为了批评。温伯格曾经说过一个法则，我很喜欢：
> Count-to-Three Principle:
If you cannot think of three ways of abusing a tool, you do not understand how to use it.”
-- Weinberg, Gerald.  An Introduction to General Systems Thinking.

说出了语言的种种缺点，才算对语言有了更好的理解，才会减少对语言的滥用，避免自己成为「铁锤人」。

#### monkey patching
monkey patching指的是，可以在类外，添加类的方法。这么做看起来是很方便，可以随意添加，但是如果这个类创建了其他对象，并且实现这个对象的是另外一个程序员B。那么就很可能出现问
题了。如果B创建了一个同名函数，原本的程序员A又将受到影响。
最大的问题是，程序员需要**不断跳转**，寻找所有的定义，这实际上非常耗精力。设计的一大理念就是[「经济原则」](http://survivor99.com/gevolution/zny/book/2-3.htm)，它的部分表现为，功能上高度相关的部件需要尽量存放在一起。在**大时间周期**的视角下，这样的设计才能经得起考验。后来读书的时发现这一推论也被写成了一个原则:-)：局部化原则（locality principle）——是代码的物理紧密度应与逻辑紧密度保持一致。
不由得想起Dijkstra在[获得图灵奖上的演讲](http://www.ituring.com.cn/article/71467)。

> 一个有能力的程序员能够意识到他自己脑容量的严格尺寸，因此，他谦逊的完成编程任务，牵涉到其他事情时，**他避免小聪明像躲避瘟疫一样**。

#### block
在我看来，block 完全多此一举。它不仅仅只是一个second-class function，而且因为它话只说了一半，结果程序员就一直需要记住这个负担。block 的功能就是一个“占位符”，不过它占的不是符号，而是一个函数。并且还可以额外传递给它传递。其实它完全可以模仿其他函数式语言的写法。
当其中又出现了proc这个first-class函数的写法时，你开始怀疑，作者一开始是不是没有吧first-class 这个语言特性设计好，只实现了传入函数的功能。

### late binding
late binding也被成为dynamic dispatch，是动态OOP的一大特点，在Ruby、Python等语言中均被使用。它指定了查询函数的规则：在当前的对象类中率先查找，如果没有，则追溯至它的父类，如果没有，再去查找它的父类。
```
classA ={private static void m = {......} }
classB extends classA;
classC extends classB;
classC instanceC = new classC();
instanceC .m()
```

如上所示，在该例中，当instanceC尝试调用方法m时，它率先在instanceC所属的类中查找，即classC，如果classC中不存在这样的类，那么就查询它的父类classB。这样的函数指定方式，是在运行期间(run-time)确立的，也因此为多态提供了条件。例如，classC自己自行定义函数m()，这样，当instanceC调用方法m()时，实际调用的是classC中的方法。
另一方面，late binding的使用也需要非常小心。因为它很可能会**破坏父类的函数封装**。这便是「脆弱的基类问题」试看以下例子：
```
classA = {
	private static int plus(int x) {return (x + 1); }
	private static int minusOne (int x) {return (plus(x) - 2);}
}
	
class classB extends classA{
	private static int plus (int x) { return (x + 1000);}
}
classB instanceB = new classB();
instanceB.minusOne();
```
在上述例子中，当instanceB试图调用minusOne(x)时，实际结果最终并不是x-1，因为在这个方法中mimusOne调用了plus函数，而plus在子类中被重新定义了。因此minusOne不再执行父类中的的功能，所以当进行函数重载的时候，需要倍加小心，因为该函数可能被其他方法调用。