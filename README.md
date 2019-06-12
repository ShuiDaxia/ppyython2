# ppyython2
python学习提炼（基于《python学习手册》第四版---起始于第四部分函数）
==========

（md之前写了好长一段手贱碰到关机键了。。。）

函数
---

原话：函数是python为了代码最大程度地重用和最小化代码冗余而提供的最基本的程序结构。（其实这句话套用到其他语言上也一样~）

def----这个经常见到的函数写法，我还是喜欢叫方法，通过这个语句，python创建一个新的对象并将函数名指向对象，就和赋值一样，函数名变成了函数的引用。

lambda----这个在java，kotlin中也都有，没细说，我还是直接习惯叫匿名方法，它可以一次性地创建函数对象，并将其作为结果返回。


def
---

python在程序运行前并不需要全部定义，因为他是引用，所以在程序运行之前已经清楚地记录在内存之中，所要做的就是当执行到它是，将变量名指向对象即可。

最简单的定义和调用：

>>>def times(x,y)

...return(x,y)

>>>times(2,5)...10

这很容易理解，也和其他语言大同小异，一点简单的代码，组成了def最简单的定义和调用。

如果这时我把x，y的参数换成'abc'和3，那么结果是'abcabcabc'，也可以完成字符串的重复计算，这就是python中的多态，根据具体的数据类型自动判断，这个感觉和java中多态有些类似也又不完全是一个概念，java中的多态是子类指向对父类的引用，既可以使用父类的方法，也可以重写加入自己的改变，反正都讲究一个灵活。

换句话说，就如上边times这个方法，只要xy的参数类型符合执行乘法的逻辑，只要是兼容的任何类型数据都可以进行运算，比如列表。

原话：大体上讲，我们在python中为对象编写接口，而不是数据类型。

当然，这种多态的编程模型意味着必须测试代码去检查错误，而不是一开始提供编译器用来为我们检测类型错误的类型声明。

作用域
---

作用域部分值得学习的地方很多，但是记录下来就和抄课文没啥区别了，多读几遍原话就可以。

值得注意的地方，不要轻易给内置函数赋值或者更改。

>>>def XXX():

>>>----open = 'abcd'

>>>----open('asdf.py')

这样给open赋值后，想用open函数打开文件是不可能的，更尴尬的是它不会报错（虽然我在微笑。。。）

在代码不熟悉的情况下少用全局变量，通过全局变量在各个部分的使用会让代码变得更加耦合，使程序变得复杂和难以理解，甚至引发未知的buuuuuug。

还有需要注意的一点是模块的引用，当被引用的模块中定义了某个变量，被当前模块中的代码引用时，被导入的模块就变成了当前对象的一个属性，获得了其访问权，那么在我就可以给被引用模块中的全局变量做赋值修改，因为python没有严格意义上的私有变量，所以在理论上是可以修改的，那么修改之后同样会引发模块和模块之间的冲突和未知的碧优姬。当然以后还会有专门模块之间的通信，在此不表。

原话：虽然我们无法避免修改文件间的变量，但通常的做法是最小化文件间变量的修改。

嵌套作用域
---

这里举了两个例子：

X = 99

def f1():

....X = 88

....def f2():

........print(X)

....f2()

之后执行f1()，最后打印出来是88，这个例子好理解，声明全局变量X，而在f1内部也生命了局部变量X，f2是只有在执行f1之后才创建的，它会自动去优先寻找局部变量X = 88。此种情况下f2只是临时变量，仅仅在f1内部执行中存在。

重点是第二个例子：

def f1():

....X = 88

....def f2():

........print(X)

....return f2()

action = f1()

最后执行action，可以看到，第二个例子中，当返回f2（）这个对象的时候，f1（）早已经没用了，原话是不在激活状态了，但是f2（）依然可以找到f1（）的映射，也是有效的，这个例子很关键，在此插眼标记一下，这里的关键不在于为什么只是返回了f2没有执行却依然可以打印出88，而是因为在返回了f2后，f1已经无用的情况下，f2依然可以自动映射到f1中的X变量。（说的有点绕，我其实也似懂非懂，不过大概意思是知道的，以后回头再看一下）。

现在给自己整理一下到此为止的两个重要的点，回头需要仔细翻看的，一个就是之前的迭代器部分，包括迭代协议，另一个就是目前的作用域部分。
---

工厂函数：这是对上一部分的解释和延伸

工厂函数-----一个能够记住嵌套值作用域的变量值的函数，尽管那个作用域或许已经不存在了。

举个栗子：

def maker(N):

....def action(X):

........return X ** N

....return action

函数action返回计算结果，maker函数仅仅返回了action对象，如果我们单纯执行f = maker(2)，那么得到的只是一个对象，并没有结果，但是此时这个2的值已经被记录了，之后我们再执行比如f(2)，那么就可以得到2的平方 4了。

























