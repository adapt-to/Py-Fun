===========
Pandas
===========
 —— 像是'字典形式'的numpy

如果说把numpy比作列表的话，那么pandas就像是字典。因为pandas可以给不同的行、不同的列进行命名

pandas一般搭配着numpy一起运用，两者可以很好地相得益彰。

带标签的'一维数组'Series
========================
 ——带有标签的一维数组，可以保存任何数据类型，轴标签统称为索引

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
 —— 行和列都有标签的多维数组

