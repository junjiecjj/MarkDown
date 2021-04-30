

<center><font face="黑体" color=blue size=8>C语言</font></center>



# 基础数据结构

## 位字段



### 位域是什么？

有些数据在存储时并不需要占用一个完整的字节，只需要占用一个或几个二进制位即可。例如开关只有通电和断电两种状态，用 0 和 1 表示足以，也就是用一个二进位。正是基于这种考虑，C语言又提供了一种叫做位域的数据结构。

在结构体定义时，我们可以指定某个成员变量所占用的二进制位数（Bit），这就是位域。请看下面的例子：

```c
struct bs{
    unsigned m;
    unsigned n: 4;
    unsigned char ch: 6;
};
```

`:`后面的数字用来限定成员变量占用的位数。成员 m 没有限制，根据数据类型即可推算出它占用 4 个字节（Byte）的内存。成员 n、ch 被:后面的数字限制，不能再根据数据类型计算长度，它们分别占用 4、6 位（Bit）的内存。

n、ch 的取值范围非常有限，数据稍微大些就会发生溢出，请看下面的例子：

```c
 #include <stdio.h>
 int main(){
     struct bs{
         unsigned m;
         unsigned n: 4;
         unsigned char ch: 6;
     } a = { 0xad, 0xE, '$'};
     //第一次输出
     printf("%#x, %#x, %c\n", a.m, a.n, a.ch);
    //更改值后再次输出
    a.m = 0xb8901c;
    a.n = 0x2d;
    a.ch = 'z';
    printf("%#x, %#x, %c\n", a.m, a.n, a.ch);
    system("pause");
    return 0;
}
```

运行结果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/wqfIPAmgib2VmW31bpoNMCzIG8CuMnqvs4JM9IH4HSauibJibza8H8EVIEZwm5qBBPwM64Lib21RPkA0KkUeibXfQPQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

对于 n 和 ch，第一次输出的数据是完整的，第二次输出的数据是残缺的。

第一次输出时，n、ch 的值分别是 0xE、0x24（'$' 对应的 ASCII 码为 0x24），换算成二进制是 `1110`、`10 0100`，都没有超出限定的位数，能够正常输出。

第二次输出时，n、ch 的值变为 0x2d、0x7a（'z' 对应的 ASCII 码为 0x7a），换算成二进制分别是 `10 1101`、`111 1010`，都超出了限定的位数。超出部分被直接截去，剩下 `1101`、`11 1010`，换算成十六进制为 0xd、0x3a（0x3a 对应的字符是 :）。

> C语言标准规定，位域的宽度不能超过它所依附的数据类型的长度。通俗地讲，成员变量都是有类型的，这个类型限制了成员变量的最大长度，:后面的数字不能超过这个长度。

例如上面的 bs，n 的类型是 unsigned int，长度为 4 个字节，共计 32 位，那么 n 后面的数字就不能超过 32；ch 的类型是 unsigned char，长度为 1 个字节，共计 8 位，那么 ch 后面的数字就不能超过 8。

我们可以这样认为，位域技术就是在成员变量所占用的内存中选出一部分位宽来存储数据。

> C语言标准还规定，只有有限的几种数据类型可以用于位域。在 ANSI C 中，这几种数据类型是 int、signed int 和 unsigned int（int 默认就是 signed int）；到了 C99，_Bool 也被支持了。

但编译器在具体实现时都进行了扩展，额外支持了 char、signed char、unsigned char 以及 enum 类型，所以上面的代码虽然不符合C语言标准，但它依然能够被编译器支持。

### 位域的存储

C语言标准并没有规定位域的具体存储方式，不同的编译器有不同的实现，但它们都尽量压缩存储空间。

**位域的具体存储规则如下：**

1. 当相邻成员的类型相同时，如果它们的位宽之和小于类型的 sizeof 大小，那么后面的成员紧邻前一个成员存储，直到不能容纳为止；如果它们的位宽之和大于类型的 sizeof 大小，那么后面的成员将从新的存储单元开始，其偏移量为类型大小的整数倍。

以下面的位域 bs 为例：

```c
 #include <stdio.h>
 int main(){
     struct bs{
         unsigned m: 6;
         unsigned n: 12;
         unsigned p: 4;
     };
     printf("%d\n", sizeof(struct bs));
     return 0;
}
```

运行结果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

m、n、p 的类型都是 unsigned int，sizeof 的结果为 4 个字节（Byte），也即 32 个位（Bit）。m、n、p 的位宽之和为`6+12+4 = 22`，小于 32，所以它们会挨着存储，中间没有缝隙。

`sizeof(struct bs)`的大小之所以为 4，而不是 3，是因为要将内存对齐到 4 个字节，以便提高存取效率。

如果将成员 m 的位宽改为 22，那么输出结果将会是 8，因为`22+12 = 34`，大于 32，n 会从新的位置开始存储，相对 m 的偏移量是 `sizeof(unsigned int)`，也即 4 个字节。

如果再将成员 p 的位宽也改为 22，那么输出结果将会是 12，三个成员都不会挨着存储。

1. 当相邻成员的类型不同时，不同的编译器有不同的实现方案，GCC 会压缩存储，而 VC/VS 不会。

请看下面的位域 bs：

```c
 #include <stdio.h>
 int main(){
     struct bs{
         unsigned m: 12;
         unsigned char ch: 4;
         unsigned p: 4;
     };
     printf("%d\n", sizeof(struct bs));
     return 0;
}
```

在 GCC 下的运行结果为 4，三个成员挨着存储；在 VC/VS 下的运行结果为 12，三个成员按照各自的类型存储（与不指定位宽时的存储方式相同）。

m 、ch、p 的长度分别是 4、1、4 个字节，共计占用 9 个字节内存，为什么在 VC/VS 下的输出结果却是 12 呢？期待您的回复。

1. 如果成员之间穿插着非位域成员，那么不会进行压缩。例如对于下面的 bs：

```c
struct bs{
    unsigned m: 12;
    unsigned ch;
    unsigned p: 4;
};
```

在各个编译器下 sizeof 的结果都是 12。

通过上面的分析，我们发现位域成员往往不占用完整的字节，有时候也不处于字节的开头位置，因此使用&获取位域成员的地址是没有意义的，C语言也禁止这样做。地址是字节（Byte）的编号，而不是位（Bit）的编号。

### 无名位域

位域成员可以没有名称，只给出数据类型和位宽，如下所示：

```c
struct bs{
    int m: 12;
    int  : 20;  //该位域成员不能使用
    int n: 4;
};
```

无名位域一般用来作填充或者调整成员位置。因为没有名称，无名位域不能使用。

上面的例子中，如果没有位宽为 20 的无名成员，m、n 将会挨着存储，`sizeof(struct bs)` 的结果为 4；有了这 20 位作为填充，m、n 将分开存储，`sizeof(struct bs)` 的结果为 8。

