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





1. 控制文字属性：

正常字体哈哈哈哈哈哈哈哈哈哈或或或

*这是斜体斤斤计较军军军军军军军*

<u>下划线咔咔咔咔咔咔扩扩扩扩</u>

~~删除线少时诵诗书所所所所所所所所~~

<!--注释哒哒哒哒哒哒多多多多多多多多-->

超链接：[百度网址](www.baidu.com)

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



2. <table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>

3. 颜色：

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

4. 大小：

size为1：<font size="1">size为1</font> 
size为2：<font size="2">size为2</font> 
size为3：<font size="3">size为3</font> 
size为4：<font size="4">size为4</font> 
size为10：<font size="10">size为10</font> 

5. 字体：

+ <font face="黑体">我是黑体字</font>
+ <font face="宋体">我是宋体字</font>
+ <font face="微软雅黑">我是微软雅黑字</font>
+ <font face="fantasy">我是fantasy字</font>
+ <font face="Helvetica">我是Helvetica字</font>

6.背景色

<table><tr><td bgcolor=#FF00FF>背景色的设置是按照十六进制颜色值：#7FFFD4</td></tr></table>
<table><tr><td bgcolor=#FF83FA>背景色的设置是按照十六进制颜色值：#FF83FA</td></tr></table>
<table><tr><td bgcolor=#D1EEEE>背景色的设置是按照十六进制颜色值：#D1EEEE</td></tr></table>
<table><tr><td bgcolor=#C0FF3E>背景色的设置是按照十六进制颜色值：#C0FF3E</td></tr></table>
<table><tr><td bgcolor=#54FF9F>背景色的设置是按照十六进制颜色值：#54FF9F</td></tr></table>

7. 居中、左对齐、右对齐：

+ 居中：

<center>月是故乡明</center>
+ 左对齐：

<p align="left">月是故乡明</p>
+ 右对齐：

<p align="right">月是故乡明</p>

+ 颜色、字体、尺寸、居中

<center><font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5，这一行需要居中</font></center>




###  3. 三级标题



超链接有以下几种方式实现，分别阐述：

1. 超链接 Markdown 语法代码,内联方式：\[超链接显示名\]\(超链接地址 "超链接title"\)，渲染如下：
   + 这是一个链接 [Markdown语法](https://markdown.com.cn).

2. 引用方式：
   + I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3]. 

[1] http://google.com/

[2] http://search.yahoo.com/

[3] http://search.msn.com/

3. 给链接增加 Title，链接 title 是当鼠标悬停在链接上时会出现的文字，这个 title 是可选的，它放在圆括号中链接地址后面，跟链接地址之间以空格分隔。这是一个链接 \[Markdown语法\]\(https://markdown.com.cn "最好的markdown教程"\)，渲染如下：

   + 这是一个链接 [Markdown语法](https://markdown.com.cn "最好的markdown教程")。

4. 网址和 Email 地址,使用尖括号可以很方便地把 URL 或者 email 地址变成可点击的链接。

   + <https://markdown.com.cn>

   + <fake@example.com>

5. 带格式化的链接,强调链接，在链接语法前后增加星号。 要将链接表示为代码，请在方括号中添加反引号。

   + I love supporting the **[EFF](https://eff.org)**.
   + This is the *[Markdown Guide](https://www.markdownguide.org)*.
   + See the section on [`code`](#code).

6. 引用类型链接
    引用样式链接是一种特殊的链接，它使 URL 在 Markdown 中更易于显示和阅读。参考样式链接分为两部分：与文本保持内联的部分以及存储在文件中其他位置的部分，以使文本易于阅读。
**==链接的第一部分格式==**

    引用类型的链接的第一部分使用两组括号进行格式设置。第一组方括号包围应显示为链接的文本。第二组括号显示了一个标签，该标签用于指向您存储在文档其他位置的链接。

    尽管不是必需的，可以在第一组和第二组括号之间包含一个空格。第二组括号中的标签不区分大小写，可以包含字母，数字，空格或标点符号。

    以下示例格式对于链接的第一部分效果相同：

    - [hobbit-hole][1]
    - [hobbit-hole] [1]

    **==链接的第二部分格式==** 

    引用类型链接的第二部分使用以下属性设置格式：

- 放在括号中的标签，其后紧跟一个冒号和至少一个空格（例如 `[label]:`）。

- 链接的 URL，可以选择将其括在尖括号中。

- 链接的可选标题，可以将其括在双引号，单引号或括号中。

  以下示例格式对于链接的第二部分效果相同：

  - [1]:https://en.wikipedia.org/wiki/Hobbit#Lifestyle

  - [1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"

  - [1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"

  - [1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"

  - [1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"

  - [1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"

  - [1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"

可以将链接的第二部分放在 Markdown 文档中的任何位置。有些人将它们放在出现的段落之后，有些人则将它们放在文档的末尾（例如尾注或脚注）。










#### 4. 四级标题



表格：

Markdown 制作表格使用 **|** 来分隔不同的单元格，使用 **-** 来分隔表头和其他行。

| 表头   | 表头   |
| ------ | ------ |
| 单元格 | 单元格 |
| 单元格 | 单元格 |



表格背景色：

<table><tbody>
    <tr>
        <th>方法说明</th><th>颜色名称</th><th>颜色</th>
    </tr>
    <tr>
        <td><font color="Hotpink">此处实现方法利用 CSDN-markdown 内嵌 html 语言的优势</font></td><td><font color="Hotpink">Hotpink</font></td><td bgcolor="Hotpink">rgb(240, 248, 255)</td>
    </tr>
    <tr>
        <td><font color="Pink">借助 table, tr, td 等表格标签的 bgcolor 属性实现背景色设置</font></td><td><font color="pink">AntiqueWhite</font></td><td bgcolor="Pink">rgb(255, 192, 203)</td>
    </tr>
</table>



跨行表格：

<table><tbody>
    <tr>
        <th rowspan="3">我占了三行</th>
        <th>第一列</th>
        <th>第二列</th>
        <th>第三列</th>
    </tr>
    <tr>
        <td>第一列</td>
        <td>第二列</td>
        <td>第三列</td>
    </tr>
    <tr>
        <td>第一列</td>
        <td>第二列</td>
        <td>第三列</td>
    </tr>
</table>  











对齐方式

**我们可以设置表格的对齐方式：**

- **-:** 设置内容和标题栏居右对齐。
- **:-** 设置内容和标题栏居左对齐。
- **:-:** 设置内容和标题栏居中对齐。

实例如下：

| 左对齐 | 右对齐 | 居中对齐 |
| :----- | -----: | :------: |
| 单元格 | 单元格 |  单元格  |
| 单元格 | 单元格 |  单元格  |



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



