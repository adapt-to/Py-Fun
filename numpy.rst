=================
科学计算库-NumPy
=================
 ————开源的数值编程工具

* 强大的N维数组对象：``ndarray``
* 对数组结构数据进行运算
* 随机数、线性代数、傅里叶变换等

.. note::
  ``numpy`` 提供了两种基本的对象：``ndarray``（N-dimensional array object）和 ``ufunc``（universal function object）。
  ``ndarray`` (下文统一称之为数组)是存储单一数据类型的多维数组，而 ``ufunc`` 则是能够对数组进行处理的函数！

  高级工具 ``Pandas`` 也是基于 ``numpy`` 来构建的

NumPy介绍
=====================

    numpy构建的数组是一个多维的数组对象，由两部分构成：
    1. 实际的数据
    2. 描述这些数据的元数据

    >>> import numpy as np  # 导入numpy并命别名为np，这一般是约定俗称的
    >>> arr = np.array([[1,2,3,4,5],[0,3,2,5,2]])  #array()函数的括号内可以是 列表、元组、数组等可迭代对象
    >>> print(arr) # 输出中没有逗号
    [[1 2 3 4 5]
     [0 3 2 5 2]]
    >>> print(arr.ndim) # 求数组的维度的个数，线代中称为'秩',rank 
    2
    >>> print(arr.shape) # 求数组的具体维度（几行几列）返回的形式是tuple。如果返回的结果只有一个元素，那么就是一维数组
    (2, 5)
    >>> print(arr.size) # 数组中元素的总个数
    10
    >>> print(arr.dtype) # data-type:指数组中数值的类型。记住是数值的类型
    int32
    >>> print(arr.itemsize) # 数组中每个元素的字节大小。 int32在内存中占4个字节
    4
    >>> print(arr.data) # 实际包含数组元素的地址位置
    <memory at 0x000001EB63459120>
    >>>

    创建数组：array()函数的括号内可以是 列表、元组、数组可迭代对象等

    >>> import numpy as np
    >>> ar1 = np.array(range(5))  # 利用可迭代对象创建数组
    >>> print(ar1)
    [0 1 2 3 4]
    >>> ar2 = np.array([1, 2, 3.1, 4.5, 5.6])    #浮点型
    >>> print(ar2) # 注意看输出形式，输入中有float和int，结果全为float，这个叫向上统一
    [1.  2.  3.1 4.5 5.6] 
    >>> ar3 = np.array([[1,2,3],['a','b','c']])
    >>> print(ar3) # 向上合并，因为ar3中含有字符型，所以数值型也转换为字符型。因为数组中要求元素类型一致
    [['1' '2' '3']
     ['a' 'b' 'c']]
    >>> print(ar3.dtype)
    <U11  # 这里的 U11 就是unicode 表示字符型
    >>>
    
