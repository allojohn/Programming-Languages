dynamic dispatch 是OOP的核心之一。因为OOP通过方法传递信息，如果不同类之间传递信息，那么，就需要分析传入的对象，如何分析？需要给这个对象传递信息。然而因为是在原有的类的对象下操作，所以实际上改变了问题的域。例如，A类对象a1，尝试操作B类对象b2,那么，就应该是a1.x b  有可能是a1.x c1 ,有可能是a1.x.d1 ······那么如何去判断他们的类呢？那就是is_B? a1 ,is_C? b1, is_D? b1?
这样，本来在A类对象下的操作，转移到了其他类的对象。这就是所谓的dynamic dispatch。它改变了调用的类。
那dinamic dispatch 是什么意思呢？就是先转至另一个类，但是另一个在使用的时候，又需要调用原有类的方法，所以被称为dynamic dispatch。
And what we wanna do ,is more,what is multiple methods?