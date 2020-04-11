# tqdm 简介及正确的打开方式


## 1. 什么是tqdm？

`tqdm`是一个快速的，易扩展的进度条提示模块，官方网站[点击这里](https://tqdm.github.io/)

但是为什么要取名为这样呢，官方上说：

>  `tqdm` derives from the Arabic word *taqaddum* (تقدّم) which can mean "progress," and is an abbreviation for "I love you so much" in Spanish (*te quiero demasiado*). 

所以呢,`tqdm`其实本身就有进度的意思，另外在西班牙语中还有`I love you so much`的意思，由此看来，作者应该是个语言大家呢



## 2. tqdm的安装

tqdm不是python的标准库，但安装很简单，直接使用`pip`即可，当然为了减少墙带来的麻烦，可以指定镜像源，关于`pip镜像源的更换`，可以查看[这篇文章]( https://blog.csdn.net/weixin_44676081/article/details/104797154 )

```
pip install tqdm -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

或者，如果你使用`Anaconda`:

```
conda install -c conda-forge tqdm
```



##  3. 三大使用方式

- `基于迭代类型`

  ```python
  # 向tqdm中传入迭代类型即可
  from tqdm import tqdm
  import time
  
  text = ""
  for char in tqdm(["a", "b", "c", "d"]):
      time.sleep(0.25)
      text = text + char
  ```

  效果如下：

  <img src="https://img-blog.csdnimg.cn/20200313095040435.gif">

  

- `手动更新进度`

  ```python
  # 使用with语句来控制tqdm的更新
  with tqdm(total=100) as pbar:
      for i in range(10):
          time.sleep(0.01)
          pbar.update(1)  # 每次更新的多少
         
  # 当然也可以不用with，将tqdm赋值给一个变量
  pbar = tqdm(total=100)
  for i in range(100):
      time.sleep(0.1)
      pbar.update(10)
  pbar.close()  # ！！ 注意这样使用之后必须调用del 或者close方法删除该变量
  ```
  效果如下：
  <img src="https://img-blog.csdnimg.cn/20200313100213321.gif">

  

- `在命令行中使用`

  ```bash
  # 将输出的结果内容通过管道传送给 tqdm
  time find . -name '*.py' -type f -exec cat \{} \; | tqdm | wc -l
  ```

  效果如下：
  
  <img src="https://img-blog.csdnimg.cn/20200313101420839.gif">
  
  
  
  命令行使用的更多参数可见官网



## 4. `tqdm.tqdm` 的使用方式

   > [官方]( https://tqdm.github.io/docs/tqdm/ )中对该模块的讲解甚多，这里也着重讲解

   - ### 参数
#### `iterable=None`

- iterable
- 可选
- 装饰进度条，如果为空，需要手动进行更新

#### `desc=None`

- str
- 可选
- 进度条的前面的提示

#### `total=None`

- int 或者 float
- 可选
- 迭代元素的多少

    - 如果没有指定，如果可以，使用len(iterable)代替
    - 如果是float("inf")，只显示统计的基本信息
    - 如果参数gui为True，并且该参数需要后续更新，则初始化为一个任意大的正数，如9e9

#### `leave=True`

- bool
- 可选
- 如果True，显示所有的进度条，如果是None，只显示第一个进度条

#### `file=None`

- io.TextIOWrapper 或者 io.StringIO
- 可选
- 指定输出的路径

#### `ncols=None`

- int
- 可选
- 输出信息的宽度

    - 指定了，动态改变进度条的宽度
    - 未指定，使用环境的宽度

#### `mininterval=0.1`

- float
- 可选
- 最小进度显示更新间隔

#### `maxinterval=10.0`

- float
- 可选
- 最大进度显示更新间隔

#### `miniters=None`

- int 或者 float
- 可选
- 在迭代中显示最小进度的间隔

    - 如果为0，并且指定dynamic_miniters，会自动的调整到mininterval
    - 如果>0，跳过指定数目的迭代

#### `ascii=None`

- bool 或者 str
- 可选
- 如果没有指定或者为False，使用平滑块(默认)，否则使用ascii字符，指定字符的时候长度需大于2

#### `disable=False`

- bool
- 可选
- 是否禁用整个进度条，如果是None，在non-tty上禁用

#### `unit='it'`

- str
- 可选
- 将用于定义每个迭代的单元的字符串

#### `unit_scale=False`

- bool 或 int  或 float
- 可选
- 如果为1或为真，则迭代次数将自动减少/缩放，并在国际单一性系统标准之后添加一个度量前缀(kilo、mega等)[默认值:False]。如果有任何其他非零数，将按比例总计和n。

#### `dynamic_ncols=False`

- bool
- 可选
- 设置为True之后，ncols相当于无效

#### `smoothing=0.3`

- float
- 可选
- 以指数型增长的速度进行增长

    - 0~1
    - gui模式下无效

#### `bar_format=None`

- str
- 可选
- 自定义进度条字符格式，可能会影响性能

    - 具体见官网

#### `initial=0`

- int 或 float
- 可选
- 初始计数值

#### `position=None`

- int
- 可选
- 指定偏移量，从0开始

#### `postfix=None`

- dic 
- 可选
- 在进度条后指定额外的信息

    - 也可调用函数set_postfix实现

#### `unit_divisor=1000`

- float
- 可选
- 除非unit_scale=True，否则忽略

#### `write_bytes=None`

- bool
- 可选
    - 如果None并且文件未指定，字节将用Python 2编写
    - 如果为真，也将写入字节
    - 其他情况下，将默认为unicode。 

#### `lock_args=None`

- tuple
- 可选
- 获取中间输出

#### `gui=False`

- bool
- 可选
- 内部参数，使用tqdm.gui.tqdm替代

    - 设置True，会使用mtplotlib中的动画输出

#### `**kwargs`

<br>
<hr>

- ## 方法

#### `update`

- 参数

    - int 或 float
    - 可选

- 手动更新进度条信息

#### `close`

- 清除并关闭进度条

#### `clear`

- 清除当前显示的进度条

#### `refresh`

- 强制刷新当前的进度条
- 参数

    - nolock

        - bool

            - True

                - 不锁

            - False

                - 调用内部的函数acquire

        - 可选

    - lock_args

        - tuple
        - 可选
        - 传入到内部的函数acquire

            - 指定之后，只有acquire返回True才会显示进度条

#### ``unpause``

- 重新启动上一次打印时tqdm的计数器

#### `reset`

- 参数

    - total

        - int or float
        - 可选
        - 新的进度条的total

- 重新指定total为0以重复利用

#### `set_description`

- 参数

    - desc
    - refresh

        - 强制刷新

- 设置/修改进度条的提示，字符串后自动添加 “:”

#### `set_description_str`

- desc
- refresh
- 设置/修改进度条的提示，字符串后不添加 “:”

#### `set_postfix`

- 参数

    - ordered_dict
    - refresh
    - **kwargs

- 设置/修改进度条后的提示信息

#### `set_postfix_str`

- 参数

    - s
    - refresh

- 以字符串的形式直接显示信息
	

