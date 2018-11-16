===========
Pandas基础
===========
 —— 像是'字典形式'的numpy

如果说把numpy比作列表的话，那么pandas就像是字典。因为pandas可以给不同的行、不同的列进行命名

pandas一般搭配着numpy一起运用，两者可以很好地相得益彰。

Series
========================
 ——带标签的“一维数组”，可以保存任何数据类型

如何创建Series
-----------------
 
 1. 直接创建：
    
    >>> import pandas as pd
    >>> import numpy as np
    >>> s = pd.Series([1,3,6,3,np.nan,8]) # 注意首字母大写；np.nan代指空，即None
    >>> print(s) # 结果中自动加上了index，即标签，默认从0开始，同时给出了数据的dtype，默认float
    0    1.0
    1    3.0
    2    6.0
    3    3.0
    4    NaN
    5    8.0
    dtype: float64

 2. 利用np.array数组创建Series，具体如下：

    >>> import numpy as np
    >>> import pandas as pd # 导入模块
    >>> 
    >>> ar = np.random.rand(5)
    >>> print(ar)
    [0.0326399  0.79344924 0.36188808 0.11620241 0.39077197]
    >>> 
    >>> s = pd.Series(ar)
    >>> print(s) # 输出s的标签 value 以及 dtype类型
    0    0.032640
    1    0.793449
    2    0.361888
    3    0.116202
    4    0.390772
    dtype: float64
    >>> 
    >>> print(type(s))
    <class 'pandas.core.series.Series'>
    >>> print(s.index) # 标签
    RangeIndex(start=0, stop=5, step=1)
    >>> print(s.values) # 显示s的所有value值,别忘了value后面的s
    [0.0326399  0.79344924 0.36188808 0.11620241 0.39077197]

 3. 直接创建的基础上给定index，如下：

    >>> s = pd.Series([99,95,79],index = ['英语','数学','语文'])
    >>> print(s)
    英语    99
    数学    95
    语文    79
    dtype: int64
    >>> print(s.index)
    Index(['英语', '数学', '语文'], dtype='object')
    >>> print(s.loc['数学']) # 以标签来索引
    95
    >>> print(s.iloc[1]) # 以标签的角标索引，索引从0开始
    95

 4. 利用字典创建

    >>> dic = {'语文':90 , '数学':94, '英语':92}
    >>> s = pd.Series(dic)
    >>> print(s)
    语文    90
    数学    94
    英语    92
    dtype: int64

DataFrame
========================
 —— 行和列都有标签的二维数组

如何创建DtaFrame
-----------------

1. 先创建行，列标签，再创建DataFrame

    >>> import pandas as pd
    >>> import numpy as np
    >>> dates = pd.date_range('20181116',periods=6) # 创建行标签，6个日期
    >>> print(dates)
    DatetimeIndex(['2018-11-16', '2018-11-17', '2018-11-18', '2018-11-19',
                   '2018-11-20', '2018-11-21'],
                  dtype='datetime64[ns]', freq='D')
    >>> df = pd.DataFrame(np.random.randn(6,3), index=dates, columns=['aa','bb','cc']) # 创建DataFrame
    >>> print(df)
                      aa        bb        cc
    2018-11-16 -0.829960 -0.883867 -0.257769
    2018-11-17 -1.274060  0.209015 -0.003057
    2018-11-18 -0.515389  1.180768 -1.872595
    2018-11-19  0.794865 -1.007322  0.809777
    2018-11-20 -0.629912 -0.497483 -0.022165
    2018-11-21 -0.145094  0.011507  1.552536

 .. note::
  由上述可以看到，我们在创建DataFrame前先创建了dates(即行的标签)，然后创建DataFrame，给入的数据为6行3列满足正态分布的随机二维数组，同时\
  将dates赋给DataFrame的index，这里的index和Series中的index类似。
  还给定了DataFrame的columns，即每列的标签，这里的columns还可以写作元组类型。
  最后的结果如上述所示，为一个具有日期的行标签以及字母列标签的二维数组。

2. 不给定行列标签直接创建DataFrame

    >>> df1 = pd.DataFrame(np.arange(12).reshape((3,4)))
    >>> print(df1)
       0  1   2   3
    0  0  1   2   3
    1  4  5   6   7
    2  8  9  10  11

 .. warning::
  结果如上代码所示，在创建DtaFrame时只给定数据并不给定行列标签，则会默认从0开始给定标签。这一点和Series也是类似的。

