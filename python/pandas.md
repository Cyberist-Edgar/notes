> ***Pandas is an open source Python library for highly specialized data analysis. It is currently the reference point that all professionals using the Python language need to study for the statistical purposes of analysis and decision making***

> 1. 安装方法

```bash
conda install pandas  # Anaconda
```

```cmd
pip install pandas # pip
```

```bash
sudo apt-get install python-pandas # Ubuntu or Debian
```

> 2. 使用方式

```python
import pandas as pd
imprt numpy as np
# OR
from pandas import * 
```



> 3. Pandas 中间重要的两个数据结构

#### Series :  

> pandas中用来代表一维数据结构的对象，和数组相似，但是有一些其他的特性。它的内在结构十分简单(如下图)，由两个相互关联的数组组成，主要的数组包含着dtype类型的数据。

<table>
<th align='center' colspan='2'>Series</th>
<tr>
<td>index</td>
<td>value</td>
</tr>
<tr>
<td>0</td>
<td>12</td>
</tr>
<tr>
<td>1</td>
<td>-4</td>
</tr>
<tr>
<td>2</td>
<td>7</td>
</tr>
<tr>
<td>3</td>
<td>9</td>
</tr>
<style>
   tr{
        text-align:center;
    }
    </style>
</table>

```python
# 声明一个Series
>>> s = pd.Series([12,-4,7,9])
>>> s
0    12
1    -4
2     7
3     9
dtype: int64
# 声明之后，如果没有定义索引，pandas默认为数字，当然也可以使用自定的数据
>>> s = pd.Series([12,-4,7,9], index=['a','b','c','d'])
>>> s
a    12
b    -4
c     7
d     9
dtype: int64

>>> s.values # 返回s中的所有值
array([12, -4,  7,  9], dtype=int64)
>>> s.index   # 返回s中所有的索引
Index(['a', 'b', 'c', 'd'], dtype='object')

```
```python
# 从Series中取值
>>> s[2]  # 索引取值
7
>>> s['b']  # 自定义标签取值
-4
>>> s[0:2]  # 切片取值
a    12
b    -4
dtype: int64
>>> s[['b','c']]  # 标签列表取值
b   -4
c    7
dtype: int64
```
```python
# 给元素赋值
# 用索引赋值
>>> s[1] = 0
>>> s
a    12
b     0
c     7
d     9
dtype: int64
# 用标签来赋值    
>>> s['b'] = 1
>>> s
a    12
b     1
c     7
d     9
dtype: int64
```

```python 
# 以numpy中的数组和其他的Series来定义新的Series
>>> arr = np.array([1,2,3,4])
>>> s = pd.Series(arr)
>>> s
0    1
1    2
2    3
3    4
dtype: int32
>>> s2 = pd.Series(s)
>>> s2
0    1
1    2
2    3
3    4
dtype: int32
    
# 但是要注意这样构造出来的Series是对原来结构的引用，对他们实现的改变都会影响构造出来的Series
>>> s
0    1
1    2
2    3
3    4
dtype: int32
>>> s[1] = 32
>>> s2
0     1
1    32
2     3
3     4
dtype: int32
```

```python
# 过滤元素值，类似numpy中过滤
>>> s
0     1
1    32
2     3
3     4
dtype: int32
>>> s[s>2]
1    32
2     3
3     4
dtype: int32
```

```python
# Series 中的运算和数学函数
>>> s
0    0
1    1
2    2
3    3
4    4
5    5
# 基本运算
>>> s/2
0    0.0
1    0.5
2    1.0
3    1.5
4    2.0
5    2.5
dtype: float64
>>> s*2
0     0
1     2
2     4
3     6
4     8
5    10

# 函数运算
>>> np.sin(s)
0    0.000000
1    0.841471
2    0.909297
3    0.141120
4   -0.756802
5   -0.958924
6   -0.279415
7    0.656987
8    0.989358
9    0.412118
dtype: float64
>>> np.cosh(s)
0       1.000000
1       1.543081
2       3.762196
3      10.067662
4      27.308233
5      74.209949
6     201.715636
7     548.317035
8    1490.479161
9    4051.542025
dtype: float64
```

