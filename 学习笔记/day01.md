# 1、常量

主要关于几个numpy中常量的定义

## 1.1 numpy.nan

`numpy.nan `，其中 `nan = NaN = NAN`

注意点：`numpy.nan != numpy.nan`

## 1.2 numpy.inf

`numpy.inf`，其中 `Inf = inf = infty = Infinity = PINF`

## 1.3 numpy.pi

`numpy.pi`表示圆周率pi

## 1.4 numpy.e

`numpy.e`表示自然常数e



# 2、基本的数据类型

## 2.1 常见的数据类型

归纳比较常用的几种数据类型：

bool_ = bool8

int8 = byte

int16 = short

int32 = intc

int_ = int64 = long = int0 = intp

float_ = float64 = double

str_ = unicode_ = str0 = unicode

还有两个表示时间的类型

datetime64 

timedelta64

## 2.2 创建数据类型

每个类型都有对应的字符代码。

比如boolean ---> 'b1'

实例：

`import numpy as np `

`a = np.dtype('b1')`

`print(a.type)`

`print(a.itemsize)`



## 2.3 数据类型信息

比如数据的范围

整数的类型使用iinfo函数。

实例：

`import numpy as np`

`ii16 = np.iinfo(np.int16)`

`print(ii16.min)`

`print(ii16.max)`

答案为：

`-32768`

`32767`



浮点数数的类型使用finfo函数。

实例：

`import numpy as np`

`ff16 = np.iinfo(np.float16)`

`print(ff16.bits) #16`

`print(ff16.min) #-65500.0`

`print(ff16.max) #65500.0`

`print(ff16.eps) #0.00977`



# 3、时间日期和时间增量

## 3.1 datetime64基础

可以方便的将字符串转换为日期

Y 年

M 月

W 周

D 天

转换实例：

`import numpy as np`

`a = np.datetime64('2020-03-01')`

`print(a, a.dtype) #  2020-03-01 datetime64[D]`   



`a = np.datetime64('2020-03')`

`print(a, a.dtype) #  2020-03 datetime64[M]`   



从字符串创建datetime64时还可以强制转换

`a = np.datetime64('2020-03', 'D')`

`print(a, a.dtype) #  2020-03-01 datetime64[D] 强制转换到每个月的一号`   

其实2020-03 和2020-03-01表示的是同一个日期。



如果单位不统一，会一律转换成其中最小的单位。

实例：

`import numpy as np`

`a = np.array(['2020-03', '2020-03-08'])`

`print(a, a.dtype)`

这里会把2020-03 强制转换为 2020-03-01



还可以使用arange 创建指定的时间范围

实例：

`import numpy as np`

`a = np.arange('2020-08-01', '2020-08-10', dtype=np.datime64)`

`print(a)`

## 3.2 datetime64 和 timedelta64 的运算

timedelta64表示两个日期之间的差，如果单位不一致，那么按照单位最小的来输出。

实例：

`import numpy as np`

`a = np.datetime64('2020-03-08') - np.datetime64('2020-03-07 20:00')`

`print(a)`



**注意：Y、M这两个单位和其他的无法转换，会报错，因为不确定每个月多少天。**



timedelta64自身有很多运算，比如：

`a = np.timedelta64(1, 'Y') represent one year`

`b = np.timedelta64(6, 'M' represent 6 months)`

`print(2 * a) represent 2 years`



## 3.3 datetime64的应用

`主要是 numpy.busday_offset这个函数`

用来计算下一个工作日，可以通过指定参数来防止报错。

`# 2020-07-10 星期五 `

`a = np.busday_offset('2020-07-10', offsets=1) `

`print(a) # 2020-07-13`



`a = np.busday_offset('2020-07-11', offsets=1) `

`print(a) `

`# ValueError: Non-business day date in busday_offset`



`a = np.busday_offset('2020-07-11', offsets=0, roll='forward') `

`b = np.busday_offset('2020-07-11', offsets=0, roll='backward') `