3. 利用字典创建DataFrame

    >>> df2 = pd.DataFrame({'A':1.2,
                        'B':pd.Timestamp('20181116'),
                        'C':pd.Series(np.arange(4)),
                        'D':np.array([2]*4,dtype='int32'),
                        'E':pd.Categorical(['car', 'airport', 'ship', 'test']),
                        'F':'Hello'
    })
    >>> print(df2)
         A          B  C  D        E      F
    0  1.2 2018-11-16  0  2      car  Hello
    1  1.2 2018-11-16  1  2  airport  Hello
    2  1.2 2018-11-16  2  2     ship  Hello
    3  1.2 2018-11-16  3  2     test  Hello
    >>>

 .. note::
  上述的字典也可以先写好，然后创建时再传入。
  
  这里需要 **注意** 的地方是：
   * 字典的key是DataFrame的列标签，而不是行标签。这里的行标签没有给出则默认从0开始。

如何拆分DtaFrame
-----------------

 面对一个DataFrame，我们要明确它表示的到底是什么，行标签、列标签还有它的内容是什么。所以首先要拆。

 1. 获得DataFrame的行标签：df.index
 2. 获得DataFrame的列标签: df.columns
 3. 获得DataFrame的数据  : df.values
 4. 获得DataFrame每列数据的dtype：df.dtypes
 5. 其中df表示一个DataFrame对象

 举例如下：

    >>> df2 = pd.DataFrame({'A':1.2,
                        'B':pd.Timestamp('20181116'),
                        'C':pd.Series(np.arange(4)),
                        'D':np.array([2]*4,dtype='int32'),
                        'E':pd.Categorical(['car', 'airport', 'ship', 'test']),
                        'F':'Hello'
    })
    >>> print(df2)
         A          B  C  D        E      F
    0  1.2 2018-11-16  0  2      car  Hello
    1  1.2 2018-11-16  1  2  airport  Hello
    2  1.2 2018-11-16  2  2     ship  Hello
    3  1.2 2018-11-16  3  2     test  Hello
    >>>
    >>> print(df2.dtypes) # 获取每一列的数据类型
    A           float64
    B    datetime64[ns]
    C             int64
    D             int32
    E          category
    F            object
    dtype: object
    >>> 
    >>> print(df2.index) # 获取行标签index
    RangeIndex(start=0, stop=4, step=1)
    >>>
    >>> print(df2.columns) # 获取列标签columns
    Index(['A', 'B', 'C', 'D', 'E', 'F'], dtype='object')
    >>>
    >>> print(df2.values) # 输出DataFrame的values，输出格式为np的array对象
    [[1.2 Timestamp('2018-11-16 00:00:00') 0 2 'car' 'Hello']
     [1.2 Timestamp('2018-11-16 00:00:00') 1 2 'airport' 'Hello']
     [1.2 Timestamp('2018-11-16 00:00:00') 2 2 'ship' 'Hello']
     [1.2 Timestamp('2018-11-16 00:00:00') 3 2 'test' 'Hello']]

如何获取DataFrame的常用数学结果
-----------------

利用 ``DataFrame`` 的 ``describle()`` 方法可以获得数据的平均值、方差、标准差等一系列的结果
  
  接上例：
   
    >>> df2 = pd.DataFrame({'A':1.2,
                        'B':pd.Timestamp('20181116'),
                        'C':pd.Series(np.arange(4)),
                        'D':np.array([2]*4,dtype='int32'),
                        'E':pd.Categorical(['car', 'airport', 'ship', 'test']),
                        'F':'Hello'
    })
    >>> print(df2.describe())
             A         C    D
    count  4.0  4.000000  4.0
    mean   1.2  1.500000  2.0
    std    0.0  1.290994  0.0
    min    1.2  0.000000  2.0
    25%    1.2  0.750000  2.0
    50%    1.2  1.500000  2.0
    75%    1.2  2.250000  2.0
    max    1.2  3.000000  2.0

 .. note:;
  这里为什么只有 `` 'A'、'C'、'D' `` 三列有结果，是因为这三列的数据类型为 **数值** ，可以参考上面的代码。
  上面代码结果中每行所表示的意思如下：
   1. count ： 表示该列有多少个数据
   2. mean ： 表示所有数据的平均值
   3. std : 表示所有数据的标准差
   4. min : 表示所有数据中的最小值
   5. 25% : 表示在最小值与最大值之间的25%处的数值大小
   6. 50% : 表示在最小值与最大值之间的50%处的数值大小，即为中位数
   7. 75% : 表示在最小值与最大值之间的75%处的数值大小
   8. max : 表示所有数据中的最大值
   