```python 
# 值的相关操作
>>> serd = pd.Series([1,0,2,1,2,3], index=['white','white','blue','green',' green','yellow'])
>>> serd
white     1
white     0
blue      2
green     1
 green    2
yellow    3
dtype: int64
# 返回所有的不同的值
>>> serd.unique()
array([1, 0, 2, 3], dtype=int64)
# 返回所有值及对应的出现次数
>>> serd.value_counts()
2    2
1    2
3    1
0    1
dtype: int64

# 判断是否满足条件，返回类型为bool
>>> serd.isin([0,3])
white     False
white      True
blue      False
green     False
 green    False
yellow     True
dtype: bool
# 获得满足条件的
>>> serd[serd.isin([0,3])]
white     0
yellow    3
dtype: int64
```

```python 
# NaN 值：Not a Number，代表某个空区或者不是数字的地方
# 通常情况之下，这些值都需要经过特殊处理，特别数据分析之中。
# NaN通常因为在提取某些有问题的数据，但是某些看似正常的情况下也可以产生
# 当然自己也可以定义NaN
>>> s = pd.Series([5,-3,np.NaN,14])  # 定义含有NaN的Series
>>> s
0     5.0
1    -3.0
2     NaN
3    14.0
dtype: float64
>>> s.isnull()  # 判断数据中是否有NaN的数据
0    False
1    False
2     True
3    False
dtype: bool
>>> s.notnull()  # 逐个判断是否不是NaN的
0     True
1     True
2    False
3     True
dtype: bool
>>> s[s.notnull()]  # 过滤
0     5.0
1    -3.0
3    14.0
dtype: float64
```

```python 
# 也可以直接使用字典为参数，这样字典对应的key就成了标签
>>> mydict = {'red':200, 'blue':100,'black':50}
>>> myseries = pd.Series(mydict)
>>> myseries
red      200
blue     100
black     50
dtype: int64
# 也可以直接另外在定义index，会按照键值对进行匹配，如果没有匹配成功，则返回NaN
>>> colors = ['red','yellow','orange','blue','green']
>>> myseries = pd.Series(mydict, index = colors)
>>> myseries
red       200.0
yellow      NaN
orange      NaN
blue      100.0
green       NaN
dtype: float64
```

```python
# Series之间的运算：如果键值对不同进行运算，则返回NaN，否则进行对应的运算
>>> myseries
red       200.0
yellow      NaN
orange      NaN
blue      100.0
green       NaN
dtype: float64
>>> myseries2 =  {'red':400,'yellow':1000,'black':700}
>>> myseries2 = pd.Series(myseries2)
>>> myseries2
red        400
yellow    1000
black      700
dtype: int64
>>> myseries + myseries2
black       NaN
blue        NaN
green       NaN
orange      NaN
red       600.0
yellow      NaN
dtype: float64
```



#### DataFrame:

> DateFrame 是一种表格式的数据结构，十分类似我们平常使用的电子表格，DataFrame的出现是因为为了将series的功能提升到多维问题上，但是DataFrame也有其扩展。DataFrame 中包含有序的列，而这些列中间可以包含不同的数据类型.

<table>
<th align='center' colspan='4'>DataFrame</th>
<tr>
<td>index</td>
<td colspan='3'>columns</td>
</tr>
<tr>
<td>0</td>
<td>color</td>
    <td>object</td>
    <td>price</td>
</tr>
<tr>
<td>1</td>
<td>blue</td>
    <td>ball</td>
    <td>1.2</td>
</tr>
<tr>
<td>2</td>
<td>green</td>
    <td>pen</td>
    <td>1.0</td>
</tr>
<tr>
<td>3</td>
<td>yellow</td>
    <td>pencil</td>
    <td>0.6</td>
</tr>
    <tr>
        <td>4</td>
        <td>red</td>
        <td>paper</td>
        <td>0.9</td>
    </tr>
    <tr>
        <td>4</td>
        <td>white</td>
        <td>mug</td>
        <td>1.7</td>
    </tr>
<style>
   tr{
        text-align:center;
    }
    </style>
</table>



> 与Series不同，DataFrame有两个索引数组，第一个索引与列相关，第二个索引与行相关

