> I’m sorry that I long ago coined the term “objects”  because it gets many people to focus on the lesser idea.The big idea is “messaging”.
--ALan Kay
## double dispatch 与消息传递
谈及面向对象，我们总是想到对象的特点：继承、多态、封装，但是却会忽略另一个重要的内容：消息传递。double dispatch的实现，则凸显了消息传递特点。


例如，如果我们可能给一个OOP对象A传入不同类型的对象，那么，在其中的函数中，我们很直观地想到，应该先做一个类型判断，用来识别到底是哪种类型的对象，然后再做相应的操作。但是，我们实际上还有一种更加优雅、更加OOP的解法：反过来，将自身传递给对象，让对方来处理！这样我们便省却了冗余的判断类型操作。
```
class Int
  def add_values v
    if v.is_a? Int
	  Int.new(v.i+i)
	elseif v.is_a? String
	  Mystring.new(v.s + i.to_s)
  end
end
```

double dispatch后：
```
class Int
  def add_values v
    v.addInt(self)
  
  def addInt v
    ...
  def addString v
    ...
```
上述的代码只是一个非常简单的叙述。原先，add_value需要判断传入的对象v，它的类型是Int还是String。不过我们完全可以直接在在String 与Int对象中均实现一个addInt操作，那么，我们就可以直接返回一个addInt(self)操作。

#### 接口隐喻
我们看到，要实现上述的double dispatch，我们实际上在不同对象中创建了**接口**：只有当传入的参数对象含有`addInt()`时，这样的传递才能实现。