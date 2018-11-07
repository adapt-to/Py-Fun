===============
春风得意马蹄疾
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