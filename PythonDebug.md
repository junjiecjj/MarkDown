#   介绍



本文档主要讲述Python程序性能分析，包括:

+ 内存分析
  + memory_profiler， 它能以图的形式展示 RAM 的使用情况随时间的变化,这样你就可以向你的同事们解释为什么某个函数占用了比预期更多的 RAM。
+ CPU分析
  +  cProfile , 告诉你如何使用这个内建工具来了解代码中哪些函数耗时最长。这将让你站在高处俯瞰你的问题,使你能够将注意力集中到关键函数上。
  +  line_profiler , 这个工具能够对你选定的函数进行逐行分析。其结果将包含每行被调用的次数以及每行花费的时间百分比。这恰能让你知道是哪里跑得慢以及为什么。
  +  perf stat 命令来了解最终执行于 CPU上的指令的个数以及 CPU 缓存的利用率。

+ 调试
  + pdb
  + 

+ 引用计数分析





## CPU 分析



### cProfile

[Python自带的调试及性能分析神器](https://cloud.tencent.com/developer/article/1752482)

**cProfile介绍**

- cProfile自python2.5以来就是标准版Python解释器默认的性能分析器。
- 其他版本的python，比如PyPy里没有cProfile的。
- cProfile是一种确定性分析器，只测量CPU时间，并不关心内存消耗和其他与内存相关联的信息。

` python -m cProfile -s cumulative julia1_nopil.py`

-s cumulative 开关告诉 cProfile 对每个函数累计花费的时间进行排序,这能让我们看到代码最慢的部分

用 runsnakerun 对 cProfile 的输出进行可视化

输入命令 pip install runsnake 来安装 runsnake。



除了要对程序进行调试，性能分析也是每个开发者的必备技能。日常工作中，我们常常会遇到这样的问题：在线上，我发现产品的某个功能模块效率低下，延迟（latency）高，占用的资源多，但却不知道是哪里出了问题。这时，对代码进行 profile 就显得异常重要了。

这里所谓的 profile，是指对代码的每个部分进行动态的分析，比如准确计算出每个模块消耗的时间等。这样你就可以知道程序的瓶颈所在，从而对其进行修正或优化。当然，这并不需要你花费特别大的力气，在 Python 中，这些需求用 cProfile 就可以实现。

举个例子，比如我想计算斐波拉契数列，运用递归思想，我们很容易就能写出下面这样的代码:

```python
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)

def fib_seq(n):
    res = []
    if n > 0:
        res.extend(fib_seq(n-1))
    res.append(fib(n))
    return res

print(fib_seq(30))
```

接下来，我想要测试一下这段代码总的效率以及各个部分的效率。那么，我就只需在开头导入 cProfile 这个模块，并且在最后运行 cProfile.run() 就可以了，如下所示：

```python
import cProfile
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)

def fib_seq(n):
    res = []
    if n > 0:
        res.extend(fib_seq(n-1))
    res.append(fib(n))
    return res

cProfile.run('fib_seq(30)')
```

或者更简单一些，直接在运行脚本的命令中，加入选项“-m cProfile”也很方便：

```javascript
python -m cProfile test.py
```

运行结果如下:

```python
(py37env) ➜  ~ python test.py
         7049218 function calls (96 primitive calls) in 1.503 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    1.503    1.503 <string>:1(<module>)
     31/1    0.000    0.000    1.503    1.503 test.py:10(fib_seq)
7049123/31    1.503    0.000    1.503    0.048 test.py:2(fib)
        1    0.000    0.000    1.503    1.503 {built-in method builtins.exec}
       31    0.000    0.000    0.000    0.000 {method 'append' of 'list' objects}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
       30    0.000    0.000    0.000    0.000 {method 'extend' of 'list' objects}
```

这里有一些参数你可能比较陌生，我来简单介绍一下：

- ncalls，是指相应代码 / 函数被调用的次数；
- tottime，是指对应代码 / 函数总共执行所需要的时间（注意，并不包括它调用的其他代码 / 函数的执行时间）；
- tottime percall，就是上述两者相除的结果，也就是tottime / ncalls；
- cumtime，则是指对应代码 / 函数总共执行所需要的时间，这里包括了它调用的其他代码 / 函数的执行时间；
- cumtime percall，则是 cumtime 和 ncalls 相除的平均结果。

了解这些参数后，再来看运行结果。我们可以清晰地看到，这段程序执行效率的瓶颈，在于第二行的函数 fib()，它被调用了 700 多万次。

有没有什么办法可以提高改进呢？答案是肯定的。通过观察，我们发现，程序中有很多对 fib() 的调用，其实是重复的，那我们就可以用字典来保存计算过的结果，防止重复。改进后的代码如下所示：

```python
def memoize(f):
    memo = {}
    def helper(x):
        if x not in memo:
            memo[x] = f(x)
        return memo[x]
    return helper

@memoize
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)


def fib_seq(n):
    res = []
    if n > 0:
        res.extend(fib_seq(n-1))
    res.append(fib(n))
    return res

fib_seq(30)
```

上述代码保存为 test2.py，直接在命令行执行，结果如下：

```python
(py37env) ➜  ~ python -m cProfile test2.py
         216 function calls (128 primitive calls) in 0.000 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 test2.py:1(<module>)
        1    0.000    0.000    0.000    0.000 test2.py:1(memoize)
     31/1    0.000    0.000    0.000    0.000 test2.py:19(fib_seq)
    89/31    0.000    0.000    0.000    0.000 test2.py:3(helper)
       31    0.000    0.000    0.000    0.000 test2.py:9(fib)
        1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
       31    0.000    0.000    0.000    0.000 {method 'append' of 'list' objects}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
       30    0.000    0.000    0.000    0.000 {method 'extend' of 'list' objects}
```





### line_profiler

[python 性能调试工具（line_profiler）使用](https://blog.csdn.net/guofangxiandaihua/article/details/77825524)

[line_profiler - python性能分析利器](https://zhuanlan.zhihu.com/p/88193562)

[使用line_profiler对python代码性能进行评估优化](https://cloud.tencent.com/developer/article/1827356)



line_profiler 是调查 Python 的 CPU 密集型性能问题最强大的工具。它可以对函数进行逐行分析,你应该先用 cProfile 找到需要分析的函数,然后用 line_profiler 对函数进行分析。

有一点需要注意的是，line_profiler所能够分析的范围仅限于加了装饰器的函数内容，如果函数内有其他的调用之类的，不会再进入其他的函数进行分析，除了内嵌的嵌套函数。

输入命令 pip install line_profiler 来安装 line_profiler。

两种使用方法：



<center><font face="黑体" color=green size=5>第一种：</font></center>

先用修饰器(@profile)标记选中的函数。

在终端中：`kernprof -l aaa.py`

运行时参数`-l` 代表逐行分析而不是逐函数分析,  `-v` 用于显示输出。没有`-v`,你会得到一个`.lprof` 的输出文件,回头你可以用 line_profiler 模块对其进行分析。

kernprof默认情况下会把分析结果写入xxx.py.lprof文件，不过可以用-v显示在命令行里。

```python
kernprof -l -v line_profiler_test.py
```

```python
# line_profiler_test.py
from line_profiler import LineProfiler
import numpy as np

@profile
def test_profiler():
    for i in range(100):
        a = np.random.randn(100)
        b = np.random.randn(1000)
        c = np.random.randn(10000)
    return None

if __name__ == '__main__':
    test_profiler()
```



<center><font face="黑体" color=green size=5>第二种：</font></center>



在代码中加入相关函数，把需要测试的函数包裹起来，然后：

`kernprof -l -v line_profiler_test.py`

```python
from line_profiler import LineProfiler
import random
 
def do_stuff(numbers):
    s = sum(numbers)
    l = [numbers[i]/43 for i in range(len(numbers))]
    m = ['hello'+str(numbers[i]) for i in range(len(numbers))]
 
numbers = [random.randint(1,100) for i in range(1000)]
lp = LineProfiler()
lp_wrapper = lp(do_stuff)
lp_wrapper(numbers)
lp.print_stats()
```

输出：

```python
❯ kernprof -l  -v line_profiler1.py
Timer unit: 1e-06 s

Total time: 0.000849 s
File: line_profiler1.py
Function: do_stuff at line 14

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    14                                           def do_stuff(numbers):
    15         1         12.0     12.0      1.4      s = sum(numbers)
    16         1        273.0    273.0     32.2      l = [numbers[i]/43 for i in range(len(numbers))]
    17         1        564.0    564.0     66.4      m = ['hello'+str(numbers[i]) for i in range(len(numbers))]

Wrote profile results to line_profiler1.py.lprof
Timer unit: 1e-06 s

```

为了能够同时显示函数每行所用时间和调用函数每行所用时间，加入add_function就能够解决。

```python
from line_profiler import LineProfiler
import random
 
def do_other_stuff(numbers):
    s = sum(numbers)
 
def do_stuff(numbers):
    do_other_stuff(numbers)
    l = [numbers[i]/43 for i in range(len(numbers))]
    m = ['hello'+str(numbers[i]) for i in range(len(numbers))]
 
numbers = [random.randint(1,100) for i in range(1000)]
lp = LineProfiler()
lp.add_function(do_other_stuff)   # add additional function to profile
lp_wrapper = lp(do_stuff)
lp_wrapper(numbers)
lp.print_stats()
```

输出：

```python
❯ kernprof -l  -v line_profiler5.py
Timer unit: 1e-06 s

Total time: 7e-06 s
File: line_profiler5.py
Function: do_other_stuff at line 14

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    14                                           def do_other_stuff(numbers):
    15         1          7.0      7.0    100.0      s = sum(numbers)

Total time: 0.000478 s
File: line_profiler5.py
Function: do_stuff at line 17

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    17                                           def do_stuff(numbers):
    18         1         11.0     11.0      2.3      do_other_stuff(numbers)
    19         1        158.0    158.0     33.1      l = [numbers[i]/43 for i in range(len(numbers))]
    20         1        309.0    309.0     64.6      m = ['hello'+str(numbers[i]) for i in range(len(numbers))]

Wrote profile results to line_profiler5.py.lprof
Timer unit: 1e-06 s

```





<center><font face="黑体" color=green size=5>第三种：</font></center>





line_profiler和cProfile一样：也提供了run，runctx，runcall，enable，disable方法，但是后两个并不安全，可以用dump_stats(filename)方法把stats加载到文件中，也可以用print_stats([stream])昂发打印结果。

------------------------------------------------
```python
# coding=utf-8
import line_profiler
import sys

def bbb():
    for i in range(0, 3):
        print i**2
    print 'end'

profile = line_profiler.LineProfiler(bbb)  # 把函数传递到性能分析器
profile.enable()  # 开始分析
bbb()
profile.disable()  # 停止分析
profile.print_stats(sys.stdout)  # 打印出性能分析结果
```

但是这种不好用，建议第一第二种。





## 内存分析

### memory_profiler

用命令 `pip install memory_profiler` 来安装 `memory_profiler` (可选装` pip install psutil`)。

用修饰器(@profile)标记选中的函数。

` python -m memory_profiler julia1_memoryprofiler.py`





### Memray

Memray是一个由彭博社开发的、开源内存剖析器；开源一个多月，已经收获了超8.4k的star，是名副其实的明显项目。今天我们就给大家来推荐这款python内存分析神器。

Memray可以跟踪python代码、本机扩展模块和python解释器本身中内存分配，可以生成多种不同类型的报告，帮助您分析python代码内存使用情况。

- 工具的主要特点：   
  - 跟踪每个函数的调用，能够准确的跟踪调用栈
  - 能跟踪c/c++库的调用
  - 分析速度很快
  - 收集内存数据，输出各种图标
  - 使用python线程
  - 与本地线程一起工作
- 可以帮助解决的问题：   
  - 分析应用程序中内存分配，发现高内存使用率的原因
  - 查找内存泄漏的原因
  - 查找导致内存大量分配的代码热点

memray安装

- 环境要求：python3.7+以上版本，linux系统（仅支持linux系统）
- 安装：`pip3 install memray`



memray使用帮助

```python
python3 -m memray --help
```

| 参数       | 作用                                           |
| ---------- | ---------------------------------------------- |
| run        | 运行指定的应用程序并跟踪内存使用情况           |
| flamegraph | 在html报告中，用火焰图方式，显示内存使用情况   |
| table      | 在html报告文件中，用表格的方式显示内存分析情况 |
| live       | 用实时屏幕显示方式，显示各种内存使用情况       |
| tree       | 在终端中，用树形结构显示内存使用情况           |
| parse      | 用debug模式，显示每一行的内存使用情况          |
| summary    | 汇总终端运行期间的内存使用概况                 |
| stats      | 在终端中非常详细的显示内存使用情况             |



run命令使用

- `python3 -m memray run --help` 获取帮助



| 参数                                | 作用                              |
| ----------------------------------- | --------------------------------- |
| -o OUTPU,--output OUTPUT            | 指定输出结果到哪里                |
| --live                              | 启动实时跟踪会话模式              |
| --live-remote                       | 启动实时跟踪会话并等待客户端连接  |
| --live-port LIVE_PORT, -p LIVE_PORT | 启动实时跟踪时要使用的端口        |
| --native                            | 跟踪C/C++堆栈                     |
| --follow-fork                       | 跟踪脚本分叉的子进程中的分配      |
| --trace-python-allocators           | 记录pymalloc分配器的分配情况      |
| -q, --quiet                         | 运行时不显示任何特定于跟踪的输出  |
| -f, --force                         | 强制复购已有文件                  |
| --compress-on-exit                  | 跟踪完成后使用 lz4 压缩生成的文件 |
| --no-compress                       | 不使用 lz4 压缩生成的文件         |
| -c                                  | 作为字符串传入的程序              |
| -m                                  | 将库模块作为脚本运行              |

- `python3 -m memray run xxx.py` 直接分析某个py文件的内存使用情况，就会在当前路径下生成一个 ‘memray-py文件名.进程id.bin’ 的内存使用记录文件。当然，也可以跟上`-o outFiPath` 指定输出路径。如果运行的py文件是模块代码，也可以使用`-m xxx.py` 方式运行。
- ‘memray-py文件名.进程id.bin’ 文件，可以通过 `python3 -m memray flamegraph memray-py文件名.进程id.bin` 转换为一份html-火焰图报告.
  + 如上图，从上往下，显示了程序的调用过程，宽度，代表函数占用内存多少。

- `python3 -m memray run --native xxxx.py` 会跟踪分析python代码中调用底层的C/C++函数消耗的内存情况

- `python3 -m memray run --trace-python-allocators xxx.py` 跟踪分析python程序内存分配器pymalloc的情况.

  + 这个看上去，和没有加参数，效果差不多，但是，实际上是完全不一样的。这种方式，会深入跟踪内存分配，python常见的内存分配器有四种（malloc、free、realloc、pymalloc），这个参数，在python出现内存溢出时，就非常有用了。但是，加了这个参数，运输速度会变慢，收集的数据生成的文件会更大。

- `python3 -m memray run --live xxx.py` 用实时屏幕模式显示跟踪的内存数据。

  + 默认时，根据Total memory的数据从大到小，往下排列；按"O"，可以根据私有内存从大到小，排序显示内存对象；按“A”，则根据内存分配次数量从高到底排序。

    有了这个统计数据，就能快速定位到哪些对象，占用内存大，哪些对象被频繁的分配内存。这些对象，就是重点分析对象。

flamegraph命令---生成火焰图报告:

+ `python3 -m memray flamegraph --help` 获取帮助

+ `python3 -m memray flamegraph xxx.bin` 生成火焰图

table命令--生成表格报告

- `python3 -m memray table --help` 获取帮助
- `python3 -m memray table xxxx.bin` 把bin文件转换为表格报告

tree命令--生成树形报告

- `python3 -m memray tree --help` 获取帮助
- `python3 -m memray tree xxxx.bin` 把bin文件转换为树形报告

summary命令--生成概要报告

- `python3 -m memray summary --help` 获取帮助
- `python3 -m memray summary xxxx.bin` 对bin文件进行分析，生成概要报告

stats命令---生成详细统计报告

- `python3 -m memray stats --help` 获取帮助
- `python3 -m memray stats xxxx.bin` 对bin文件进行分析，生成详细报告



## PDB



[**调试和性能分析(一)、pdb模块**](https://blog.51cto.com/u_13977270/3398700)

[19 pdb & cProfile：调试和性能分析](https://www.jianshu.com/p/26f19ab65590)

[python高级调试技巧（一）——原生态的pdb调试](https://blog.csdn.net/qq_27825451/article/details/85600992)

Debug功能对于developer是非常重要的，python提供了相应的模块pdb让你可以在用文本编辑器写脚本的情况下进行debug. pdb是python debugger的简称。

常用的一些命令如下：

命令	               用途
break 或 b	    设置断点
continue 或 c	继续执行程序
list 或 l	          查看当前行的代码段
step 或 s	       进入函数
return 或 r	     执行代码直到从当前函数返回
exit 或 q	        中止并退出
next 或 n	       执行下一行
pp	                 打印变量的值
help	              帮助

```python
    1）进入命令行Debug模式，python -m pdb xxx.py  这个格式是固定的

          之所以可以这样做，主要是因为pdb.py 可以被当做一个脚本执行。

    2）h：（help）帮助

    3）w：（where）打印当前执行堆栈

    4）d：（down）执行跳转到在当前堆栈的深一层（个人没觉得有什么用处）

    5）u：（up）执行跳转到当前堆栈的上一层

    6）b：（break）添加断点

                 b 列出当前所有断点，和断点执行到统计次数

                 b line_number：当前脚本的line_no行添加断点

                 b filename:line_number：脚本filename的line_no行添加断点

                 b function：在函数function的第一条可执行语句处添加断点

    7）tbreak：（temporary break）临时断点

                 在第一次执行到这个断点之后，就自动删除这个断点，用法和b一样

    8）cl：（clear）清除断点

                cl 清除所有断点

                cl bpnumber1 bpnumber2... 清除断点号为bpnumber1,bpnumber2...的断点

                cl line_number 清除当前脚本line_number行的断点

                cl filename:line_number 清除脚本filename的line_number行的断点

    9）disable：停用断点，参数为bpnumber，和cl的区别是，断点依然存在，只是不启用

    10）enable：激活断点，参数为bpnumber(即哪一个断点，1，2，3，4......)

    11）s：（step）执行下一条命令

                如果本句是函数调用，则s会执行到函数的第一句

    12）n：（next）执行下一条语句

                如果本句是函数调用，则执行函数，接着执行当前执行语句的下一条。

    13）r：（return）执行当前运行函数到结束

    14）c：（continue）继续执行，直到遇到下一条断点，这个比较重要，常常和断点结合起来使用。

    15）l：（list）列出源码

                 l 列出当前执行语句周围11条代码

                 l first 列出first行周围11条代码

                 l first second 列出first--second范围的代码，如果second<first，second将被解析为行数

    16）a：（args）列出当前执行函数的函数

    17）p expression：（print）输出expression的值

           比如：(Pdb) p 1+2

                   这里会打印出 3

          再比如：(Pdb) p c    #这里用来查看某个变量c的值

    18）pp expression：好看一点的p expression

    19）run：重新启动debug，相当于restart

    20）q：（quit）退出debug

    21）j lineno：（jump）设置下条执行的语句函数

                只能在堆栈的最底层跳转，向后重新执行，向前可直接执行到行号

    22）unt：（until）执行到下一行（跳出循环），或者当前堆栈结束

    23）condition bpnumber conditon，给断点设置条件，当参数condition返回True的时候bpnumber断点有效，否则bpnumber断点无效
```

### 使用set_trace()设置断点



```python
import pdb
a = 1
b = 2
pdb.set_trace()
c = 3
print(a + b + c)
```

运行这个程序时，会在“pdb.set_trace()” 暂停，后面就可以通过上面的命令进行调试.

开始介绍如何使用pdb。



我们只需要在程序中，加入“import pdb”和“pdb.set_trace()”这两行代码就行了，然后用`python  test.py`跑程序，比如下面这个简单的例子：

```python
import pdb
for i in range(10000):
    print(i)
    if i == 800:
        pdb.set_trace()
```

当这个循环进行到 i==800 时，自动停下来进入命令行的调试，输入 i 即可查询变量的值，输入 n 表示执行下一行，输入 ll 查看上下文，输入 help 查看帮助。

其他命令：

- s 表示 step into，即进入相对应的代码内部。这时，命令行中会显示”--Call--“的字样，当你执行完内部的代码块后，命令行中则会出现”--Return--“的字样。
- r 表示 step out，即继续执行，直到当前的函数完成返回。
- b 可以用来设置断点。比方说，我想要在代码中的第 10 行，再加一个断点，那么在 pdb 模式下输入”b 11“即可。
- c 则表示一直执行程序，直到遇到下一个断点。

1 开始调试：

`[root@rcc-pok-idg-2255 ~]#  python epdb1.py`



使用n+enter表示执行当前的statement，在第一次按下了n+enter之后可以直接按enter表示重复执行上一条debug命令。如果您按ENTER键而不输入任何内容，pdb将重新执行您给出的最后一个命令。quit或者q可以退出当前的debug，但是quit会以一种非常粗鲁的方式退出程序，直接crash。

```python
[root@rcc-pok-idg-2255 ~]#  python epdb1.py
> /root/epdb1.py(4)?()
-> b = "bbb"
(Pdb) n
> /root/epdb1.py(5)?()
-> c = "ccc"
(Pdb) q
Traceback (most recent call last):
  File "epdb1.py", line 5, in ?
    c = "ccc"
  File "epdb1.py", line 5, in ?
    c = "ccc"
  File "/usr/lib64/python2.4/bdb.py", line 48, in trace_dispatch
    return self.dispatch_line(frame)
  File "/usr/lib64/python2.4/bdb.py", line 67, in dispatch_line
    if self.quitting: raise BdbQuit
bdb.BdbQuit
```



在使用过程中打印变量的值，可以直接使用p加上变量名，但是需要注意的是打印仅仅在当前的statement已经被执行了之后才能看到具体的值，否则会报 NameError: <exceptions.NameError 。。> 错误。

```python
[root@rcc-pok-idg-2255 ~]#  python epdb1.py
> /root/epdb1.py(4)?()
-> b = "bbb"
(Pdb) n
> /root/epdb1.py(5)?()
-> c = "ccc"
(Pdb) p b
'bbb'
(Pdb)
'bbb'
(Pdb) n
> /root/epdb1.py(6)?()
-> final = a + b + c
(Pdb) p c
'ccc'
(Pdb) p final
*** NameError: <exceptions.NameError instance at 0x1551b710>
(Pdb) n
> /root/epdb1.py(7)?()
-> print final
(Pdb) p final
'aaabbbccc'
(Pdb)
```



使用c可以停止当前的debug使得程序继续执行。如果在下面的程序中继续有set_statement()的申明，则又会重新进入到debug的状态。

```python
[root@rcc-pok-idg-2255 ~]#  python epdb1.py
> /root/epdb1.py(4)?()
-> b = "bbb"
(Pdb) n
> /root/epdb1.py(5)?()
-> c = "ccc"
(Pdb) c
aaabbbccc
```

可以在代码print final之前再加上set_trace()验证。

如果代码过程，在debug的时候不一定能记住当前的代码快，则可以通过使用list或者l命令在显示。list会用箭头->指向当前debug的语句

```python
[root@rcc-pok-idg-2255 ~]#  python epdb1.py
> /root/epdb1.py(4)?()
-> b = "bbb"
(Pdb) list
  1     import pdb
  2     a = "aaa"
  3     pdb.set_trace()
  4  -> b = "bbb"
  5     c = "ccc"
  6     final = a + b + c
  7     pdb.set_trace()
  8     print final
[EOF]
(Pdb) c
> /root/epdb1.py(8)?()
-> print final
(Pdb) list
  3     pdb.set_trace()
  4     b = "bbb"
  5     c = "ccc"
  6     final = a + b + c
  7     pdb.set_trace()
  8  -> print final
[EOF]
(Pdb)
```



对于使用函数的情况下进行debug：

```python
#  epdb2.py --
import pdb

def combine(s1,s2):      # define subroutine combine, which...
    s3 = s1 + s2 + s1    # sandwiches s2 between copies of s1, ...
    s3 = '"' + s3 +'"'   # encloses it in double quotes,...
    return s3            # and returns it.

a = "aaa"
pdb.set_trace()
b = "bbb"
c = "ccc"
final = combine(a,b)
print final
```



如果直接使用n进行debug则到final=combine这句的时候会将其当做普通的赋值语句处理，进入到print final。如果想要对函数进行debug如何处理？可以直接使用s进入函数块。

```python
[root@rcc-pok-idg-2255 ~]# python epdb2.py
> /root/epdb2.py(10)?()
-> b = "bbb"
(Pdb) n
> /root/epdb2.py(11)?()
-> c = "ccc"
(Pdb) n
> /root/epdb2.py(12)?()
-> final = combine(a,b)
(Pdb) s
--Call--
> /root/epdb2.py(3)combine()
-> def combine(s1,s2):      # define subroutine combine, which...
(Pdb) n
> /root/epdb2.py(4)combine()
-> s3 = s1 + s2 + s1    # sandwiches s2 between copies of s1, ...
(Pdb) list
  1     import pdb
  2
  3     def combine(s1,s2):      # define subroutine combine, which...
  4  ->     s3 = s1 + s2 + s1    # sandwiches s2 between copies of s1, ...
  5         s3 = '"' + s3 +'"'   # encloses it in double quotes,...
  6         return s3            # and returns it.
  7
  8     a = "aaa"
  9     pdb.set_trace()
 10     b = "bbb"
 11     c = "ccc"
(Pdb) n
> /root/epdb2.py(5)combine()
-> s3 = '"' + s3 +'"'   # encloses it in double quotes,...
(Pdb) n
> /root/epdb2.py(6)combine()
-> return s3            # and returns it.
(Pdb) n
--Return--
> /root/epdb2.py(6)combine()->'"aaabbbaaa"'
-> return s3            # and returns it.
(Pdb) n
> /root/epdb2.py(13)?()
-> print final
(Pdb)
```

如果不想在函数里单步调试可以在断点出直接按r退出到调用的地方。在调试的时候动态改变值 。注意下面有个错误，原因是b已经被赋值了，如果想重新改变b的赋值，则应该使用！b

```python
[root@rcc-pok-idg-2255 ~]# python epdb2.py
> /root/epdb2.py(10)?()
-> b = "bbb"
(Pdb) var = "1234"
(Pdb) b = "avfe"
*** The specified object '= "avfe"' is not a function
or was not found along sys.path.
(Pdb) !b="afdfd"
(Pdb)
```





pdb模块包含调试器。pdb包含一个类pdb，它继承自bdb.Bdb。调试器文档中提到了六个函数，它们创建了一个交互式调试会话:

登录后复制 

```python
pdb.run(statement[, globals[, locals]])
pdb.runeval(expression[, globals[, locals]])
pdb.runcall(function[, argument, ...])
pdb.set_trace()
pdb.post_mortem(traceback)

pdb.pm()
```



所有六个函数都提供了一种稍微不同的机制，用于将用户放入调试器。

`pdb.run(statement[, globals[, locals]])`
run()在调试器的控制下执行string语句。全局和本地字典是可选参数:

```python
#!/usr/bin/env python

import pdb

def test_debugger(some_int):
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

if __name__ == "__main__":
    pdb.run("test_debugger(0)")
```





pdb.runeval(expression[,globals[, locals]])
runeval()与pdb.run()相同，只是pdb.runeval()返回被求值的字符串表达式的值:

```python
#!/usr/bin/env python

import pdb

def test_debugger(some_int):
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

if __name__ == "__main__":
    pdb.runeval("test_debugger(0)")
```

pdb.runcall(function[,argument, ...])
runcall()调用指定的函数，并将指定的参数传递给它:

```python
#!/usr/bin/env python

import pdb

def test_debugger(some_int):
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

if __name__ == "__main__":
    pdb.runcall(test_debugger, 0)
```

pdb.set_trace()
set_trace()在执行时将代码放入调试器:

```python
#!/usr/bin/env python

import pdb

def test_debugger(some_int):
    pdb.set_trace()
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

if __name__ == "__main__":
    test_debugger(0)
```

pdb.post_mortem(traceback)
pdb.post_mortem()对特定的回溯进行事后调试:

```python
#!/usr/bin/env python

import pdb

def test_debugger(some_int):
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

if __name__ == "__main__":
    try:
        test_debugger(0)
    except:
        import sys
        tb = sys.exc_info()[2]
        pdb.post_mortem(tb)
```

pdb.pm()
pm()对sys.last_traceback中包含的回溯进行事后调试:

```python
#!/usr/bin/env python

import pdb
import sys

def test_debugger(some_int):
    print "start some_int>>", some_int
    return_int = 10 / some_int
    print "end some_int>>", some_int
    return return_int

def do_debugger(type, value, tb):
    pdb.pm()

if __name__ == "__main__":
    sys.excepthook = do_debugger
    test_debugger(0)
```

























