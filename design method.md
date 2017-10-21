### 间接原则

#### 面向接口编程

OOP领域似乎经常强调这一点，但是这其实并不是OOP的特色。接口的意义，在于它作为一个间接层，联系了用户与实现者。用户不需要也不应该直接去接触这个地方，而是通过接口去访问。使用者因此不需要了解那么多有关实现的细节，并且因为不明白细节，系统变得更安全了。如Unix的内核对用户提供专门的操作指令，但是并不让你去直接修改内核。

如果我们将这一原则迁移到其他领域，同样合适，例如金钱。金钱的产生，使得**原有的物物交换**不再显得那么必要，金钱提供了一个间接层，充当物物交换的媒介。直觉上看，添加了一个间接层，似乎会拖累整个系统，但是实际上间接层的出现实际上是有利的。货币的流通**大大加快了物物交换的速度**。同样的，当我们来面对编程的世界中，两个沟通的层次是人与机器，机器语言、汇编语言、高级语言等的出现，都可以看作是间接层，它们最终都不断便利着人对机器的操控。

![](/assets/interface.jpg)

> Any problem in computer science can be solved with another layer of indirection

上面是业内流传的一句经典的话，任何问题都可以通过添加一个抽象层来解决。不过，据说原作者实际上还添了半句话：

> Except for the problem of too many layers of indirection.

也就是说，当添加间接层试图解决问题的时候，间接层本身成了问题。

### 抽象必漏

Joel Spolsky曾经在自己的博客中谈到了一个话题：[The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)。意思说，虽然我们一直在试图使用抽象试图隐藏底层的细节，但是这往往在只是柏拉图式的幻想，并不存在这种完完全全的理想化。抽象并不完美。

1. 如果需要聘用程序员来做VB编程，只选择会VB的人是不够的，因为当VB这层抽象出了问题，他们就会束手无策。
 2.  会使用号称能大大提高编码效率的代码生成工具也是不够的，还要自己先动手编写代码。
3. 开车不能开太快。 虽然雨刮器、车前灯等工具试图屏蔽天气的影响，但是天气极其恶劣的时候仍然要放慢速度。也就是说，天气的影响其实并没有完全被屏蔽掉。

在这些情况下，程序员便需要充当「管道工」，深入更底层去解决问题。也就是说，底层的知识，仍然不可或缺。
在其他领域，同样可以见到这种抽象失败的情况。经济学领域的「理性人假设」便是一个不完美的抽象，古典主义认为人是完全的理性的，会仔细合理地考虑自己在经济活动中的选择。这是美丽的假设，实际上很多时候我们都很不理性，而后来的「行为经济学」流派是基于理性人假设，探讨人在经济活动中的不理性行为。