```python
# 创建一个DateFrame， 字典的key值被视为列的名称
>>> data
{'color': ['blue', 'green', 'yellow', 'red', 'white'], 'object': ['ball', 'pen', 'pencil', 'paper', 'mug'], 'price': [1.2, 1.0, 0.6, 0.9, 1.7]}
>>> frame = pd.DataFrame(data)
>>> frame
    color  object  price
0    blue    ball    1.2
1   green     pen    1.0
2  yellow  pencil    0.6
3     red   paper    0.9
4   white     mug    1.7

# 如果data中的数据中间有不需要的，指定需要的数据即可
>>> frame2 = pd.DataFrame(data, columns=['object','price']) >>> frame2
   object  price
0    ball    1.2
1     pen    1.0
2  pencil    0.6
3   paper    0.9
4     mug    1.7

# 类似Series，标签可以自定义
>>> frame3 = pd.DataFrame(data, index=['one','two','three','four','five'])
>>> frame3
        color  object  price
one      blue    ball    1.2
two     green     pen    1.0
three  yellow  pencil    0.6
four      red   paper    0.9
five    white     mug    1.7

# 使用矩阵来定义 
>>> frame4 = pd.DataFrame(np.arange(16).reshape((4,4)), ...                   index=['red','blue','yellow','white'], ...                   columns=['ball','pen','pencil','paper']) 
>>> frame4       
		 ball  pen  pencil  paper 
red        0    1       2      3 
blue       4    5       6      7 
yellow     8    9      10     11 
white     12   13      14     15
```



```python
# 从DataFrame中挑选出数据
>>> frame.columns # 挑选出所有的列名
Index(['color', 'object', 'price'], dtype='object')
>>> frame.index # 标签名
RangeIndex(start=0, stop=5, step=1)
>>> frame.values  # 值
array([['blue', 'ball', 1.2],
       ['green', 'pen', 1.0],
       ['yellow', 'pencil', 0.6],
       ['red', 'paper', 0.9],
       ['white', 'mug', 1.7]], dtype=object)
>>> frame['price']  # 类似字典的使用，且和series类似
0    1.2
1    1.0
2    0.6
3    0.9
4    1.7
Name: price, dtype: float64
>>> frame.price # 和上一样
0    1.2
1    1.0
2    0.6
3    0.9
4    1.7
Name: price, dtype: float64
>>> frame.loc[2]  # 定位第2+1行的数据，起始为0
color     yellow
object    pencil
price        0.6
Name: 2, dtype: object
>>> frame.loc[[2,4]]  # 一次可定位多个
    color  object  price
2  yellow  pencil    0.6
4   white     mug    1.7


# 也可以根据切片来选取
>>> frame[1:3]
    color  object  price
1   green     pen    1.0
2  yellow  pencil    0.6
>>> frame['object'][3]
'paper'
```



```python 
# 赋值类操作
# 可以自定义行列的名称
>>> frame.index.name = 'id'
>>> frame.columns.name = 'item'
>>> frame
item   color  object  price
id
0       blue    ball    1.2
1      green     pen    1.0
2     yellow  pencil    0.6
3        red   paper    0.9
4      white     mug    1.7

# 可以类似字典的操作增加一个新的列
>>> frame['new'] = 12  # 只声明一个，全部都是
>>> frame
item   color  object  price  new
id
0       blue    ball    1.2   12
1      green     pen    1.0   12
2     yellow  pencil    0.6   12
3        red   paper    0.9   12
4      white     mug    1.7   12
>>> frame['new'] = [3.0,1.3,2.2,0.8,1.1] # 当然可以一次性全部赋值
>>> frame
item   color  object  price  new
id
0       blue    ball    1.2  3.0
1      green     pen    1.0  1.3
2     yellow  pencil    0.6  2.2
3        red   paper    0.9  0.8
4      white     mug    1.7  1.1

# 也可以使用Series来添加
>>> frame['new2']=ser
>>> frame
item   color  object  price  new  new2
id
0       blue    ball    1.2  3.0     0
1      green     pen    1.0  1.3     1
2     yellow  pencil    0.6  2.2     2
3        red   paper    0.9  0.8     3
4      white     mug    1.7  1.1     4

# 若要修改其中的某个值，可类似二维数组进行操作
>>> frame['price'][2]=3.3
>>> frame
item   color  object  price  new  new2
id
0       blue    ball    1.2  3.0     0
1      green     pen    1.0  1.3     1
2     yellow  pencil    3.3  2.2     2
3        red   paper    0.9  0.8     3
4      white     mug    1.7  1.1     4
```



