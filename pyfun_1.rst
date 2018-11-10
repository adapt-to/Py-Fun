===============
没事就看看吧
===============
 —— 没想好名字，用古诗吧 T-T


序列可迭代的原因：iter函数
=============================

python解释器需要迭代对象时，会自动调用 ``iter(x)``

内置的iter函数有以下作用：
 1. 检查对象是否实现了 *__iter__* 方法，如果实现了就调用它，获取到一个迭代器
 2. 如果对象中没有实现 *__iter__* 方法，但是实现了  *__getitem__* 方法，Python会创建一个迭代器，并尝试按顺序（索引从0开始）获取元素
 3. 如果上述都失败了，通常会返回该对象不可迭代的错误提示

言归正传，之所以序列可迭代的原因，正是由于它们都实现了 *__getitem__* 方法。事实上，标准的序列也都实现了 *__iter__* 方法。
因此如果我们自己实现一个序列对象的话也应该这么做（即在对象中实现 *__iter__* 方法）。

下面简单验证一下上述的内容：

首先打开python的交互终端，输入：

>>> s = str() # 实例化字符串对象
>>> print(dir(s)) # 打印对象中的方法
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', \
'__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', \
'__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize',\
 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', \
 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace',\
  'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> ll = list() # 实例化列表对象
>>> print(dir(ll)) # 打印对象中的方法
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', \
'__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__',\
 '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', \
 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
