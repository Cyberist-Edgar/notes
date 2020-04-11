> **NumPy is a basic package for scientific computing with Python and especially for data analysis. In fact, this library is the basis of a large amount of mathematical and scientific Python packages， with its powerful support，we can do a lot of things**


> 1. 安装方法
```bash
sudo apt-get install python-numpy # Ubuntu or Debian
```

```bash
pip install numpy # Windows
```
```bash
conda install numpy  # Anaconda 
```

>2. 使用方法
```py
>>> import numpy as np # 导入库
>>> a = np.array([1,2,3,4])  # 创建最简单的一维数组
>>> a.size # 获取数组中的元素个数
4
>>> a.itemsize # 返还数组的每个元素所占字节数
4
>>> a.ndim # 获取数组的维度
1
>>> a.shape # 获取数组的形状，返回类型为元组
(4,)
>>> a.dtype # 返回数组的数据类型
dtype('int32')

# 当然也可以创建多维数组
>>> b = np.array([[1,3.4,2],[2,4,5]])
>>> b.size
6
>>> b.itemsize
8
>>> b.ndim
2
>>> b.shape
(2, 3)
>>> b.dtype
dtype('float64')
```

>3. dtype 选项

|Data Type |Description|
|---|:-----|
|bool_| boolean (true or false) stored as a byte|
| int_| Default integer type (same as C long; normally either int64 or int32) |
|intc| identical to C int (normally int32 or int64)|
|intp |integer used for indexing (same as C size_t; normally either int32 or int64)|
| int8 |byte (–128 to 127) |
|int16| integer (–32768 to 32767)| int32 |integer (–2147483648 to 2147483647) |
|int64 |integer (–9223372036854775808 to 9223372036854775807) |
|uint8 |unsigned integer (0 to 255) uint16 unsigned integer (0 to 65535) |
|uint32 |unsigned integer (0 to 4294967295)|
| uint64 |unsigned integer (0 to 18446744073709551615) |
|float_ |Shorthand for float64|
| float16 |half precision float: sign bit, 5-bit exponent, 10-bit mantissa|
| float32| Single precision float: sign bit, 8-bit exponent, 23-bit mantissa |
|float64 |Double precision float: sign bit, 11-bit exponent, 52-bit mantissa 
|complex_ |Shorthand for complex128 |
|complex64 |Complex number, represented by two 32-bit floats (real and imaginary components) |
|complex128| Complex number, represented by two 64-bit floats (real and imaginary components)|


```py
# 在使用array的时候可以指定dtype类型
>>> f = np.array([1,2,3],dtype=complex)
>>> f
array([1.+0.j, 2.+0.j, 3.+0.j])
```

>4. 其他创建数组的方式

```py
# 创建元素全为0的指定大小数组
>>> np.zeros((3,3))
array([[0., 0., 0.],
       [0., 0., 0.],
       [0., 0., 0.]])
# 创建元素全为1的指定大小数组
>>> np.ones((3,3))
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
# 创建一个顺序数组
>>> np.arange(0,10)
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> np.arange(0,10,3)
array([0, 3, 6, 9])
# linspace(start, end, elem_num)
>>> np.linspace(0,10,5)
array([ 0. ,  2.5,  5. ,  7.5, 10. ])
# 创建随机数组
>>> np.random.random(4)
array([0.96215695, 0.64629081, 0.86759819, 0.90073058])
>>> np.random.random((3,3))
array([[0.27513149, 0.11492121, 0.3915962 ],
       [0.08766173, 0.46365986, 0.0249593 ],
       [0.42738941, 0.00530375, 0.48070045]])
# reshape函数变化数组形状，但元素数目不变
>>> a = np.arange(12).reshape((3,4))
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
```

>5. 基本运算
```py
>>> a
array([0, 1, 2, 3])
>>> a + 4  # 全部元素 + 4
array([4, 5, 6, 7])
>>> a * 2  # 全部元素 *2
array([0, 2, 4, 6])
>>> b = np.arange(4,8)
>>> b
array([4, 5, 6, 7])
>>> a + b  # 相对应的位置相加
array([ 4,  6,  8, 10])
>>> a - b  # 相对应的位置相减
array([-4, -4, -4, -4])
>>> a * b  # 相对应的位置相乘
array([ 0,  5, 12, 21])
>>> a * np.sin(b)  # 这里只能使用np中的sin
array([-0.        , -0.95892427, -0.558831  ,  1.9709598 ]) >>> a * np.sqrt(b)
array([0.        , 2.23606798, 4.89897949, 7.93725393])
```
>6. 矩阵运算
```py
# 矩阵乘法
>>> A = np.array([[1,2,3], [4, 5, 6], [7,8,9]])
>>> A
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
>>> B
array([[2, 3, 4],
       [5, 6, 7],
       [8, 9, 0]])
>>> A.dot(B)
array([[ 36,  42,  18],
       [ 81,  96,  51],
       [126, 150,  84]])
>>> np.dot(A, B)
array([[ 36,  42,  18],
       [ 81,  96,  51],
       [126, 150,  84]])
```

