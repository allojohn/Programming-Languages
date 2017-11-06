[一个系统就是一个观点](https://book.douban.com/subject/26298694/)，同样，一个语言范式代表了一个独特的世界观。这样的特点，直接体现在**分解问题的方式**上。函数式的视角，是用函数去操作不同的数据，而OOP的视角，则是拆分为不同的对象，通过相互传递消息来解决问题。编程范式的比较，只有在[特定情境下才有意义](https://terrificjhony.gitbooks.io/intuition-pumps/content/29-the-wandering-two-bitser,twin-earth,and-the-giant-robot.html)。他们绝非仅仅与“品味”有关。
### 寻找表征
想要一下子说清楚OOP与FP的区别很困难。那么，有没有什么好的表征方式，让我们能够直观地对二者做一个对比？这个问题的表征方式是表格。如图所示，行表示的是数据结构，如Int,Add,Negate;列表示的是可能的操作,如eval,toString,hasZero。
![alt text](/Users/mac/www.jiangxinwen.com/gitbook.jiangxinwen.com/Programming-Languages/assets/extensibility.png "extensibility")

### 可扩展性
当我们试图在横轴一栏，也就是添加一个数据类型时，所有处理的函数都要相应增加。然而函数式的特点之一，就是通过函数来分解，这便意味着程序员需要在不同的函数之间跳转修改。因此这样会非常累赘。
对于OOP，添加的繁琐程度完全相反。当在数据结构一栏中添加一个新的数据类型的候，我们相当于添加一个新的类，并在这个类中添加对应的种种函数，这样的操作方式无疑非常方便。而当我们尝试添加一个新的函数的时候，则意味着之前的每一类中都要添加相应的函数，相关的数据类非常多时就显得很繁琐了。
可见，如果扩展的主要是函数，那么函数式更方便。如果扩展的主要是变量，那么主要得用OOP。

### 设计模式
在前面的关于double dispatch的描述中，我们已经可以发现，double dispatch所要解决的，便是OOP本身的表达力缺陷。与之类似的，是在设计模式中,visistor pattern也是为了解决这样的问题。在A LIttle Java一书中，两位研究函数式的作者展示了如何从无到有一步步创建visitor pattern的过程。