```python 
# 对DataFrame中的值进行操作
>>> frame.isin([1.0, 'pen'])
item  color  object  price    new   new2
id
0     False   False  False  False  False
1     False    True   True  False   True
2     False   False  False  False  False
3     False   False  False  False  False
4     False   False  False  False  False
>>> frame[_] # 对上述结果符合的进行过滤
item color object  price  new  new2
id
0      NaN    NaN    NaN  NaN   NaN
1      NaN    pen    1.0  NaN   1.0
2      NaN    NaN    NaN  NaN   NaN
3      NaN    NaN    NaN  NaN   NaN
4      NaN    NaN    NaN  NaN   NaN

# 删除列
>>> frame
item   color  object  price  new  new2
id
0       blue    ball    1.2  3.0     0
1      green     pen    1.0  1.3     1
2     yellow  pencil    3.3  2.2     2
3        red   paper    0.9  0.8     3
4      white     mug    1.7  1.1     4
>>> del frame['new']  # 使用del
>>> frame
item   color  object  price  new2
id
0       blue    ball    1.2     0
1      green     pen    1.0     1
2     yellow  pencil    3.3     2
3        red   paper    0.9     3
4      white     mug    1.7     4

```

```python 
# 同时还可以用嵌套的字典来是赋值
>>> nestdict = { 'red': { 2012: 22, 2013: 33 },                    ...     		    'white': { 2011: 13, 2012: 22, 2013: 16},         ...              'blue': {2011: 17, 2012: 27, 2013: 18}}
>>> frame2 = pd.DataFrame(nestdict)
>>> frame2
       red  white  blue
2012  22.0     22    27
2013  33.0     16    18
2011   NaN     13    17
```

```python
# DataFrame的转置
>>> frame2.T
       2012  2013  2011
red    22.0  33.0   NaN
white  22.0  16.0  13.0
blue   27.0  18.0  17.0
```

```python
# 索引的方法
>>> ser = pd.Series([5,0,3,8,4], index=['red','blue','yellow','white','green'])
>>> ser.index
Index(['red', 'blue', 'yellow', 'white', 'green'], dtype='object')
>>> ser.idxmin()  # 获取index中最小的元素，按字符串比较
'blue'
>>> ser.idxmax()  # 获取index中最大的
'white'
# 另外对于pandas中的DataFrame类型和Series类型，可以使用
# 需要判断的类型示例.index.is_unique 来判断中间是否有相同的index
# 有则返回False， 无则返回True
>>> ser.index.is_unique
True
```

> **其他的一些方法**

```python
# reindex -> 重新更改index
>>> ser = pd.Series([2,5,7,4], index=['one','two','three','four'])
>>> ser
one      2
two      5
three    7
four     4
dtype: int64
>>> ser.reindex(['three', 'four', 'five','one'])
three    7.0
four     4.0
five     NaN
one      2.0
dtype: float64
   
# 但是如果有时候所有的index都是数字，我们需要将他们填满
# 这时候我们可以指定method
>>> ser3 = pd.Series([1,5,6,3],index=[0,3,5,6])
>>> ser3
0    1
3    5
5    6
6    3
dtype: int64
>>> ser3.reindex(range(6), method='ffill')  # 补充的和之前的元素对齐
0    1
1    1
2    1
3    5
4    5
5    6
dtype: int64
>>> ser3.reindex(range(6), method='bfill')  # 补充的元素和之后的元素对齐
0    1
1    5
2    5
3    5
4    6
5    6
dtype: int64
    
```

```python
# 删除index对应的元素 使用drop函数 DataFrame和series均可使用
>>> ser = pd.Series(np.arange(4.), index=['red','blue','yellow','white'])
>>> ser
red       0.0
blue      1.0
yellow    2.0
white     3.0
dtype: float64
>>> ser.drop('yellow')
red      0.0
blue     1.0
white    3.0
dtype: float64
    
# 对于DataFrame有类似的操作，但是如果要删除列的时候需要指定axis=1
>>> frame = pd.DataFrame(np.arange(16).reshape((4,4)),
...  index=['red','blue','yellow','white'],
... columns=['ball','pen','pencil','paper'])
>>> frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>> frame.drop('blue')
        ball  pen  pencil  paper
red        0    1       2      3
yellow     8    9      10     11
white     12   13      14     15
>>> frame.drop('pen', axis=1)
        ball  pencil  paper
red        0       2      3
blue       4       6      7
yellow     8      10     11
white     12      14     15
```

