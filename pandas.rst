===========
Pandas
===========

一维数组 Series
=================
 ——带有标签的一维数组，可以保存任何数据类型，轴标签统称为索引

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

 创建Series数组时给定index

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