`print(a) `

`# 2020-07-13 `

`print(b) `

`# 2020-07-10`



`numpy.is_busday()可以用来判断某一天是不是工作日。`

实例：

`import numpy as np`

`# 2020-07-10 星期五 `

`a = np.is_busday('2020-07-10') `

`b = np.is_busday('2020-07-11') `

`print(a) `

`# True `

 `print(b) `

`# False`



`numpy.busday_count可以用来统计两个日期间的工作日数量`

实例：

`# 2020-07-10 星期五 `

`begindates = np.datetime64('2020-07-10') `

`enddates = np.datetime64('2020-07-20') `

`a = np.busday_count(begindates, enddates) `

`b = np.busday_count(enddates, begindates) `

`print(a) `

`# 6 `

`print(b) `

`# -6`

# 4、数组的创建

## 4.1 根据数据创建数组

### 4.1.1 通过array()创建

`a = np.array([1, 2, 3, 4])`

`print(a, type(a)) # [0 1 2 3 4] <class 'numpy.ndarray'> `

### 4.1.2 通过asarray()函数创建

asarray() 和 array() 的区别在于：当数据源是ndarray 时，array()会copy一个副本出来，占内存，而asarray()不会。



`x = [[1, 1, 1], [1, 1, 1], [1, 1, 1]] `

`y = np.array(x) `

`z = np.asarray(x) `

`x[1][2] = 2 `

`print(x,type(x)) # [[1, 1, 1], [1, 1, 2], [1, 1, 1]] <class 'list'>`



### 4.1.3 通过fromfunction()函数创建

通过函数来创建数组。

`def f(x, y): return 10 * x + y`

 `x = np.fromfunction(f, (5, 4), dtype=int) `

`print(x) `

`# [[ 0 1 2 3] `

`# [10 11 12 13] `

`# [20 21 22 23] `

`# [30 31 32 33] `

`# [40 41 42 43]]`

## 4.2 通过ones 和 zeros等 填充

几种特殊的数组，ones、zeros、empty（随机数）、eye（单位数组）、diag（对角）、full（常数）

他们都具有的特征，用ones来举例就是

`zeros()函数：返回的是给定形状和类型的零数组`

`zeros_like():返回与给定数组形状和类型相同的零数组。`

实例：

`x = np.zeros(5) `

`print(x) # [0. 0. 0. 0. 0.] `

`x = np.zeros([2, 3]) `

`print(x) `

`# [[0. 0. 0.] `

`# [0. 0. 0.]]`

其他的几个都是一毛一样的！！ 都加上_like就可以！



有一个特殊的就是,indenty() 是返回一个方的单位数组

实例：

`x = np.identity(4) `

`print(x) `

`# [[1. 0. 0. 0.] `

`# [0. 1. 0. 0.] `

`# [0. 0. 1. 0.] `

`# [0. 0. 0. 1.]]`



## 4.3 利用数值范围来创建

几个重要的函数：

`arange(): 给定间隔，均匀间隔`

`linspace(): 等间隔`

`logspace(): 对数刻度为间隔`

`numpy.random.rand():0～1间的随机数组成的数组`



`x = np.arange(5) `

`print(x) # [0 1 2 3 4]`



`x = np.arange(3, 7, 2)`

` print(x) # [3 5]`



`x = np.linspace(start=0, stop=2, num=9) `

`print(x) `

`# [0. 0.25 0.5 0.75 1. 1.25 1.5 1.75 2. ]`



`x = np.logspace(0, 1, 5) `

`print(np.around(x, 2)) `

`# [ 1. 1.78 3.16 5.62 10. ]`



`x = np.linspace(start=0, stop=1, num=5) `

`x = [10 ** i for i in x] `

`print(np.around(x, 2)) `

`# [ 1. 1.78 3.16 5.62 10. ]`



`x = np.random.random(5) `

`print(x) `

`# [0.41768753 0.16315577 0.80167915 0.99690199 0.11812291]`