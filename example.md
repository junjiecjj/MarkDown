[TOC]



# 1. 一级标题  \#\#

正常字体哈哈哈哈哈哈哈哈哈哈或或或

*这是斜体斤斤计较军军军军军军军*

<u>下划线咔咔咔咔咔咔扩扩扩扩</u>

~~删除线少时诵诗书所所所所所所所所~~

<!--注释哒哒哒哒哒哒多多多多多多多多-->

[百度网址](www.baidu.com)

插入公式：

1. 这样的代码可以生成如$x^n+y^n=z^n$这样的行内公式。

2. 这样的代码可以生成如$$x^n+y^n=z^n$$这样的块级公式。

3. 这样的代码可以生成如$$x^n+y^n=z^n \tag{1.1}$$的编号块级公式。

4. 这样的代码可以生成如

$$
\begin{equation}

x^n+y^n=z^n

\end{equation}
$$

的自动编号块级公式。

5. 有时候需要罗列多个公式，可以用 eqnarray* 标签包围公式代码，在需要转行的地方使用 \\，每行需要使用 2 个 & 来标识对齐位置，两个 &...& 号之间的是公式间对齐的位置，每行公式后可使用 \tag{...} 标签编号：

$$
\begin{eqnarray*}

x^n+y^n &=& z^n \tag{1.4} \\

x+y &=& z \tag{1.5}

\end{eqnarray*}
$$



不编号：

$\sum_{i=1}^{N} = f(x)+g(x)^2    $

  编号：
$$
\begin{equation}
th^* =   \mathop{\arg\max}_{0<th<1}  \mathrm{AUC}(\mathrm{TPR}(th, {\Delta t}_\mathrm{alarm}),\mathrm{FPR}(th, {\Delta t}_\mathrm{alarm})) \tag{equ4}
\end{equation}
$$

$$
\begin{equation}
n_\mathrm{GW}(10^{20}\mathrm{m}^{-3}) = \frac{I_\mathrm{p}(\mathrm{MA})}{\pi a^{2}(\mathrm{m}^{2})}~,
\end{equation}
$$


发发发发发^上标^

哒哒哒哒哒哒~下标下标~

==高亮文本==

**加粗字体**

[^1]: 这是注脚

有序列表:

1. 第一：
2. 第二
3. 第三

无序列表：

- 第一
- 第二

任务列表：

- [ ] 任务列表1
- [ ] 任务列表2
- [ ] 任务列表3

无序列表

+ 第一
+ 第二
+ 第三









```c
#include<stdio.h>
int main()
{
    printf("hello, world");
}
```



##  2. 二级标题

水平分割线

------

水平分割线





控制文字属性：

<font face="黑体">我是黑体字</font>

<font face="Time New Roman">我是新罗马字体</font>

<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#008000>我是绿色</font>

==字体标记==

<font color=Blue>我是蓝色</font>

<font size=5>我是尺寸</font>



<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>

```
<center>这一行需要居中</center>
```



<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>

1. 颜色：

浅红色文字：<font color="#dd0000">浅红色文字：</font> 
深红色文字：<font color="#660000">深红色文字</font> 
浅绿色文字：<font color="#00dd00">浅绿色文字</font> 
深绿色文字：<font color="#006600">深绿色文字</font> 
浅蓝色文字：<font color="#0000dd">浅蓝色文字</font> 
深蓝色文字：<font color="#000066">深蓝色文字</font>
浅黄色文字：<font color="#dddd00">浅黄色文字</font> 
深黄色文字：<font color="#666600">深黄色文字</font> 
浅青色文字：<font color="#00dddd">浅青色文字</font> 
深青色文字：<font color="#006666">深青色文字</font> 
浅紫色文字：<font color="#dd00dd">浅紫色文字</font> 
深紫色文字：<font color="#660066">深紫色文字</font> 

2. 大小：

size为1：<font size="1">size为1</font> 
size为2：<font size="2">size为2</font> 
size为3：<font size="3">size为3</font> 
size为4：<font size="4">size为4</font> 
size为10：<font size="10">size为10</font> 

3. 字体：

+ <font face="黑体">我是黑体字</font>
+ <font face="宋体">我是宋体字</font>
+ <font face="微软雅黑">我是微软雅黑字</font>
+ <font face="fantasy">我是fantasy字</font>
+ <font face="Helvetica">我是Helvetica字</font>

4. 背景色

<table><tr><td bgcolor=#FF00FF>背景色的设置是按照十六进制颜色值：#7FFFD4</td></tr></table>
<table><tr><td bgcolor=#FF83FA>背景色的设置是按照十六进制颜色值：#FF83FA</td></tr></table>
<table><tr><td bgcolor=#D1EEEE>背景色的设置是按照十六进制颜色值：#D1EEEE</td></tr></table>
<table><tr><td bgcolor=#C0FF3E>背景色的设置是按照十六进制颜色值：#C0FF3E</td></tr></table>
<table><tr><td bgcolor=#54FF9F>背景色的设置是按照十六进制颜色值：#54FF9F</td></tr></table>

5. 居中、左对齐、右对齐：

居中：
<center>月是故乡明</center>
左对齐：
<p align="left">月是故乡明</p>
右对齐：

<p align="right">月是故乡明</p>






###  3. 三级标题







#### 4. 四级标题



##### 5. 五级标题

插入代码：

```c
#include <stdio.h>
#include <stdlib.h>
#include <float.h>
#include <limits.h>
#include <math.h>
#include <string.h>
#include <sys/socket.h>
#include <stddef.h>
#include <locale.h>
#include <time.h>
#include <complex.h>
#include <netdb.h>
#include <arpa/inet.h>

int main(int argc, char *argv[])
{
    struct hostent *host;

    host = gethostbyname("www.baidu.com");

    // 主机的规范名
    printf("h_name=%s\n", host->h_name);

    //别名
    for (int i = 0; host->h_aliases[i]; i++)
    {
        printf("Aliases %d: %s\n", i + 1, host->h_aliases[i]);
    }

    //IP地址类型
    printf("Address type: %s\n", (host->h_addrtype == AF_INET) ? "AF_INET" : "AF_INET6");
    printf("addrtype=%d\n", host->h_addrtype);

    // IP 地址
    for (int i = 0; host->h_addr_list[i]; i++)
    {
        //将IP指针转换为 in_addr 结构体, 再调用inet_ntoa转换为字符串形式
        printf("Ip addr: %s\n", inet_ntoa(*(struct in_addr *)host->h_addr_list[i]));
    }
}

```

插入一张图片，原图：

![图2](1.png)

缩放70%大小。

<img src="1.png" alt="图1" style="zoom:70%;" />



$$
\sum_{i=0}^{N} = e^3*x^2 
$$


再来一次，缩放30%大小。

<img src="C:\文档\Markdown\1.png" alt="图2" style="zoom:33%;" />



。。。。。