>7. 函数使用
```py
------------------------一般函数------------------------
array([1, 2, 3, 4])
>>> np.sqrt(a)
array([1.        , 1.41421356, 1.73205081, 2.        ])
>>> np.sin(a)
array([ 0.84147098,  0.90929743,  0.14112001, -0.7568025 ]) 
>>> np.log(a)
array([0.        , 0.69314718, 1.09861229, 1.38629436])
>>> np.cos(a)
array([ 0.54030231, -0.41614684, -0.9899925 , -0.65364362]) 
>>> np.tan(a)
array([ 1.55740772, -2.18503986, -0.14254654,  1.15782128]) 
>>> np.tanh(a)
array([0.76159416, 0.96402758, 0.99505475, 0.9993293 ])
>>> np.sinh(a)
array([ 1.17520119,  3.62686041, 10.01787493, 27.2899172 ])

------------------------聚合函数-------------------------
>>> a = np.array([3.3, 4.5,1.2,5.7,0.3])
>>> a.sum()  # 所有元素之和
15.0
>>> a.min()  # 最小元素
0.3
>>> a.max()  # 最大元素
5.7
>>> a.mean()  # 平均值
3.0
>>> a.std()  # 方差
2.0079840636817816
```

>8. 索引，切片，迭代
```py
# ------------------------索引-----------------------------
>>> a = np.arange(10,16)
>>> a
array([10, 11, 12, 13, 14, 15])
>>> a[4]
14
>>> a[-1] # 倒数第一个
15
>>> a[-6]  # 倒数第6个
10
>>> a[[1,3,4]]  # 取第2,4,5个
array([11, 13, 14])
>>> A = np.arange(10,19).reshape((3,3))
>>> A
array([[10, 11, 12],
       [13, 14, 15],
       [16, 17, 18]])
>>> A[1,2]  # 取第2行第3列的数据
15
```
```py
# -------------------------切片------------------------
>>> a = np.arange(10,16)
>>> a
array([10, 11, 12, 13, 14, 15])
>>> a[1:5] # 取第2到第5个元素
array([11, 12, 13, 14])
>>> a[1:5:2] # 从第2到第5个元素中隔2取一个
array([11, 13])
>>> a[::2] # 每隔2取一个元素
array([10, 12, 14]) 
>>> a[:5:] # 取前5个元素
array([10, 11, 12, 13, 14])
>>> A = np.arange(10,19).reshape((3,3))
>>> A
array([[10, 11, 12],
       [13, 14, 15],
       [16, 17, 18]])
>>> A[0,:]  # 取第一行数据
array([10, 11, 12])
>>> A[:,0]  # 取第一列数据
array([10, 13, 16])
>>> A[0:2,0:2]  # 取1到2行并且1到2列的数据
array([[10, 11],
       [13, 14]])
>>> A[[0,2],0:2] # 取指定行1,3的数据并取其中的前两个
array([[10, 11],
       [16, 17]])
```
```py
# -----------------------迭代-------------------------
>>> for i in a:  # 一维数组迭代，输出每一个数据
...     print(i)
...
10
11
12
13
14
15
>>> for row in A:  # 二维数组这样迭代每次输出每一行
...     print(row)
...
[10 11 12]
[13 14 15]
[16 17 18]
>>> for item in A.flat:  # 可每次打印出A中每一个元素
...     print(item)
...
10
11
12
13
14
15
16
17
18

```
```py
# ---------------------聚合函数------------------
>>> A = np.arange(10,19).reshape((3,3))
>>> A
array([[10, 11, 12],
       [13, 14, 15],
       [16, 17, 18]])
>>> np.apply_along_axis(np.mean,axis=0,arr=A) # apply_along_axis接三个参数，第一个为函数名称，第二个为轴，0代表横轴，1代表列，第三个为数组名
array([13., 14., 15.])
>>> np.apply_along_axis(np.mean,axis=1,arr=A)
array([11., 14., 17.])

# 当然也可以利用自己构建的函数
>>> def foo(x):
...     return x/2
...
>>> np.apply_along_axis(foo, axis=1,arr=A)
array([[5. , 5.5, 6. ],
       [6.5, 7. , 7.5],
       [8. , 8.5, 9. ]])
```