>>> from array import array # 导入数组类库
>>> print(dir(array)) # 打印类的方法
['__add__', '__class__', '__contains__', '__copy__', '__deepcopy__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',\
 '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', \
 '__reduce__', '__reduce_ex__', '__repr__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'buffer_info', 'byteswap', 'count', \
 'extend', 'frombytes', 'fromfile', 'fromlist', 'fromstring', 'fromunicode', 'index', 'insert', 'itemsize', 'pop', 'remove', 'reverse', 'tobytes', 'tofile', 'tolist', 'tostring', 'tounicode', 'typecode']

从上面3种不同的内置序列中，发现都实现了 *__iter__* 方法和 *__getitem__* 方法。所以它们都是可迭代的，其他的序列类型，有兴趣可以自己尝试一下，
下面为了验证上述中所说的是否仅实现 *__getitem__* 方法也能够迭代，所以自己实现一个序列类型。

实现可迭代序列类型::

    #coding-utf-8

    class Bag():
        def __init__(self, maxsize=10): # 指定背包的默认最大长度
            self.maxsize = maxsize
            self._items = list() # 实例化容器对象，这里使用list

        def __len__(self): # 求背包现有物品长度
            return len(self._items)

        def __getitem__(self, index): 
            return self._items[index]

        def add(self, item):
            if len(self) >= self.maxsize: # add之前判断背包是否物品已满
                raise Exception('Bag is full')
            self._items.append(item)

        def remove(self, item):
            self._items.remove(item)

        def clear(self): # 清除
            self._items.clear()

    # 测试是否可迭代
    bag = Bag()

    for i in range(10):
        bag.add(i)
    
    for item in bag:
        print(item)

    #####################################
    #输出如下 （python版本--python3.6.5）
    #####################################
    G:\python-base>python code_test.py
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9

这里的输出证明了，仅仅实现 *__getitem__* 方法的序列对象也是可迭代对象!

--------------------------------------------------------

Iterable、Iterator、generator的区别
===================================

我也时常忘记这几个概念，或是知道是怎么回事但却并不能够准确直白的阐述出来。
'迭代' 在python中是我们永远避免不了的东西，不管代码里是否有 ``for...in...`` 、``while ...`` 亦或是其他显而易见的循环语句，
我们都在不可避免的使用 '迭代' 这个东西。

简单举个例子:

>>> ll = list(range(4))
>>> ll
[0,1,2,3]
>>> ll.remove(2)
>>> ll
[0,1,3]

这里虽然没有使用显式的循环，不过这里的 ``remove`` 方法能够找到列表元素 ``2`` 实则是通过循环找到的这个元素并将其移除。

**从概念上看这三者的区别：**

 .. note::
  1. 可迭代对象（Iterable）：顾名思义，能够被迭代的对象，python中所有的序列（包括但不限于list、string、dict、set等）都是可迭代对象
  2. 迭代器（Iterator）：自身可以迭代的对象容器，该对象迭代完内部的元素就不能再被迭代使用。可迭代对象之所以可迭代就是因为其背后实现了迭代器
  3. 生成器（generator）：所有生成器都是迭代器，不过生成器更侧重于 **凭空产出**，迭代器侧重于从 **内部拿出**。如果对这两个区别不是很清楚，后面会讲到
 
 
 .. warning:: 什么是迭代：

   **迭代** 是重复反馈过程的活动，其目的通常是为了逼近所需目标或结果。每一次对过程的重复称为一次“迭代”，而每一次迭代得到的结果会作为下一次迭代的初始值。
   重复执行一系列运算步骤，从前面的量依次求出后面的量的过程。此过程的每一次结果，都是由对前一次所得结果施行相同的运算步骤得到的。例如利用迭代法*求某一数学问题的解。
   
   对计算机特定程序中 **需要反复执行的子程序*(一组指令)，进行一次重复，即重复执行程序中的循环，直到满足某条件为止**，亦称为迭代。

   本部分参考自 `[百度百科--迭代] <https://baike.baidu.com/item/%E8%BF%AD%E4%BB%A3/8415523>`_


**从代码实现侧面上看，这三者又有什么区别呢？**

 本文档的另一节有说到为什么序列都可以迭代的原因是在内部实现了 ``__iter__`` 或 ``__getitem__`` 方法。
 但你想没想过这些方法背后的东西，这也正是可迭代对象和迭代器的区别所在。

 首先一言以蔽之，先给出可迭代对象和迭代器代码实现上的差别所在（迭代器和生成器之后再说）：
  1. 可迭代对象是由于内部实现了 ``__iter__`` 或 ``__getitem__`` 方法，并且如果自己实现一个迭代器，更倾向于去实现 ``__iter__`` 方法。
  2. 其实，可迭代对象中的 ``__iter__`` 方法内部实现了迭代器的实例，说直白点就是每调用一次这个函数方法都会生成一个迭代器可供我们迭代使用，
     因为上面说过，迭代器使用一次（这里的一次是指完整的整个迭代过程，或者是已经迭代过几次的）后就不能再迭代出前面已经迭代出的元素了，所以，
     每次想重新迭代，都会重新生成一个迭代器。
     
        >>> list_test = [1,2,3]
        >>> for item in list_test: # 生成迭代器
        ...     print(item)
        ...
        1
        2
        3
        >>> for i in list_test: # 生成迭代器
        ...     print(i)
        ...
        1
        2
        3
        >>>
    
    虽然这两次循环，输出的元素都是 ``list_test`` 中的元素，但是两次 ``for`` 语句执行时都生成了迭代器，并且执行完迭代之后迭代器就被弃用了。
 
  3. 迭代器是由于内部实现了 ``__iter__`` 和 ``__next__`` 方法。为此我做一个简单的测试
        
        >>> a = iter(list_test) # python内置的iter()方法可生成迭代器
        >>> a
        <list_iterator object at 0x0000027A7567FBA8> # a是一个迭代器
        >>> s = str(dir(a)) # 将a中的方法转换为string并给s
        >>> s
        "['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__length_hint__', '__lt__', '__ne__', '__new__', '__next__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook__']"
        >>> if '__iter__' in s and '__next__' in s: # 查看是否在里面
        ...     print('this is Iterator')
        ... else:
        ...     print('this is not Iterator')
        ...
        this is Iterator
        >>>
    
    你也可以用和上面同样的方法去测试可迭代对象，python中的 ``range()`` 方法生成的都是可迭代对象。
    **需要注意的是：** 迭代器中实现的 ``__iter__`` 方法是指向自己而不是像可迭代对象中的那样去生成一个迭代器实例，原因是迭代器本身就是迭代器，所以指向自己有问题吗？


**为什么用迭代器使用一次就不能够再次迭代了**
 ———— 注意这里的 *一次* 是指整个迭代过程

先看例子：
        >>> ll = [0,1,2,3,4]
        >>> ll
        [0, 1, 2, 3, 4]
        >>> ll_iter = iter(ll) 
        >>> ll_iter            # ll_iter是一个迭代器
        <list_iterator object at 0x0000027A7567FE10>
        >>> next(ll_iter)      # 可以使用python内置的next()方法，这里的next()方法会去调用迭代器内部的 __next__方法
        0
        >>> next(ll_iter)
        1
        >>> next(ll_iter)
        2
        >>> next(ll_iter)
        3
        >>> next(ll_iter)
        4

此时将 ``ll`` 对象中的所有元素都打印出来了，如果继续调用 ``next(ll_iter)`` 会发生什么，请看：
        
        >>> next(ll_iter)
        Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
        StopIteration
    
此时如果再调用，将会抛出 ``StopIteration`` 异常，并且之后无论调用多少次 ``next(ll_iter)`` 都会抛出这个异常，
这是提示此迭代器中已经没有元素了。既然没有元素了当然不能再使用啦，记住一点：迭代器的原则就是从里面拿出来的元素不能再拿回去了，不走回头路，除非你再创建一个新的。

.. note::
 
 既然``for`` 语句执行可迭代对象会生成迭代器，那么为什么没有抛出 ``StopIteration`` 异常呢？

 原因是因为在 ``for`` 语句中已经对 ``StopIteration`` 异常进行了异常处理，所以我们在终端并不会看到这个异常。
 那如果不想使用 ``for`` 语句进行迭代的同时也不想看到 ``StopIteration`` 异常要如何实现呢？
 其实做一个异常捕获就可以了，看下面：
    
    >>> s = '123'
    >>> s_iter = iter(s) # 创建一个迭代器
    >>> s_iter
    <str_iterator object at 0x0000027A7567FF28>
    >>> while 1:
    ...     try:
    ...         print(next(s_iter))
    ...     except StopIteration: # 捕获异常
    ...         del s_iter        # 废弃该迭代器
    ...         print('Iterator was deled') # 输出异常捕获后的提示
    ...         break  # 退出循环
    ...
    1
    2
    3
    Iterator was deled

 上述结果能看出，利用 ``while`` 循环遍历了迭代器并捕获了异常。这些步骤在 ``for`` 语句中都已经帮我们完成了。

**实现自己的可迭代对象**

 前面说了，要实现可迭代对象就必须要在对象内的  ``__iter__`` 方法中返回迭代器的实例。

 借用一下前面已经实现的bag的可迭代类型::
    
    #coding-utf-8
    class Bag():
        def __init__(self, maxsize=10): # 指定背包的默认最大长度
            self.maxsize = maxsize
            self._items = list() # 实例化容器对象，这里使用list

        def __len__(self): # 求背包现有物品长度
            return len(self._items)

        def __getitem__(self, index):
            return self._items[index]

        def add(self, item):
            if len(self) >= self.maxsize: # add之前判断背包是否物品已满
                raise Exception('Bag is full')
            self._items.append(item)

        def remove(self, item):
            self._items.remove(item)

        def __iter__(self):
            for item in self._items:
                yield item

        def clear(self): # 清除
            self._items.clear()
 
 上述就是实现了一个可迭代的对象，我们重点关注 ``__iter__`` 和 ``__getitem__`` 方法。这里在 ``__iter__`` 中用到了一个关键字 ``yield``，这个关键字是构建生成器函数的关键字。可以说只要有 ``yield`` 存在的函数\
 就是生成器表达式。

 .. note:: 
  关于 ``yield`` 的解释：

    ``yield`` 是python的关键字，它具有和 ``return`` 类似的功能，但它却又和 ``return`` 具有很大的差别。 
    它们的共同点都是可以返回元素，不同点是 ``return`` 返回后便不会再执行该函数。而 ``yield`` 返回值后，该函数会在返回值的 ``yield`` （如何函数中有多个 ``yield`` 时）处将函数暂停。继续执行函数，
    会从上次暂停的 ``yield`` 处继续向下执行直到遇到下一个 ``yield`` 时再返回它对应的值。比如你看下面这个代码：
        
        >>> def test():
        ...     yield 1
        ...     yield 2
        ...     yield 3
        ...
        >>> s = test()
        >>> s   # s是一个生成器对象
        <generator object test at 0x0000027A754DD1A8>
        >>> next(s)
        1
        >>> next(s)
        2
        >>> next(s)
        3
        >>> next(s)
        Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
        StopIteration

    先创建一个含有 ``yield`` 的函数，此时 ``s`` 就是一个生成器对象，之前说过所有生成器都是迭代器，所以用 ``next()`` 函数打印出函数中的元素，我们就可以发现这里的 ``1、2、3`` 都是逐次打出直到出现 ``StopIteration``，
    这也印证了两点：
     1. 生成器就是迭代器 （反之看如何看待，如果细分概念的话可以说迭代器不一定生成器）
     2. ``yield`` 是生成器关键字，它具有 **暂停返回** 的功能
     3. 任何含有 ``yield`` 的函数都可称为生成器函数 （即产生生成器对象的工厂）

 至此，我们上面自己实现的可迭代对象中的 ``__iter__`` 方法中的内容就可以理解了吧。它返回的就是一个迭代器（生成器）实例。
 
 其实上面实现 ``__iter__`` 方法中的内容也可以更改为： 利用自己实现的一个迭代器类型，在 ``__iter__`` 方法中调用这个迭代器类型产生迭代器实例。

**实现自己的迭代器实例**

 实现迭代器实例前面讲过是要在内部实现 ``__iter__`` 和 ``__next__`` 方法。并且 ``__iter__`` 方法要返回自身。所以自己实现的话可以这样写::

    class MyIterator:

        def __init__(self, items):
            self.items = items
            self.index = 0
        
        def __next__(self):
            try:
                item = self.items[self.index]
            except IndexError:
                raise StopIteration()
            self.index += 1
            return item
        
        def __iter__(self): # 在迭代器类型中这里要返回自身
            return self

 不过python生而就是为了简洁优雅，所以这种写法只做理解，实际中还是直接用 ``yield`` 来的更清爽简洁不是吗？

.. note::
  **什么是生成器表达式？**

  这是除了生成器函数，还有一个叫做生成器表达式的概念，我们都知道列表推导式，看下面：
        
        >>> def test():
        ...     yield 1
        ...     yield 2
        ...     yield 3
        ...
        >>> list_1 = [i for i in test()]  # 列表推导式
        >>> list_1 # 列表推导式能够将内部要迭代的变量一次全拿出来
        [1, 2, 3]
        >>> generator_1 = (item for item in test())  # 生成器表达式，和列表推导式的区别是外面用的括号
        >>> generator_1
        <generator object <genexpr> at 0x0000027A754DD308>
        >>> next(generator_1) 
        1
        >>> next(generator_1)
        2
        >>> next(generator_1)
        3
        >>> next(generator_1)
        Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
        StopIteration

  生成器表达式可以将某些比较简单的生成器函数用一句话表示出来，并且不需要加入 ``yield``，你看上面那个是不是就没有关键字 ``yield``。其实这样好也不好，好处是看着简洁，不好的地方是其他人不一定知道那是一个生成器。


    

           