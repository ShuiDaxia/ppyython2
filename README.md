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
---

工厂函数-----一个能够记住嵌套值作用域的变量值的函数，尽管那个作用域或许已经不存在了。

举个栗子：

def maker(N):

....def action(X):

........return X ** N

....return action

函数action返回计算结果，maker函数仅仅返回了action对象，如果我们单纯执行f = maker(2)，那么得到的只是一个对象，并没有结果，但是此时这个2的值已经被记录了，之后我们再执行比如f(2)，那么就可以得到2的平方 4了。

这个例子很直观地将这部分主题表达出来，就是我想什么时候用就什么时候用，当我只输入2的时候，只有内嵌函数记住了2，当返回action时，f被maker赋值后就不在了，但这个2却被结结实实记录下来了。

一看到工厂函数，不自觉会想到java中的工厂模式，其实它们在思想上是有一定的相似性的。

一大波原话：

这是一种相当高级的技术（指上边的嵌套），除了那些拥有函数式编程背景的程序猿们，以后在实际应用中也不会常常见到（fxxk，那我费了老半天劲看是为了森么）。
另一方面，嵌套的作用域常常被lambda函数创建表达式使用---因为它是表达式，它们几乎总是嵌套在一个def中。此外，函数嵌套通常用作装饰器（这个我知道是一个重点）---在某些情况下，它是最为合理的编码模式。

通常来说：类（class）是一个更好的像这样进行“记忆”的选择，因为它们让状态变得很明确。不实用类的话，全局变量，像这样的嵌套作用域引用以及默认的参数就是Python的函数能够保留状态信息的主要方法了。

嗯，感觉目前位置嵌套作用域的作用也是主要作为一种保存数据信息的方法。

以上的例子为了说明作用域其实有些强行的感觉，正常来说作者还是推荐所谓的'程序员式方法'：

def f1(x,y):

....m = x,y

....f2()

def f2(a)

....print(a)

f1(2,5)>>>>10

这才是程序员思维(滑稽)

lambda
---

匿名函数使得之前的嵌套作用域变得更加简单

def fun():

....x = 4

....action = (lambda n : x ** n)

....return action

x = fun()

print(x(2))

16

fun中返回action的对象，所以x只需要接收原lambda嵌套中的n的变量值就可以了，有了之前的学习，这一步就很好理解了。

嵌套于循环中的陷阱
---

书中举了这样一个很有意思的栗子来说明嵌套中的陷阱，我们在学习完嵌套和lambda的时候也许会想有这种操作：

def fun():

....act = []

....for i in range(5):

........act.append(lambda x : i ** x)

....return act

acts = fun()

你可能会想到acts[0](2)、acts[1](2)、acts[2](2)、acts[3](2)会分别等于0，2，4，9，但实际上都是16，因为嵌套作用域中的变量在嵌套函数被调用时才进行查找，也就是每次都循环到最后一个值时才记录。

想得到正确的答案也很简单，在lambda处这样写：lamba x ,i = i : i ** x，这样每次迭代都会赋予i新的值。

官方解释：这是在嵌套作用域的值和默认参数方面遗留的一种仍需解释清楚的情况，而不是引用所在的嵌套作用域的值。也就是说，为了能让这类代码能够工作，必须使用默认参数把当前的值传递给嵌套作用域的变量。因为默认参数是在创建变量时评估的，而不是在稍后的调用时，每一个函数记住了自己的变量i的值。

nonlocal
---

作用：使得嵌套的函数能够提供可写的状态信息，以便在随后调用嵌套的函数的时候能够记住这些信息。允许状态修改，这使得嵌套函数更有用。

这一部分的书很绕口，读起来也懵逼，我个人觉得首先要知道global和nonlocal的作用就是在某种程度上来限制一般的查找规则。

global原话：使得作用域查找从嵌套的模块的作用域开始，并且允许对那里的名称赋值。如果名称不存在于该模块中，作用域查找继续到内置作用域，但是，对全局名称的赋值总是在模块的作用域中创建或修改它们。