> 8. 条件和布尔数组

   ```py
   >>> A = np.random.random((4,4))
   >>> A    array([[0.77114544, 0.08274154, 0.3380893 , 0.09124811],
          [0.38458493, 0.04101999, 0.5752505 , 0.36101901],
          [0.281558  , 0.17994346, 0.5637831 , 0.82207882],
          [0.87628886, 0.84768998, 0.9459325 , 0.93927827]])
   >>> A < 0.5  # 符合条件的被视为True    array([[False,  True,  True,  True],
          [ True,  True, False,  True],
          [ True,  True, False, False],
          [False, False, False, False]])
   >>> A[A < 0.5] # 挑选出符合条件的    array([0.08274154, 0.3380893 , 0.09124811, 0.38458493, 0.04101999,
          0.36101901, 0.281558  , 0.17994346])    
   ```

> 9. 数组的形状操作

   ```python
   >>> a = np.random.random(12)
   >>> a    array([0.79770216, 0.56088829, 0.82827561, 0.77196219, 0.98281623,
          0.69956778, 0.11579138, 0.22625572, 0.13765588, 0.13327294,
          0.5892318 , 0.55351276])
   >>> A = a.reshape(3,4)  # 返回一个自定义的形状的数组，数组内容保持不变，形状改变
   >>> A    array([[0.79770216, 0.56088829, 0.82827561, 0.77196219],
          [0.98281623, 0.69956778, 0.11579138, 0.22625572],
          [0.13765588, 0.13327294, 0.5892318 , 0.55351276]])
   >>> a.shape = (3,4) # 直接改变本身的形状
   >>> a    array([[0.79770216, 0.56088829, 0.82827561, 0.77196219],
          [0.98281623, 0.69956778, 0.11579138, 0.22625572],
          [0.13765588, 0.13327294, 0.5892318 , 0.55351276]])
   >>> a = a.ravel()  # 将2维数组变为一维数组
   >>> a    array([0.79770216, 0.56088829, 0.82827561, 0.77196219, 0.98281623,
          0.69956778, 0.11579138, 0.22625572, 0.13765588, 0.13327294,
          0.5892318 , 0.55351276])
   >>> A.transpose()  # 数组的转置    array([[0.79770216, 0.98281623, 0.13765588],
          [0.56088829, 0.69956778, 0.13327294],
          [0.82827561, 0.11579138, 0.5892318 ],
          [0.77196219, 0.22625572, 0.55351276]])   
   ```

> 10. 数组操作

```python
# --------------------数组的合并-----------------------
>>> A = np.ones((3,3))
>>> B = np.zeros((3,3))
>>> np.vstack((A,B))  # vertical stacking 垂直堆积 array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.],
       [0., 0., 0.],
       [0., 0., 0.],
       [0., 0., 0.]])
>>> np.hstack((A,B))  # horizontal stacking 水平方向堆积 array([[1., 1., 1., 0., 0., 0.],
       [1., 1., 1., 0., 0., 0.],
       [1., 1., 1., 0., 0., 0.]])

>>> a = np.array([0,1,2])
>>> b = np.array([3,4,5,])
>>> c = np.array([6,7,8])
>>> np.column_stack((a,b,c))  # 一列一列的堆积 array([[0, 3, 6],
       [1, 4, 7],
       [2, 5, 8]])
>>> np.row_stack((a,b,c))  # 一行一行的堆积 array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]]) 
```


```py
# -------------------数组的切分----------------------
>>> A = np.arange(16).reshape(4,4)
>>> A array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
>>> [B,C] = np.hsplit(A, 2) # horizontally split 水平内容分成两半
>>> B array([[ 0,  1],
       [ 4,  5],
       [ 8,  9],
       [12, 13]])
>>> C array([[ 2,  3],
       [ 6,  7],
       [10, 11],
       [14, 15]])
>>> [B,C] = np.vsplit(A, 2)  # vertically split 竖直内容分成两半
>>> B array([[0, 1, 2, 3],
       [4, 5, 6, 7]])
>>> C array([[ 8,  9, 10, 11],
       [12, 13, 14, 15]])
>>> [A1,A2,A3] = np.split(A,[1,3],axis=1) # axis=1指定竖直方向切割， [1, 3] 表示切成第1列，2-3列，最后几列
>>> A1 array([[ 0],
       [ 4],
       [ 8],
       [12]])
>>> A2 array([[ 1,  2],
       [ 5,  6],
       [ 9, 10],
       [13, 14]])
>>> A3 array([[ 3],
       [ 7],
       [11],
       [15]])

```



