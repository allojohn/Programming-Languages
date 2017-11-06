### mixin
Ruby中mixin用 `module `来表示。Ruby是完全的OOP，而mixin则脱离了这样的束缚，它可以被不同的类使用。并且mixin的实现中，可以包含类的方法。而mixin的多态方式与之前介绍的method-lookup rules又有所不同。mixin中涉及的方法，并不会直接在类被调用。方法首先在self指向的类中被查找，其次才是mixin，之后是self的父类。举例如下：

```
module Doubler    # define mixin
  def double
    self + self # uses self's + message, not defined in Doubler
  end
end
##############################
class Pt
  attr_accessor :x, :y
  include Doubler
  def + other
    ans = Pt.new
    ans.x = self.x + other.x
    ans.y = self.y + other.y
    ans
  end
  def double    # deine double in class,and the method of the same name defined in mixin won't be called .
    ans = Pt.new
    ans.x = 10 * self.x
    ans.y = 10 * self.y
    ans
  end
end
###############################
class String
  include Doubler
end
```
在上述例子中，如果新建了一个Pt类sample，则当试图调用double方法时，程序不会直接去调用mixin中调用的方法，而是现在类中寻找。类似的，如果调用了mixin的方法，则现在类中寻找方法`+`。

### interface
mixin是动态类型OOP的特点，在其中定义的一系列函数都没有指定类型。在静态语言中与之相对的，则是interface。interface不是类，不能创建interface的对象,它被其他类调用。使用时使用`implements`关键字。

### abstract class
与interface相似，当使用abstract class关键字时，不能创建该类的对象。但这个类可以被继承，子类可以正常创建对象。abstract class中常常定义了一系列抽象函数（使用abstract 关键字），这些函数的定义不是为了被使用，而是为了被重载。