nonlocal原话：限制作用域查找只是嵌套的def，要求名称已经存在于那里，并且允许对它们赋值。作用域查找不会继续到全局或内置作用域。

书中比较直观的例子，可以清楚得看到nonlocal对变量做了什么：（通常情况下，嵌套作用域中的变量是不允许修改的，而加上nonlocal后情况就变了）

def tester(start):

....state = start

....def nested(label):

........nonlocal state

........print(label,state)

........state += 1

....return nested

当F = tester(0)

那么F('abc')>>>abc...0

继续F('bcd')>>>bcd...1

...

使用该嵌套作用域引用时，多次调用上边的tester，就会获得tester(0)的多个副本，每次返回nested的时候都会产生新的state值，state这个值每次是会记录的，当我们创建tester(1)的时候，就又会获得新的tester(1)的副本。

这里切记用nonlocal的变量一定要先被赋值。

那这个nonlocal到底特么有啥用？

原话：假设有了极其复杂的嵌套函数，你会为一团糟乱而感到吃惊。尽管很难在我们的小示例中看到这点，但在很多程序中，状态信息变得很重要。在Python中，有各种不同的方法来‘记住’跨函数和方法信息。尽管有利有弊，对于嵌套的作用域引用，nonlocal确实起到了改进作用---nonlocal语句允许在内存中保持可变状态的多个副本，并且解决了在类无法保证的情况下的简单的状态保持。


参数传递
---

参数传递一定要记住最早最早学的，变量只是对象的引用，变量是变量，对象是对象。还需注意可变参数的对象引用不要轻易改变，容易引发一系列错误。

参数传递时候注意别搞混了一点，name = value，这个写法在不同地方意义是不一样的：

比如def f1(a,b,c)，如果我写成def f1(a,b = 1, c = 3) 那么代表的意义是b，c是有默认参数的，如果实在函数调用过程中f1(a = 2,c = 3,b = 1)那么这样是进行参数匹配的，这两种写法形式是一样的但是注意一个是在调用中，一个是在函数头部，意义是不一样的。

所以参数传递部分建议反复阅读一下，虽然理解难度上与之前嵌套循环相对容易，但每一段话都非常重要。

同样注意当* 号和** 号当用在函数头部的时候，是用来接收多个参数和关键字参数的，而当它们出现在函数调用的时候，是用来解包的。曾经看过其他的一些资料，这里都是一笔带过了，没有细节，当时很懵逼，这里需要特别注意:

def func(a,b,c,d):print(a,b,c,d)

args = (1,2,3,4)

func(*args)

1 2 3 4

感觉就像是倒放。

关键字参数keyword-only：def func(a,* b,c)，如果这样写，参数c必须接收关键字参数传递，即例如func(1,2,c = 3)

又或者这样def func(a, * ,b,c),使用的时候要这样写func(1,b = 2,c = 3)。

再或者这样，加入默认值def func(a,* ,b = 'cxk',c = 'rap')，使用的时候就这样func(1)完事了，就可以自动唱跳rap。。。


****************** （又是一个崭新的周一而且并不知道到底是学习第几天的  学习第十二天）
---

通过一个排序练习来说明来实际感受参数匹配：假想编写一个函数，这个函数可以计算任意参数集合和任意对象数据类型集合中的最小值。

看看三种例子：

1

def func1(* args):

....res = args[0]

....for arg in args[1:]:

........if arg < res:

............res = arg

....return res

第一种方法用到了之前的切片，接收args不定参数后，先取出索引0的值，然后通过for让此值不断迭代比较其它的元素，并用最小的值重新对res进行赋值。

2

def func2(first,* args):

....res = first

....for arg in args:

........if arg < res:

............res = arg

....return res

第二种方法接收两个参数，这样就可以省略用切片将第一个元素分离的操作了，在形式和理解上更加简单。

3

def func3(* args):

....tmp = list(args)

....tmp.sort()

....return tmp[0]

第三种方法更加简单暴力，利用了内置的list和sort函数，将接收到的参数转为列表并排序，最后返回索引0，即最小值。熟悉这些函数的话，这个是最简单的，而且这种在排序的运算速度上远超其它两种。。不过我个人感觉就阅读性上来说，第二种是非常合适了，我第一个能想到的也类似第二种方法。