> 10. 其他的一般概念

```python
# 对象的浅拷贝和深拷贝是两个不同的概念：浅拷贝之后的对象执行任何操作，原对象都不会受到影响，但是深拷贝的对象会随之改变
>>> a = np.array([1, 2, 3, 4]) 
>>> b = a  # 深拷贝，相当于C/C++中的引用
>>> b 
array([1, 2, 3, 4]) 
>>> a[2] = 0 
>>> b 
array([1, 2, 0, 4])

>>> c = a[0:2]  # 仍然是深拷贝
>>> c 
array([1, 2]) 
>>> a[0] = 0 
>>> c 
array([0, 2])

>>> a = np.array([1, 2, 3, 4]) 
>>> c = a.copy()  # 利用copy函数的为浅拷贝，相当于副本
>>> c 
array([1, 2, 3, 4]) 
>>> a[0] = 0 
>>> c 
array([1, 2, 3, 4])
```
```python
# numpy中数组的向量化，当numpy中直接执行连个数组相乘的时候，并不会获得理想的结果，而只是对应位置的相乘，for example:
>>> A
array([[0.67874916, 0.09553741, 0.39305942],
       [0.02969179, 0.97060658, 0.93600762],
       [0.77268671, 0.2525612 , 0.03904034]])
>>> A = np.arange(6).reshape(2,3)
>>> A
array([[0., 1., 2.],
       [3., 4., 5.]])
>>> B = np.ones((2,3))
>>> A * B
array([[0., 1., 2.],
       [3., 4., 5.]])
```
```python
# 当两个及两个以上的数组满足一定的条件之后，即使两个数组的形状不同可以进行运算，称为广播运算
# 条件：他们所有的维度都可以兼容，每一个维数都必须相同，或者有一个必须为1
>>> A = np.arange(16).reshape(4, 4) 
>>> b = np.arange(4) 
>>> A 
array([[ 0,  1,  2,  3],       
       [ 4,  5,  6,  7],       
       [ 8,  9, 10, 11],       
       [12, 13, 14, 15]]) 
>>> b 
array([0, 1, 2, 3])
# A 为 4 x 4, b 为 4 x 1
# A + b 运算的时候 b 相当于 
>>> np.row_stack((b,b,b,b))
array([[0, 1, 2, 3],
       [0, 1, 2, 3],
       [0, 1, 2, 3],
       [0, 1, 2, 3]])
```
```py
# 结构体数组， 数组中的元素可以是不同的，数据类型可以表示为：
# bytes                     b1 
# int                       i1, i2, i4, i8 
# unsigned ints             u1, u2, u4, u8 
# floats                    f2, f4, f8 
# complex                   c8, c16 
# fixed length strings      a<n>

# 可以使用上述的数据类型简称
>>> structured = np.array([(1, 'First', 0.5, 1+2j),(2, 'Second', 1.3, 2-2j), (3, 'Third', 0.8, 1+3j)],dtype=('i2, a6, f4, c8'))
>>> structured
array([(1, b'First', 0.5, 1.+2.j), (2, b'Second', 1.3, 2.-2.j),
       (3, b'Third', 0.8, 1.+3.j)],
      dtype=[('f0', '<i2'), ('f1', 'S6'), ('f2', '<f4'), ('f3', '<c8')])
# 也可以显式的显示数据类型
>>> structured = np.array([(1, 'First', 0.5, 1+2j),(2, 'Second', 1.3, 2-2j), (3, 'Third', 0.8, 1+3j)],dtype=('int16, a6, float32, complex64'))
>>> structured
array([(1, b'First', 0.5, 1.+2.j), (2, b'Second', 1.3, 2.-2.j),
       (3, b'Third', 0.8, 1.+3.j)],
      dtype=[('f0', '<i2'), ('f1', 'S6'), ('f2', '<f4'), ('f3', '<c8')])

# 可以用索引来获得对应的row
>>> structured[1]
(2, b'Second', 1.3, 2.-2.j)
# 可以用结构体索引，可以引用相同类型的元素
>>> structured['f1']
array([b'First', b'Second', b'Third'], dtype='|S6')

```