```python
# 数据结构之间的运算,可以使用方法，也可以直接使用运算符
# add()  ->  +
# sub()  ->  -
# div()  ->  /
# mul()  ->  *
# 但是要注意的一点是，只有对应的数据进行操作的时候才有可读结果
# 否则结果是NaN
>>> frame1 = pd.DataFrame(np.arange(16).reshape((4,4)), index=['red', 'blue','yellow', 'white'], columns=['ball', 'pen', 'pencil', 'paper'])
>>> frame2 = pd.DataFrame(np.arange(12).reshape((4,3)), index=['blue','green','white','yellow'], columns=['mug','pen','ball'])
>>> frame1
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>> frame2
        mug  pen  ball
blue      0    1     2
green     3    4     5
white     6    7     8
yellow    9   10    11
>>> frame1 + frame2  # 直接加
        ball  mug  paper   pen  pencil
blue     6.0  NaN    NaN   6.0     NaN
green    NaN  NaN    NaN   NaN     NaN
red      NaN  NaN    NaN   NaN     NaN
white   20.0  NaN    NaN  20.0     NaN
yellow  19.0  NaN    NaN  19.0     NaN
>>> frame1.add(frame2)  # 使用方法
        ball  mug  paper   pen  pencil
blue     6.0  NaN    NaN   6.0     NaN
green    NaN  NaN    NaN   NaN     NaN
red      NaN  NaN    NaN   NaN     NaN
white   20.0  NaN    NaN  20.0     NaN
yellow  19.0  NaN    NaN  19.0     NaN

# 此外DataFrame 类型还可以与Series进行运算 Series相当于其中的一行
>>> frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>> ser = pd.Series(np.arange(4), index=['ball','pen','pencil', 'paper'])
>>> frame - ser
        ball  pen  pencil  paper
red        0    0       0      0
blue       4    4       4      4
yellow     8    8       8      8
white     12   12      12     12


```

```python
# 使用自己定义的函数对数据结构进行操作，这时候使用apply方法
# 如果进行的是行操作的话，指定axis=1
>>> f = lambda x:x.max() - x.min()
>>> frame.apply(f)
ball      12
pen       12
pencil    12
paper     12
dtype: int64
>>> frame.apply(f, axis=1)
red       3
blue      3
yellow    3
white     3
dtype: int64   
```

```python
# 数据统计类函数
>>> frame.sum()
ball      24
pen       28
pencil    32
paper     36
dtype: int64
>>> frame.mean()
ball      6.0
pen       7.0
pencil    8.0
paper     9.0
dtype: float64
>>> frame.mean(axis=1)
red        1.5
blue       5.5
yellow     9.5
white     13.5
dtype: float64
>>> frame.describe()
            ball        pen     pencil      paper
count   4.000000   4.000000   4.000000   4.000000
mean    6.000000   7.000000   8.000000   9.000000
std     5.163978   5.163978   5.163978   5.163978
min     0.000000   1.000000   2.000000   3.000000
25%     3.000000   4.000000   5.000000   6.000000
50%     6.000000   7.000000   8.000000   9.000000
75%     9.000000  10.000000  11.000000  12.000000
max    12.000000  13.000000  14.000000  15.000000
```

```python
# 排序
# Series类
>>> ser
ball      0
pen       1
pencil    2
paper     3
dtype: int32
>>> ser.sort_index()   # 按索引排列
ball      0
paper     3
pen       1
pencil    2
dtype: int32
>>> ser.sort_values()   # 按值排列
ball      0
pen       1
pencil    2
paper     3
dtype: int32
>>> ser.rank()  # 确定索引的排序的次序排名
ball      1.0
pen       2.0
pencil    3.0
paper     4.0
dtype: float64
>>> ser.rank(method='first')  # 按第一列来排序，递增
ball      1.0
pen       2.0
pencil    3.0
paper     4.0
dtype: float64
>>> ser.rank(method='first', ascending=False) # 按第一列来排序，但是非递增
ball      4.0
pen       3.0
pencil    2.0
paper     1.0
dtype: float64    
    
# DataFrame类   
>>> frame
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
>>> frame.sort_index(axis=1)  # 行index按序排列
        ball  paper  pen  pencil
red        0      3    1       2
blue       4      7    5       6
yellow     8     11    9      10
white     12     15   13      14
>>> frame.sort_index() # 列index按序
        ball  pen  pencil  paper
blue       4    5       6      7
red        0    1       2      3
white     12   13      14     15
yellow     8    9      10     11
>>> frame.sort_values(by='pen')  # 必须指定哪一个值
        ball  pen  pencil  paper
red        0    1       2      3
blue       4    5       6      7
yellow     8    9      10     11
white     12   13      14     15
```

```python

```