更深入一点的，我们将第二种方法进行改造并加入切片，把第一个参数传入另外的比较函数：

def minmax(test,* args):

....res = args[0]

....for arg in args[1:]:

........if test(res,arg):

............res = arg

....return res

def min1(x,y):return x > y

print(minmax(min1,[1,2,3,4,5]))

这就是函数的多态和参数的灵活性的应用，当我们需要求max的时候，仅需要改动min函数就可以了，原来的minmax函数可以保持完整性。

其实到这里通过上边这里例子我想到了map这个函数，有一丝丝的相似性，看看之后会不会提到。

接着循序渐进，来看更为有实际真实的例子：如果我们要求任意序列的公共部分或者出现过的所有元素，即交集和并集。

交集：

def intersect(* args):

....res = []

....for x in args[0]:

........for other in args[1:]:

............if x not in other: break

........else:

............res.append(x)

....return res

并集

def union(* args):

....res = []

....for seq in args:

........for x in seq:

............if x not in res:

................res.append(x)

....return res

细细品味....


函数的高级话题：
---
递归、属性、注解、lambda、map、filter等等（我有点慌了）

函数设计概念(一般原则)：

耦合性：对于输入使用参数并且对于输出使用return语句。

.......只有在真正必要的情况下使用全局变量。

.......不要改变可变类型的参数，除非调用者希望这样做。

聚合性：每一个函数都应该有一个单一的、统一的目标。（很哲♂学的说法，说白了就是程序中每个函数实现一个小目标，但所有的函数所实现的所有功能又是为整个程序服务的）

大小：每一个函数都应该相对较小（说谁小呢？！）

耦合：避免直接改变在另一个模块文件中的变量。

递归
---

一看到这个词我其实是很虚的，因为其它语言也都有这个，而且确实是高级技巧，看过各种语言的教程，每到这一部分直接就是斐波那契数列，要么什么杨辉三角之类的直接糊脸，看不懂也一脸萌哔，看看用这本书怎么样吧。

def mysum(L):

....if not L:

........return 0

....else: return L[0] + mysum(L[1:])

mysum([1,2,3,4,5])

15

enmmmm，还阔以，能看懂，但是我想学会自己理解并写。

当然咱要是会三元if/else语句，两行搞定：def mysum(L):>>>return 0 if not L else L[0] + mysum(L[1:])


****************** （突然有事不能学习但不想断更假装提交一下的  学习第十二天）
---

for循环之于递归
---

这个例子我看了很久，看起来很简单，但实际理解起来需要熟悉for循环的工作流程和递归的工作流程，细细品味这个例子，很关键：

1....def sumtree(L):

2........tot = 0

3........for x in L:

4............if not isinstance(x,list):

5................tot += x

6............else:

7................tot += sumtree(x)

8........return tot

L = [1,[2,[3,4],5],6,[7,8]]

print(sumtree(L))

36...end

强烈建议这个例子用pycharm等ide来执行，可以打断点，可以一步一步感受工作流程。

首先进入循环后，L中的第一个值是1，那么肯定走if，tot变为1，继续走第二个解析到[2,[3,4],5]，是列表，肯定走else了，但走完else后，如果你打了断点，可以发现tot这个值归零了，我当时也是有点懵逼的，后来想了一下，想到了递归是有副本的，这里tot之所以归零，是因为python创建了[2,[3,4],5]这个副本，原来第一次循环的tot的值应该依然存在的，我们往后继续走；果然，当进行完[3,4]这个步骤之后，系统执行了一次return，返回了tot=7这个值，再之后分别与之前的2，和5两个值相加，然后执行return返回tot = 14，之后与1相加，接着往下进行。。。

后来我又在if和else中添加了打印日志，用断点的方法重走了一遍，发现在细节上与之前还是有区别，妹的，不管了，反正走这么一遍对迭代有了更清晰的认识。

函数注解和属性一脸懵逼，讲的也不是很详细，暂时略过了
---






























