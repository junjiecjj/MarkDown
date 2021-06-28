[TOC]

<center><font face="黑体" color=blue size=8>C语言</font></center>



# 数据结构

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





# 算法

## 排序算法

### 冒泡





### 选择排序













#   Linux/C网络



##  Socket函数介绍与使用

[<font color=green> <工程师纯干货总结：TCP/IP 网络编程></font>](https://mp.weixin.qq.com/s/SIFdmkoZDVJGD-0Z4SIEiA)

[<font color=green><linux C socket 函数介绍和使用实例></font>](https://blog.csdn.net/XiaoXiaoPengBo/article/details/50349834)



以上两篇文章是以下内容的铺垫。

Socket 是应用层与协议族通信的中间软件抽象层，它是一组接口。

先附图一张，虽然是讲解 TCP 的 socket，但是道理相通。

![img](https://img-blog.csdn.net/20151218101102971?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### 基本 socket 函数

Linux 系统是通过提供套接字 (socket) 来进行网络编程的。网络的 socket 数据传输是一种特殊的I/O,socket 也是一种文件描述符。socket 也有一个类似于打开文件的函数：socket (), 调用 socket (), 该函数返回一个整型的 socket 的描述符，随后的连接建立、数据传输等操作也都是通过该 socket 实现。

####   socket 函数

```c
#include <sys/scoket.h>
int socket(int af, int type, int protocol)；
```

<font color=blue> 功能说明：</font>

+ 调用成功，返回 socket 文件描述符；失败，返回－ 1，并设置errno;两个网络程序之间的一个网络连接包括五种信息：通信协议、本地协议地址、本地主机端口、远端主机地址和远端协议端口。 socket 数据结构中包含这五种信息。

<font color=blue> 参数说明：</font>

+ af 指明所使用的协议族，通常为 PF_INET，表示 TCP/IP协议。AF 是“Address Family”的简写。AF_INET 表示 IPv4 地址，例如 127.0.0.1；AF_INET6 表示IPv6 地址，例如 1030::C9B4:FF12:48AA:1A2B。大家需要记住 127.0.0.1，它是一个特殊 IP 地址，表示本机地址，后面的教程会经常用到;

  | 名称     | 协议族               |
  | -------- | -------------------- |
  | PF_INET  | IPv4 互联网协议族    |
  | PF_INET6 | IPv4 互联网协议族    |
  | PF_LOCAL | 本地通信 unix 协议族 |


   + type 参数指定 socket 的类型，基本上有三种：数据流套接字、数据报套接字、原始套接字;

     + 面向链接的套接字类型 (SOCK_STREAM)

        SOCK_STREAM 表示面向连接的数据传输方式。数据可以准确无误地到达另一台计算机，如果损坏或丢失，可以重新发送，但效率相对较慢。常见的 http 协议就使用SOCK_STREAM 传输数据，因为要确保数据的正确性，否则网页不能正常解析。

     + 面向消息的套接字类型 ( SOCK_DGRAM)

        SOCK_DGRAM 表示无连接的数据传输方式。计算机只管传输数据，不作数据校验，如果数据在传输中损坏，或者没有到达另一台计算机，是没有办法补救的。也就是说，数据错了就错了，无法重传。因为 SOCK_DGRAM 所做的校验工作少，所以效率比 SOCK_STREAM 高。

     <font color=red>注意：SOCK_DGRAM 没有想象中的糟糕，不会频繁的丢失数据，数据错误只是小概率事件。</font>



   + protocol : 计算机通信中实用的协议信息， protocol 参数协议最终选择,常用的有 IPPROTO_TCP 和 IPPTOTO_UDP，分别表示 TCP传输协议和 UDP 传输协议。
     有了地址类型和数据传输方式，还不足以决定采用哪种协议吗？为什么还需要第三个参数呢？正如大家所想，一般情况下有了 af 和 type 两个参数就可以创建套接字了，操作系统会自动推演出协议类型，除非遇到这样的情况：有两种不同的协议支持同一种地址类型和数据传输类型。如果我们不指明使用哪种协议，操作系统是没办法自动推演的。该教程使用 IPv4 地址，参数 af 的值为 PF_INET。如果使用
     SOCK_STREAM 传输数据，那么满足这两个条件的协议只有TCP，因此可以这样来调用 socket () 函数;

     ```c
     int tcp_socket = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP); // IPPROTO_TCP表示TCP协议
     ```

     这种套接字称为 TCP 套接字。

     如果使用 SOCK_DGRAM 传输方式，那么满足这两个条件的协议只有 UDP，因此可以这样来调用 socket () 函数：

     ```c
     int ud_socket = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP); //IPPROTO_UDP表示UDP协议
     ```

     这种套接字称为 UDP 套接字。
     上面两种情况都只有一种协议满足条件，可以将 protocol 的值设为 0，系统会自动推演出应该使用什么协议，如下所示：

     ```c
     int tcp_socket = socket(AF_INET, SOCK_STREAM, 0); //创建TCP套接字
     int udp_socket = socket(AF_INET, SOCK_DGRAM, 0); //创建UDP套接字
     ```



####   bind 函数

```c
#include <sys/socket.h>
int bind(int server_sockfd, struct sockaddr *server_addr, int addrlen);
// socketfd 要分配的套接字文件描述 符
// myaddr 存储地址信息的结构体变量地址值
// addrlen 第二个结构体变量的长度
```

<font color=blue>功能说明：</font>

+ 调将套接字和指定的端口相连，对于服务器和客户端都是这样。成功返回 0，否则，返回-1，并置 errno;

<font color=blue>参数说明：</font>

+ server_sockfd 是调用 socket 函数返回值;

+ server_addr 是一个指向包含有本机 IP 地址及端口号等信息的sockaddr类型的指针；

  ```c
  struct sockaddr {
      __uint8_t sa_len;
      sa_family_t sa_family; //地址族
      char sa_data[14]; //地址信息
  };
  ```

  在 sa_data 一个成员里，包含了 ip、port 的地址信息，这样写起来很麻烦，所以有了新的结构体 sockaddr_in (IP 和端口进行了拆分)。sockaddr_in 结构体：

  ```c
  typedef unsigned short int	uint16_t;
  typedef uint16_t in_port_t;
  typedef unsigned short int sa_family_t;
  
  struct sockaddr_in {
      __uint8_t sin_len;
      sa_family_t sin_family;  //地址族
      in_port_t sin_port;      // TCP/UDP端口号
      struct in_addr sin_addr; //IP地址
      char sin_zero[8];
  };
  
  //在上面的结构体中，又嵌套了 in_addr 结构体，记录 IP 地址:
  typedef unsigned int	uint32_t;
  typedef uint32_t   in_addr_t;
  struct in_addr{
      in_addr_t s_addr; //32位IPv4地址
  };
  
  ```





  <font color=blue>结构体 sockaddr_in 的成员分析:</font>

  +  **成员 sin_family**:

    | 地址族   | 含义                       |
    | :------- | :------------------------- |
    | AF_INET  | IPv4 互联网使用的地址族    |
    | AF_INET6 | IPv6 互联网使用的地址族    |
    | AF_LOCAL | 本地通信 unix 使用的地址族 |
    | …        | …                          |

  + **成员 sin_port**:16 位端口号;

  + **成员 sin_addr**:32 位 ip 地址信息，以网络字节序保存;

  + **成员 sin_zero**:无特殊含义，为与 sockaddr 大小保持一致，写入 0 即可。

+ **addrlen 参数**:传递地址信息的长度;

  <font color=blue>**最终我们使用 bind 绑定地址方式**</font>

```c
//分配内存-构造服务端地址端口
memset(&serv_addr, 0, sizeof(serv_addr));
//IPv4中的地址族
serv_addr.sin_family = AF_INET;
//32位的IPv4地址， INADDR_ANY表示当前ip
serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
//16位tcp/udp端口号
serv_addr.sin_port = htons(atoi(argv[1]));

//分配地址
if (bind(serv_sock, (struct sockaddr*) &serv_addr,sizeof(serv_addr) )==-1){
    printf("bind() error");
    exit(0);
}
```

   bind 函数之前，构造了 sockaddr_in 结构体的数据，其中介绍几个点.

 + INADDR_ANY 会自动获取当前服务器的 IP;

 + 我们看到使用到了 htonl、htons 函数，构造 IP 地址和端口;

**==为什么构造结构体地址时候使用了 htonl、htons 对 IP、端口进行了转换？==**

首先我们来看下这几个函数的含义:

| 地址族 | 含义                                        |
| :----- | :------------------------------------------ |
| htons  | 把 short 型数据从主机字节序转化为网络字节序 |
| htonl  | 把 long 型数据从主机字节序转化为网络字节序  |
| ntohs  | 把 short 型数据从网络字节序转化为主机字节序 |
| ntohl  | 把 long 型数据从网络字节序转化为主机字节序  |
| …      | …                                           |

数据传输采用的网络字节序，那在传输前应直接把数据转换成网络字节序，接收的数据也需要转换城主机字节序再保存。上面这句话是有问题的，原因是数据收发过程中是有自动转换机制的。

除了 socketaddr_in 结构体变量手动填充数据转换外，其他情况不需要考虑字节序问题。

**==说了这么多字节序，那到底什么是网络字节序，什么是主机字节序==**

- 主机字节序：主机内部内存中数据的处理方式。
- 网络字节序：网络字节顺序是 TCP/IP 中规定好的一种数据表示格式，它与具体的 CPU 类型、操作系统等无关，从而可以保证数据在不同主机之间传输时能够被正确解释。网络字节顺序采用 big endian（大端）排序方式。

 **==大端又是啥，我们从两种网络字节顺序说起==**

+ 字节序：是指整数在内存中保存的顺序
+ cpu 向内存保存数据字节序有两种实现方式：
  + **小端字节序（little endian）**：低字节数据存放在内存低地址处，高字节数据存放在内存高地址处。
  + **大端字节序（bigendian）**：高字节数据存放在低地址处，低字节数据存放在高地址处。

图例:

![图片](https://mmbiz.qpic.cn/mmbiz_png/ABBvyuGJ3WYYGdFRRtMFt3qoXUaYUe9YH9XnibbNYb65sXubofOP3NrtY3up9fhtUIWGIPEtXwcRchvUYdszBzQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/ABBvyuGJ3WYYGdFRRtMFt3qoXUaYUe9YbSdemjnWVQiaT7eXbo2NKR5oeIQdrOydHiclWAIicuD1Bn54AK9zIdHlw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



大字节序更符合我们的阅读习惯。但是我们的主机使用的是哪种字节序取决于 CPU，不同的 CPU 型号有不同的选择。

当我们两台计算机是需要网络通信时，规范统一约定为大端序进行通讯处理.



#### htonl/htons/ntohl/ntohs函数



htonl 函数将主机的 unsigned long 值转换成网络字节顺序（32 位）（一般主机跟网络上传输的字节顺序是不通的，分大小端），函数返回一个网络字节顺序的数字。

作用相反的函数即把网络字节顺序转化成主机序列为 ntohl () 函数。

记忆这类函数，主要看前面的 n 和后面的 hl。。n 代表网络，h 代表主机 host，l 代表 long 的长度，还有相对应的 s 代表 16 位的 short

> 同类的函数：ntohs ()、htons () 就是转成 short 类型的。

```c
//
main(){
//u_long a = 0x12345678;
//u_long b = htonl (a);// 将主机的 unsigned long 转为网络字节顺序（32 位）
//u_long b = ntohl (a);// 将网络字节顺序（32 位）转为主机字节


u_short a = 0x1234;
//u_short b = ntohs (a);//32 位
u_short b = htons(a);
}
```



#### inet_addr / inet_ntoa函数

函数原型：

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

typedef unsigned int	uint32_t;
typedef uint32_t   in_addr_t;
typedef unsigned short int	uint16_t;
typedef uint16_t in_port_t;
typedef unsigned short int sa_family_t;


struct sockaddr_in {
    __uint8_t sin_len;
    sa_family_t sin_family;  //地址族
    in_port_t sin_port;      // TCP/UDP端口号
    struct in_addr sin_addr; //IP地址
    char sin_zero[8];
};


struct in_addr{
    in_addr_t s_addr; //32位IPv4地址
};

in_addr_t inet_addr(const char *cp);
int inet_aton(const char *cp, struct in_addr *inp);
char *inet_ntoa(struct in_addr in)
```

函数说明：inet_addr () 用来将参数 cp 所指的网络地址字符串转换成网络所使用的二进制数字。网络地址字符串是以数字和点组成的字符串，例如：`"163. 13. 132. 68"`。

返回值：成功则返回对应的网络二进制的数字，失败返回 - 1.

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
    struct sockaddr_in m_sockaddr;
    m_sockaddr.sin_family = AF_INET;
    //将字符串转换为in_addr类型
    m_sockaddr.sin_addr.s_addr = inet_addr("192.168.1.111");
    m_sockaddr.sin_port = htons(5000);

    printf("#############  1  ##########################\n\n");
    printf("inet_addr(\"192.168.1.111\") = %u\n", inet_addr("192.168.1.111"));
    printf("m_sockaddr.sin_addr.s_addr = %u\n", inet_addr("192.168.1.111"));
    printf("inet_addr ip = %u\n", inet_addr("192.168.1.111"));

    //将s_addr类型转换为字符串
    printf("inet_ntoa ip = %s\n", inet_ntoa(m_sockaddr.sin_addr));

    m_sockaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    m_sockaddr.sin_port = htons(5000);
    printf("#############  2  ##########################\n\n");
    printf("inet_addr(\"192.168.1.111\") = %u\n", htonl(INADDR_ANY));
    printf("m_sockaddr.sin_addr.s_addr = %u\n", htonl(INADDR_ANY));
    printf("inet_addr ip = %u\n", htonl(INADDR_ANY));

    //将s_addr类型转换为字符串
    printf("inet_ntoa ip = %s\n", inet_ntoa(m_sockaddr.sin_addr));

    return 0;
}

```

输出如下：

```bash
# junjie @ Ubuntu in ~/公共的/c文件/systemlib [日期: 周二 3月 16日, 时间:20:51:07]
$ ./test
#############  1  ##########################

inet_addr("192.168.1.111") = 1862379712
m_sockaddr.sin_addr.s_addr = 1862379712
inet_addr ip = 1862379712
inet_ntoa ip = 192.168.1.111
#############  2  ##########################

inet_addr("192.168.1.111") = 0
m_sockaddr.sin_addr.s_addr = 0
inet_addr ip = 0
inet_ntoa ip = 0.0.0.0
```





#### **inet_pton** /  **inet_ntop**函数

```c
#include <arpa/inet.h>
int inet_pton(int af, const char *src, void *dst);
```

 <font color=blue>功能说明：</font>

+ 将 IPv4 和 IPv6 地址从点分十进制转换为二进制

+ 该函数将字符串 `src` 转换为 `af` 地址类型协议簇的网络地址，并存储到 `dst` 中。对于 `af` 参数，必须为 `AF_INET` 或 `AF_INET6`

<font color=blue>返回值：</font>

+ `inet_pton` 转换成功则返回 1, 对于指定的地址类型协议簇，如果不是一个有效的网络地址，将转换失败，返回 0, 如果指定的地址类型协议簇不合法，将返回 - 1 并，并且 `errno` 设置为 `EAFNOSUPPORT `



```c
#include <arpa/inet.h>
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
```

 <font color=blue>功能说明：</font>

+ 将 IPv4 和 IPv6 地址从二进制转换为点分十进制

+ 该函数将地址类型协议簇为 `af` 的网络地址 `src` 转换为字符串，并将其存储到 `dst` 中，其中 `dst` 不能是空指针。调用者在参数 `size` 中指定可使用的缓冲字节数。

<font color=blue>返回值：</font>

+ `inet_ntop` 执行成功，返回一个指向 `dst` 的非空指针，如果执行失败，将返回 `NULL`，并且 `errno` 设置为相应的错误类型。

<font color=blue>错误代码：</font>

- EAFNOSUPPORT
  `af` 并不是一个合法的地址类型协议簇
- ENOSPC
  要转换的字符串地址 `src` 其字节大小超过了给定的缓冲字节大小



> 实例
>
> ```c
> #include <arpa/inet.h>
> #include <stdio.h>
> #include <stdlib.h>
> #include <string.h>
> 
> int main(int argc, char *argv[])
> {
>  unsigned char buf[sizeof(struct in6_addr)];
>  int domain, s;
>  char str[INET6_ADDRSTRLEN];
> 
>  if (argc != 3)
>  {
>      fprintf(stderr, "Usage: %s {i4|i6|<num>} string\n", argv[0]);
>      exit(EXIT_FAILURE);
>  }
> 
>  domain = (strcmp(argv[1], "i4") == 0) ? AF_INET : (strcmp(argv[1], "i6") == 0) ? AF_INET6: atoi(argv[1]);
> 
>  s = inet_pton(domain, argv[2], buf);
>  if (s <= 0)
>  {
>      if (s == 0)
>          fprintf(stderr, "Not in presentation format");
>      else
>          perror("inet_pton");
>      exit(EXIT_FAILURE);
>  }
> 
>  if (inet_ntop(domain, buf, str, INET6_ADDRSTRLEN) == NULL)
>  {
>      perror("inet_ntop");
>      exit(EXIT_FAILURE);
>  }
> 
>  printf("%s\n", str);
> 
>  exit(EXIT_SUCCESS);
> }
> ```
>
> 输出：
>
> ```c
> # junjie @ Ubuntu in ~/公共的/c文件/systemlib [日期: 周二 3月 16日, 时间:21:05:51]
> $ ./test i4 192.168.1.110
> 192.168.1.110
> ```
>
> 



####   connect 函数

```c
#include <sys/types.h>
int connect(int client_sockfd, struct sockaddr_in *serv_addr,int addrlen);
```

<font color=blue>功能说明：</font>

+ 客户端发送服务请求，只有客户端才用，对于客户端，client_sockfd是客户端 socket() 返回的套接字描述符，serv_addr 是服务端的sockadd_in 结构体。成功返回 0，否则返回-1，并置 errno。

<font color=blue>参数说明：</font>

+ client_sockfd 是客户端 socket 函数返回的 socket 描述符；serv_addr是包含远端主机 IP 地址和端口号的指针；addrlen 是结构 sockaddr_in 的长度。

####  listen 函数

   ```c
   #include <sys/socket.h>
   int listen(int sock_fd, int backlog);  //Linux
   int listen(SOCKET sock, int backlog);  //Windows
   ```


<font color=blue>功能说明：</font>

   + 等待指定的端口的出现客户端连接，只有服务器端才用。调用

    成功返回 0，否则，返回－ 1，并置 errno.

<font color=blue>参数说明：</font>

   + sock_fd 是服务器端 socket() 函数返回值；
   + backlog 指定在请求队列中允许的最大请求数

   **请求队列**

   当套接字正在处理客户端请求时，如果有新的请求进来，套接字是没法处理的，只能把它放进缓冲区，待当前请求处理完毕后，再从缓冲区中读取出来处理。如果不断有新的请求进来，它们就按照先后顺序在缓冲区中排队，直到缓冲区满。这个缓冲区，就称为请求队列（Request Queue）。

   缓冲区的长度（能存放多少个客户端请求）可以通过 listen () 函数的 backlog 参数指定，但究竟为多少并没有什么标准，可以根据你的需求来定，并发量小的话可以是 10 或者 20。

   如果将 backlog 的值设置为 SOMAXCONN，就由系统来决定请求队列长度，这个值一般比较大，可能是几百，或者更多。

   当请求队列满时，就不再接收新的请求，对于 Linux，客户端会收到 ECONNREFUSED 错误，对于 Windows，客户端会收到 WSAECONNREFUSED 错误。

   <font color=red>注意：listen () 只是让套接字处于监听状态，并没有接收请求。接收请求需要使用 accept () 函数。</font>

> ##### accept函数

   ```c
   #include <sys/types.h>
   int accept(int server_sockfd, struct sockadd * client_addr, int addrlen);
   ```

   <font color=blue>功能说明：</font>

   + 只在服务端使用，用于接受客户端的服务请求，成功返回新的套接字描述符 clent_sockfd，这个新的描述符是服务端的 send/recv/read/write 函数的第一个参数，失败返回－1，并置 errno。

     errno错误代码：

     + EBADF 参数 s 非合法 socket 处理代码.
     + EFAULT 参数 addr 指针指向无法存取的内存空间.
     + ENOTSOCK 参数 s 为一文件描述词，非 socket.
     + EOPNOTSUPP 指定的 socket 并非 SOCK_STREAM.
     + EPERM 防火墙拒绝此连线.
     + ENOBUFS 系统的缓冲内存不足.
     + ENOMEM 核心内存不足.

   <font color=blue>参数说明：</font>

   + server_sockfd 是被监听的 socket 描述符，也就是服务端的 socket 返回的 socket 描述符，是服务器端套接字, addr 通常是一个指向客户端 sockaddr 变量的指针，这个指针的内容是不需要指定的，只需要定义、分配好内存空间；addrlen 是结构 sockaddr 的长度。

   <font color=red>注意：accept () 返回一个新的套接字来和客户端通信，client_addr 保存了客户端的 IP 地址和端口号，而 sock 是服务器端的套接字，大家注意区分。后面服务端和客户端通信时，要使用这个新生成的套接字，而不是原来服务器端的套接字。accept () 用来接受参数 server_sockfd 的 socket 连线。参数 server_sockfd 的 socket 必需先经 bind ()、listen () 函数处理过，当有连线进来时 accept () 会返回一个新的 socket 处理代码，往后的数据传送与读取就是经由新的 socket 处理，而原来参数 server_sockfd 的 socket 能继续使用 accept () 来接受新的连线要求。连线成功时，参数 client_addr 所指的结构会被系统填入远程主机的地址数据，参数 addrlen 为 scokaddr 的结构长度。

   最后需要说明的是：listen () 只是让套接字进入监听状态，并没有真正接收客户端请求，listen () 后面的代码会继续执行，直到遇到 accept ()。accept () 会阻塞程序执行（后面代码不能被执行），直到有新的请求到来。</font>



####   write函数

```c
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t nbytes);
```

<font color=blue>功能说明：</font>

+ write 函数将 buf 中的 nbytes 字节内容写入文件描述符 fd，write () 会把参数 buf 所指的内存写入 count 个字节到参数 fd 所指的文件内。当然，文件读写位置也会随之移动. 对于客户端，fd 为客户端 socket 返回的套接字描述符 client_sockfd，对于服务端，fd 为 accept 返回的新的套接字描述符。成功时返回写的字节数. 失败时返回-1. 并设置 errno 变量,错误代码：
  + EINTR 此调用被信号所中断.
  + EAGAIN 当使用不可阻断 I/O 时 (O_NONBLOCK), 若无数据可读取则返回此值.
  + EADF 参数 fd 非有效的文件描述词，或该文件已关闭.

<font color=blue>参数说明：</font>

####    read函数

```c
#include <unistd.h>
ssize_t read(int fd,void *buf,size_t nbyte);
```



<font color=blue>功能说明：</font>

+ read 函数是负责从 fd 中读取内容，对于客户端，fd 为客户端socket 返回的套接字描述符 client_sockfd，对于服务端，fd 为accept 返回的新的套接字描述符。当读成功时，read 返回实际
  所读的字节数，如果返回的值是 0 表示已经读到文件的结束了,当有错误发生时则返回-1, 错误代码存入 errno 中，而文件读写位置则无法预期. 小于 0 表示出现了错误. 如果错误为 EINTR 说明读是由中断引起的, 如果错误是 ECONNREST 表示网络连接出了问题.
+ read () 会把参数 fd 所指的文件传送 count 个字节到 buf 指针所指的内存中。若参数 count 为 0, 则 read () 不会有作用并返回 0. 返回值为实际读取到的字节数，如果返回 0, 表示已到达文件尾或是无可读取的数据，此外文件读写位置会随读取到的字节移动.错误代码：
  + EINTR 此调用被信号所中断.
  + EAGAIN 当使用不可阻断 I/O 时 (O_NONBLOCK), 若无数据可读取则返回此值.
  + EBADF 参数 fd 非有效的文件描述词，或该文件已关闭.

write 和 read 可以用send/recv替代。



#### send/recv函数

```c
#include <sys/socket.h>
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
```

recv 和 send 的前 3 个参数等同于 read 和 write。flags 参数一般置为 0，或：

| flags         | 说明               | recv  | send |
| ------------- | ------------------ | ----- | ---- |
| MSG_DONTROUTE | 绕过路由表查找     |       | •    |
| MSG_DONTWAIT  | 仅本操作非阻塞     | **•** | •    |
| MSG_OOB       | 发送或接收带外数据 | •     | •    |
| MSG_PEEK      | 窥看外来消息       | •     |      |
| MSG_WAITALL   | 等待所有数据       | •     |      |

<font size=24, color=blue>Send</font>

同步 Socket 的 send 函数的执行流程：当调用该函数时，send先比较待发送数据的长度 len 和套接字 sockfd 的发送缓冲的长度（因为待发送数据是要 copy 到套接字 sockfd 的发送缓冲区的，注意并不是 send 把 sockfd 的发送缓冲中的数据传到连接的另一端的，而是协议传的，send 仅仅是把 buf 中的数据 copy到 sockfd 的发送缓冲区的剩余空间里）：

+ 如果 len 大于 sockfd 的发送缓冲区的长度，该函数返回SOCKET_ERROR；
+ 如果 len 小于或者等于 sockfd 的发送缓冲区的长度，那么send 先检查协议是否正在发送 sockfd 的发送缓冲中的数据，如果是就等待协议把数据发送完，如果协议还没有开始发送 sockfd 的发送缓冲中的数据或者 sockfd 的发送缓冲中没有数据，那么 send 就比较 sockfd 的发送缓冲区的剩余空间
  和 len：
  + 如果 len 大于剩余空间大小 send 就一直等待协议把 sockfd 的发送缓冲中的数据发送完；
  + 如果 len 小于剩余空间大小 send 就仅仅把 buf 中的数据 copy 到剩余空间里。

+ 如果 send 函数 copy 数据成功，就返回实际 copy 的字节数，如果 send 在 copy 数据时出现错误，那么 send 就返回SOCKET_ERROR；如果 send 在等待协议传送数据时网络断开的话，那么 send 函数也返回 SOCKET_ERROR。
+ send 函数把 buf 中的数据成功 copy 到 sockfd 的发送缓冲的剩余空间里后它就返回了，但是此时这些数据并不一定马上被传到连接的另一端。如果协议在后续的传送过程中出现网络错误的话，那么下一个 Socket 函数就会返回 SOCKET_ERROR。(每一个除 send 外的 Socket 函数在执行的最开始总要先等待套接字的发送缓冲中的数据被协议传送完毕才能继续，如果在等待时出现网络错误，那么该 Socket 函数就返回 SOCKET_ERROR）.
+ 在 unix 系统下，如果 send 在等待协议传送数据时网络断开，调用 send 的进程会接收到一个 SIGPIPE 信号，进程对该信号的处理是进程终止。

<font size=24, color=blue>recv</font>

```c
#include <sys/socket.h>
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
```



+ 同步 Socket 的 recv 函数的执行流程：当应用程序调用 recv 函数时，recv 先等待 sockfd 的发送缓冲中的数据被协议传送完毕;
+ 如果协议在传送 sockfd 的发送缓冲中的数据时出现网络错误，那么 recv 函数返回 SOCKET_ERROR；
+ 如果sockfd 的发送缓冲中没有数据或者数据被协议成功发送完毕后，recv 先检查套接字 sockfd 的接收缓冲区;
+ 如果 sockfd 接收缓冲区中没有数据或者协议正在接收数据，那么 recv 就一直等待，直到协议把数据接收完毕;
+ 当协议把数据接收完毕，recv 函数就把 sockfd 的接收缓冲中的数据 copy 到 buf 中（注意协议接收到的数据可能大于 buf 的长度，所以在这种情况下要调用几次 recv 函数才能把 sockfd 的接收缓冲中的数据 copy 完。recv 函数仅仅是 copy 数据，真正的接收数据是协议来完成的），recv 函数返回其实际 copy 的字节数;
+ 如果 recv 在 copy 时出错，那么它返回 SOCKET_ERROR；如果 recv 函数在等待协议接收数据时网络中断了，那么它返回0。

####  close函数

   ```
   #include <unistd.h>
   int close(sock_fd);
   ```

   <font color=blue>功能说明：</font>

   + 当所有的数据操作结束以后，你可以调用 close() 函数来释放该socket，从而停止在该 socket 上的任何数据操作，函数运行成功返回 0，否则返回-1;



####  示例

>
>实例1：
>
>```c
>//server.c
>#include <sys/types.h>
>#include <sys/socket.h>
>#include <stdio.h>
>#include <netinet/in.h>
>#include <arpa/inet.h>
>#include <unistd.h>
>#include <string.h>
>#include <stdlib.h>
>#include <fcntl.h>
>#include <sys/shm.h>
>
>#define MYPORT 8887
>#define QUEUE 20
>#define BUFFER_SIZE 1024
>
>int main()
>{
>///定义sockfd
>int server_sockfd = socket(AF_INET, SOCK_STREAM, 0);
>
>///定义sockaddr_in
>struct sockaddr_in server_sockaddr;
>server_sockaddr.sin_family = AF_INET;
>server_sockaddr.sin_port = htons(MYPORT);
>server_sockaddr.sin_addr.s_addr = htonl(INADDR_ANY);
>
>///bind，成功返回0，出错返回-1
>if (bind(server_sockfd, (struct sockaddr *)&server_sockaddr, sizeof(server_sockaddr)) == -1)
>{
>    perror("bind");
>    exit(1);
>}
>
>///listen，成功返回0，出错返回-1
>if (listen(server_sockfd, QUEUE) == -1)
>{
>    perror("listen");
>    exit(1);
>}
>
>///客户端套接字
>char buffer[BUFFER_SIZE];
>struct sockaddr_in client_addr;
>socklen_t length = sizeof(client_addr);
>
>///成功返回非负描述字，出错返回-1
>int conn = accept(server_sockfd, (struct sockaddr *)&client_addr, &length);
>if (conn < 0)
>{
>    perror("connect");
>    exit(1);
>}
>
>while (1)
>{
>    memset(buffer, 0, sizeof(buffer));
>    int len = recv(conn, buffer, sizeof(buffer), 0);
>    if (strcmp(buffer, "exit\n") == 0)
>        break;
>    fputs(buffer, stdout);
>    send(conn, buffer, len, 0);
>}
>close(conn);
>close(server_sockfd);
>return 0;
>}
>```
>
>```c
>//client.c
>#include <sys/types.h>
>#include <sys/socket.h>
>#include <stdio.h>
>#include <netinet/in.h>
>#include <arpa/inet.h>
>#include <unistd.h>
>#include <string.h>
>#include <stdlib.h>
>#include <fcntl.h>
>#include <sys/shm.h>
>
>#define MYPORT 8887
>#define BUFFER_SIZE 1024
>
>int main()
>{
>///定义sockfd
>int sock_cli = socket(AF_INET, SOCK_STREAM, 0);
>
>///定义sockaddr_in
>struct sockaddr_in servaddr;
>memset(&servaddr, 0, sizeof(servaddr));
>servaddr.sin_family = AF_INET;
>servaddr.sin_port = htons(MYPORT);                 ///服务器端口
>servaddr.sin_addr.s_addr = inet_addr("127.0.0.1"); ///服务器ip
>
>///连接服务器，成功返回0，错误返回-1
>if (connect(sock_cli, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
>{
>    perror("connect");
>    exit(1);
>}
>
>char sendbuf[BUFFER_SIZE];
>char recvbuf[BUFFER_SIZE];
>while (fgets(sendbuf, sizeof(sendbuf), stdin) != NULL)
>{
>    send(sock_cli, sendbuf, strlen(sendbuf), 0); ///发送
>    if (strcmp(sendbuf, "exit\n") == 0)
>        break;
>    recv(sock_cli, recvbuf, sizeof(recvbuf), 0); ///接收
>    fputs(recvbuf, stdout);
>
>    memset(sendbuf, 0, sizeof(sendbuf));
>    memset(recvbuf, 0, sizeof(recvbuf));
>}
>
>close(sock_cli);
>return 0;
>}
>```
>
>实例2
>
>```c
>//echo_server.c
>#include <stdio.h>
>#include <sys/socket.h>
>#include <arpa/inet.h>
>
>#define BUF_SIZE 5
>
>int main(int argc, char *argv[])
>{
>char message[BUF_SIZE];
>int str_len, i;
>
>struct sockaddr_in serv_addr, clnt_addr;
>
>int serv_sock = socket(PF_INET, SOCK_STREAM, 0);
>if (serv_sock == -1)
>{
>    printf("socket() error");
>    exit(1);
>}
>
>memset(&serv_addr, 0, sizeof(serv_addr));
>serv_addr.sin_family = AF_INET;
>serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
>serv_addr.sin_port = htons(9600);
>
>if (bind(serv_sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == -1)
>{
>    printf("bind() error");
>    exit(1);
>}
>
>if (listen(serv_sock, 5) == 1)
>{
>    printf("listen() error");
>    exit(1);
>}
>
>int clnt_addr_sz = sizeof(clnt_addr);
>for (i = 0; i < 5; i++)
>{
>    int clnt_sock = accept(serv_sock, (struct sockaddr *)&clnt_addr, &clnt_addr_sz);
>    if (clnt_sock == -1)
>    {
>        printf("accept() error");
>        exit(1);
>    }
>
>    while (str_len = read(clnt_sock, message, BUF_SIZE) > 0)
>    {
>        write(clnt_sock, message, str_len);
>    }
>
>    close(clnt_sock);
>}
>
>close(serv_sock);
>return 0;
>}
>```
>
>```c
>//echo_client.c
>#include <stdio.h>
>#include <string.h>
>#include <sys/socket.h>
>#include <arpa/inet.h>
>
>#define BUF_SIZE 5
>
>int main(int argc, char *argv[])
>{
>char message[BUF_SIZE];
>int str_len, i;
>
>struct sockaddr_in serv_addr, clnt_addr;
>
>int serv_sock = socket(PF_INET, SOCK_STREAM, 0);
>if (serv_sock == -1)
>{
>    printf("socket() error");
>    exit(1);
>}
>
>memset(&serv_addr, 0, sizeof(serv_addr));
>serv_addr.sin_family = AF_INET;
>serv_addr.sin_addr.s_addr = inet_addr("127.0.0.1");
>serv_addr.sin_port = htons(9600);
>
>if (connect(serv_sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == -1)
>{
>    printf("connect() error");
>    exit(1);
>}
>
>while (1)
>{
>    fputs("请输入您的信息,按Q键退出\n", stdout);
>    fgets(message, 1024, stdin);
>
>    //因为fgets会保留输入中换行符,故判断加\n
>    if (!strcmp(message, "q\n") || !strcmp(message, "Q\n"))
>    {
>        break;
>    }
>
>    write(serv_sock, message, sizeof(message));
>    read(serv_sock, message, BUF_SIZE - 1);
>    printf("Message from server: %s\n", message);
>}
>
>close(serv_sock);
>return 0;
>}
>```
>
>



#### sendto / recvfrom函数

```c
#include < sys/types.h >
#include < sys/socket.h >
//定义函数
int sendto(int s , const void * msg, int len, unsigned int flags, const struct sockaddr * to , int tolen ) ;

```

   <font color=blue>功能说明：</font>

+ sendto () 用来将数据由指定的 socket 传给对方主机。参数 s 为已建好连线的 socket, 如果利用 UDP 协议则不需经过连线操作。参数 msg 指向欲连线的数据内容，参数 flags 一般设 0，详细描述请参考 send ()。参数 to 用来指定欲传送的网络地址，结构 sockaddr 请参考 bind ()。参数 tolen 为 sockaddr 的结果长度。

    <font color=blue>返回值：</font>

+ 成功则返回实际传送出去的字符数，失败返回－1，错误原因存于 errno 中。

   <font color=blue>errno错误代码：</font>

+  EBADF 参数 s 非法的 socket 处理代码。 
+ EFAULT 参数中有一指针指向无法存取的内存空间。 
+ WNOTSOCK canshu s 为一文件描述词，非socket。 
+ EINTR 被信号所中断。 
+ EAGAIN 此动作会令进程阻断，但参数 s 的 soket 为补课阻断的。 
+ ENOBUFS 系统的缓冲内存不足。
+  EINVAL 传给系统调用的参数不正确。

```c
#include < sys/types.h >
#include < sys/socket.h >
//定义函数
int recvfrom(int s,void *buf,int len,unsigned int flags ,struct sockaddr *from ,int *fromlen);
```

   <font color=blue>功能说明：</font>

+  recv () 用来接收远程主机经指定的 socket 传来的数据，并把数据存到由参数 buf 指向的内存空间，参数 len 为可接收数据的最大长度。参数 flags 一般设 0，其他数值定义请参考 recv ()。参数 from 用来指定欲传送的网络地址，结构 sockaddr 请参考 bind ()。参数 fromlen 为 sockaddr 的结构长度。

    <font color=blue>返回值：</font>

+  成功则返回接收到的字符数，失败则返回 - 1，错误原因存于 errno 中。

   <font color=blue>errno错误代码：</font>

+   EBADF 参数 s 非合法的 socket 处理代码;
+  EFAULT 参数中有一指针指向无法存取的内存空间。
+  ENOTSOCK 参数 s 为一文件描述词，非 socket。
+  EINTR 被信号所中断。 
+ EAGAIN 此动作会令进程阻断，但参数 s 的 socket 为不可阻断。 
+ ENOBUFS 系统的缓冲内存不足;
+ ENOMEM 核心内存不足 ；
+ EINVAL 传给系统调用的参数不正确。







> 实例
>
> ```c
> //server.c
> #include <sys/types.h>  
> #include <sys/socket.h>  
> #include <netinet/in.h>  
> #include <arpa/inet.h>  
> #include <unistd.h>  
> #include <stdlib.h>  
> #include <string.h>  
> #include <stdio.h>  
> #define PORT 1111 /* 使用的 port*/  
> main(){  
>  int sockfd,len;  
>  struct sockaddr_in addr;  
>  int addr_len = sizeof(struct sockaddr_in);  
>  char buffer[256];  
>  /* 建立 socket*/  
>  if((sockfd=socket(AF_INET,SOCK_DGRAM,0))<0){  
>      perror ("socket");  
>      exit(1);  
>  }  
>  /* 填写 sockaddr_in 结构 */  
>  bzero ( &addr, sizeof(addr) );  
>  addr.sin_family=AF_INET;  
>  addr.sin_port=htons(PORT);  
>  addr.sin_addr.s_addr=htonl(INADDR_ANY) ;  
>  if (bind(sockfd, (struct sockaddr *)&addr, sizeof(addr))<0){  
>      perror("connect");  
>      exit(1);  
>  }  
>  while(1){  
>      bzero(buffer,sizeof(buffer));  
>      len = recvfrom(sockfd, buffer,sizeof(buffer), 0 , (struct sockaddr *)&addr ,&addr_len);  
>      /* 显示 client 端的网络地址 */  
>      printf("receive from %s\n" , inet_ntoa( addr.sin_addr));  
>      /* 将字串返回给 client 端 */  
>      sendto(sockfd,buffer,len,0,(struct sockaddr *)&addr,addr_len);  
>  }  
> }  
> 
> 
> // client.c
> #include <sys/types.h>  
> #include <sys/socket.h>  
> #include <netinet/in.h>  
> #include <arpa/inet.h>  
> #include <unistd.h>  
> #include <stdlib.h>  
> #include <string.h>  
> #include <stdio.h>  
> #define PORT 1111  
> #define SERVER_IP "127.0.0.1"  
> main()  
> {  
>  int s,len;  
>  struct sockaddr_in addr;  
>  int addr_len =sizeof(struct sockaddr_in);  
>  char buffer[256];  
>  /* 建立 socket*/  
>  if((s = socket(AF_INET,SOCK_DGRAM,0))<0){  
>      perror("socket");  
>      exit(1);  
>  }  
>  /* 填写 sockaddr_in*/  
>  bzero(&addr,sizeof(addr));  
>  addr.sin_family = AF_INET;  
>  addr.sin_port = htons(PORT);  
>  addr.sin_addr.s_addr = inet_addr(SERVER_IP);  
>  while(1){  
>      bzero(buffer,sizeof(buffer));  
>      /* 从标准输入设备取得字符串 */  
>      len =read(STDIN_FILENO,buffer,sizeof(buffer));  
>      /* 将字符串传送给 server 端 */  
>      sendto(s,buffer,len,0,(struct sockaddr *)&addr,addr_len);  
>      /* 接收 server 端返回的字符串 */  
>      len = recvfrom(s, buffer, sizeof(buffer), 0, (struct sockaddr *)&addr, &addr_len);  
>      printf("receive: %s",buffer);  
>  }  
> }
> ```
>
> 





## setsockopt () 函数

```c
#include <sys/types.h>
#include <sys/socket.h>
int setsockopt (int sockfd, int level, int optname, const void * optval, ,socklen_toptlen);

struct in_addr
{
	in_addr_t s_addr;
};

struct ip_mreq
{
	struct in_addr imn_multiaddr; // 多播组 IP，类似于 QQ 群号
	struct in_addr imr_interface;   // 将要添加到多播组的 IP，类似于QQ 成员号
};
// 若进程要加入到一个组播组中，用 soket 的 setsockopt () 函数发送该选项。该选项类型是 ip_mreq 结构，它的第一个字段 imr_multiaddr 指定了组播组的地址，第二个字段 imr_interface 指定了接口的 IPv4 地址。
```

<font color=blue>功能说明：</font>

+ setsockopt () 用来设置参数 sockfd 所指定的 socket 状态。获取或者***设置***与某个套接字关联的选 项。选项可能存在于多层**协议***中，它们总会出现在最上面的套接字层。当操作套接字选项时，选项位于的层和选项的名称必须给出。为了操作套接字层的选项，应该 将层的值指定为SOL_SOCKET。为了操作其它层的选项，控制选项的合适协议号必须给出。例如，为了表示一个选项由 ***TCP*** 协议解析，层应该设定为协议 号 TCP 。

<font color=blue>参数说明：</font>

+ sockfd是套接字描述符,指向一个打开的套接口描述符。

+ 参数 level 是被设置的选项的级别，如果想要在套接字级别上设置选项，就必须把 level 设置为 SOL_SOCKET，它有以下选项：

  + SOL_SOCKET: 基本套接口，通用套接字选项。
  + IPPROTO_IP: IPv4 套接口
  + IPPROTO_IPV6: IPv6 套接口
  + IPPROTO_TCP: TCP 套接口

    一般设成 SOL_SOCKET 以存取 socket 层。

+ 参数 optname (选项名) 代表欲设置的选项的名称，option_name 可以有哪些取值，这取决于 level，下面会具体阐述：


+ 参数 optval (选项值) 代表欲设置的值；
+ 参数 optlen  (选项长度) 则为 optval 的长度.

返回值：成功则返回 0, 若有错误则返回 - 1, 错误原因存于 errno.

errno附加说明：

+ EBADF参数 sockfd 不是有效的**文件**描述词；
+ ENOTSOCK 参数 sockfd描述的不是套接字；
+ ENOPROTOOPT 参数 optname 指定的选项不正确，指定的协议层不能识别选项；
+ EFAULT 参数 optval 指针指向无法存取的内存空间，optval 指向的内存并非有效的进程 空间 ；

> 当level为SOL_SOCKET时，option_name 可以有以下取值：[见此处](https://www.cnblogs.com/cthon/p/9270778.html)
>   + SO_DEBUG 打开或关闭调试信息，当 option_value 不等于 0 时，打开调试信息，否则，关闭调试信息；+  SO_REUSEADDR 允许在 bind () 过程中本地地址可重复使用，打开或关闭地址复用功能，当 option_value 不等于 0 时，打开，否则，关闭；
>
> + SO_TYPE 返回 socket 形态；
>
>  + SO_ERROR 返回 socket 已发生的错误原因；
>
>  + SO_DONTROUTE 送出的数据包不要利用路由设备来传输；
>
>  + SO_BROADCAST 使用广播方式传送；
>
>  + SO_SNDBUF 设置发送缓冲区的大小；
>
>    + 在 send () 的时候，返回的是实际发送出去的字节 (同步) 或发送到 socket 缓冲区的字节(异步); 系统默认的状态发送和接收一次为8688 字节 (约为 8.5K)；在实际的过程中发送数据和接收数据量比较大，可以设置 socket 缓冲区，而避免了 send (),recv () 不断的循环收发：
>
>      ```c
>      // 接收缓冲区
>      int nRecvBuf=32*1024;// 设置为 32K
>      setsockopt(s,SOL_SOCKET,SO_RCVBUF,(const char*)&nRecvBuf,sizeof(int));
>      // 发送缓冲区
>      int nSendBuf=32*1024;// 设置为 32K
>      setsockopt(s,SOL_SOCKET,SO_SNDBUF,(const char*)&nSendBuf,sizeof(int));
>      //如果在发送数据的时，希望不经历由系统缓冲区到 socket 缓冲区的拷贝而影响程序的性能：
>      int nZero=0;
>      setsockopt(socket，SOL_S0CKET,SO_SNDBUF，(char *)&nZero,sizeof(nZero));
>
>      //同上在 recv () 完成上述功能 (默认情况是将 socket 缓冲区的内容拷贝到系统缓冲区)：
>      int nZero=0;
>      setsockopt(socket，SOL_S0CKET,SO_RCVBUF，(char *)&nZero,sizeof(int));
>      ```
>
> 
>
>  + SO_RCVBUF 设置接收缓冲区的大小；
>
>  + SO_KEEPALIVE 定期确定连线是否已终止，套接字保活，如果协议是 TCP，并且当前的套接字状态不是侦听 (listen) 或关闭 (close)，那么，当 option_value 不是零时，启用 TCP 保活定时 器，否则关闭保活定时器；
>
>  + SO_OOBINLINE 当接收到 OOB 数据时会马上送至标准输入设备；
>
>  +  SO_LINGER 确保数据安全且可靠的传送出去；



> 当level为IPPROTO_IP时，option_name 可以有以下取值：
>
>   +  IP_ADD_MEMBERSHIP这个 option 和下面的 option 是实现多播必不可少的，它用于加入一个多播组，例：
>
>     ```c
>     struct ip_mreq ipmr;
>     ipmr.imr_interface.s_addr = htonl(INADDR_ANY);//本地某一网络设备接口的IP地址。
>     ipmr.imr_multiaddr.s_addr = inet_addr("234.5.6.7");//组播组的IP地址。
>     setsockopt(s, IPPROTO_IP, IP_ADDR_MEMBERSHIP, (char*)&ipmr, sizeof(ipmr));
>     ```
>
> 
>
>   +  IP_DROP_MEMBERSHIP 用于离开一个多播组，使用方法同 IP_ADDR_MEMBERSHIP:
>
>     ```c
>     struct ip_mreq ipmr;
>     int   len;
>     setsockopt(s, IPPROTO_IP, IP_DROP_MEMBERSHIP, (char*)&ipmr, &len);
>     ```
>
> 
>
> + IP_MULTICAST_IF 该选项可以修改网络接口，在结构 ip_mreq 中定义新的接口，发送多播报文时用的本地接口，默认情况下被设置成了本地接口的第一个地址；
>
> 
>
>  + IP_MULTICAST_TTL 设置组播报文的数据包的 TTL（生存时间）。默认值是 1，表示数据包只能在本地的子网中传送，默认情况下，多播报文的TTL被设置成了1，也就是说到这个报文在网络传送的时候，它只能在自己所在的网络传送，当要向外发送的时候，路由器把TTL减1以后变成了0，这个报文就已经被 Discard 了。例：
>
>    ```c
>    char ttl;
>    ttl = 2;
>    setsockopt(s, IPPROTO_IP, IP_MULTICAST_TTL, (char*)ttl, sizeof(ttl));
>    ```
>
> 
>
>  + IP_MULTICAST_LOOP 组播组中的成员自己也会收到它向本组发送的报文。这个选项用于选择是否激活这种状态，当接收者加入到一个多播组以后，再向这个多播组发送数据，这个字段的设置是否允许再返回到本身；
>
>    ```c
>    int loop=1;    //1:on  0:off
>    setsockopt(sock,IPPROTO_IP,IP_MULTICAST_LOOP,&loop,sizeof(loop));
>    ```
>
> 



> 当level为IPPROTO_TCP时，option_name 可以有以下取值：
>
>   +  TCP_NODELAY 是唯一使用 IPPROTO_TCP 层的选项;
>   +  



## select函数

```c
#include<sys/time.h>
#include<sys/types.h>
#include<unistd.h>
int select(int maxfdp,fd_set *readfds,fd_set *writefds,fd_set *errorfds,struct timeval*timeout);
```

<font color=blue>先说明两个结构体：</font>

+ 第一，struct fd_set 可以理解为一个集合，这个集合中存放的是文件描述符 (filedescriptor)，即文件句柄，这可以是我们所说的普通意义的文件，当然 Unix 下任何设备、管道、FIFO 等都是文件形式，全部包括在内，所以毫无疑问一个 socket 就是一个文件，socket 句柄就是一个文件描述符。fd_set 集合可以通过一些宏由人为来操作，比如清空集合 FD_ZERO (fd_set *)，将一个给定的文件描述符加入集合之中 FD_SET (int ,fd_set*)，将一个给定的文件描述符从集合中删除 FD_CLR (int,fd_set*)，检查集合中指定的文件描述符是否可以读写 FD_ISSET (int ,fd_set* )。 
+ 第二，struct timeval 是一个大家常用的结构，用来代表时间值，有两个成员，一个是秒数，另一个是毫秒数。

<font color=blue>参数：</font>

+ int maxfdp 是一个整数值，是指集合中所有文件描述符的范围，即所有文件描述符的最大值加 1，不能错！在 Windows 中这个参数的值无所谓，可以设置不正确。
+ fd_set * readfds 是指向 fd_set 结构的指针，这个集合中应该包括文件描述符，我们是要监视这些文件描述符的读变化的，即我们关心是否可以从这些文件中读取数据了，如果这个集合中有一个文件可读，select 就会返回一个大于 0 的值，表示有文件可读，如果没有可读的文件，则根据 timeout 参数再判断是否超时，若超出 timeout 的时间，select 返回 0，若发生错误返回负值。可以传入 NULL 值，表示不关心任何文件的读变化。
+ fd_set * writefds 是指向 fd_set 结构的指针，这个集合中应该包括文件描述符，我们是要监视这些文件描述符的写变化的，即我们关心是否可以向这些文件中写入数据了，如果这个集合中有一个文件可写，select 就会返回一个大于 0 的值，表示有文件可写，如果没有可写的文件，则根据 timeout 参数再判断是否超时，若超出 timeout 的时间，select 返回 0，若发生错误返回负值。可以传入 NULL 值，表示不关心任何文件的写变化。
+ fd_set * errorfds 同上面两个参数的意图，用来监视文件错误异常。
+ struct timeval * timeout 是 select 的超时时间，这个参数至关重要，它可以使 select 处于三种状态，第一，若将 NULL 以形参传入，即不传入时间结构，就是将 select 置于阻塞状态，一定等到监视文件描述符集合中某个文件描述符发生变化为止；第二，若将时间值设为 0 秒 0 毫秒，就变成一个纯粹的非阻塞函数，不管文件描述符是否有变化，都立刻返回继续执行，文件无变化返回 0，有变化返回一个正值；第三，timeout 的值大于 0，这就是等待的超时时间，即 select 在 timeout 时间内阻塞，超时时间之内有事件到来就返回了，否则在超时后不管怎样一定返回，返回值同上述。

<font color=blue>返回值：</font>**返回值：返回状态发生变化的描述符总数。**

+ 负值：select 错误；

+ 正值：某些文件可读写或出错；

+ 0：等待超时，没有可读写或错误的文件；



> 实例1
>
> ```c
> #include <sys/types.h>
> #include <sys/time.h>
> #include <stdio.h>
> #include <fcntl.h>
> #include <sys/ioctl.h>
> #include <unistd.h>
> 
> int main()
> {
>  char buffer[128];
>  int result, nread;
>  fd_set inputs, testfds;
>  struct timeval timeout;
>  FD_ZERO(&inputs);   //用select函数之前先把集合清零
>  FD_SET(0, &inputs); //把要检测的句柄——标准输入（0），加入到集合里。
>  while (1)
>  {
>      testfds = inputs;
>      timeout.tv_sec = 2;
>      timeout.tv_usec = 500000;
>      result = select(FD_SETSIZE, &testfds, (fd_set *)0, (fd_set *)0, &timeout);
>      switch (result)
>      {
>      case 0:
>          printf("timeout/n");
>          break;
>      case -1:
>          perror("select");
>          exit(1);
>      default:
>          if (FD_ISSET(0, &testfds))
>          {
>              ioctl(0, FIONREAD, &nread); //取得从键盘输入字符的个数，包括回车。
>              if (nread == 0)
>              {
>                  printf("keyboard done/n");
>                  exit(0);
>              }
>              nread = read(0, buffer, nread);
>              buffer[nread] = 0;
>              printf("read %d from keyboard: %s", nread, buffer);
>          }
>          break;
>      }
>  }
>  return 0;
> }
> ```
>
> 实例2
>
> ```c
> //server.c
> 
> 
> //client.c
> 
> 
> 
> ```
>
> 



## gethostbyname函数

域名仅仅是 IP 地址的一个助记符，目的是方便记忆，通过域名并不能找到目标计算机，通信之前必须要将域名转换成 IP 地址。

gethostbyname () 函数可以完成这种转换，它的原型为：

```c
struct hostent *gethostbyname(const char *hostname);
```

hostname 为主机名，也就是域名。使用该函数时，只要传递域名字符串，就会返回域名对应的 IP 地址。返回的地址信息会装入hostent 结构体，该结构体的定义如下：

```c
struct hostent{
    char *h_name;  //official name
    char **h_aliases;  //alias list
    int  h_addrtype;  //host address type
    int  h_length;  //address lenght
    char **h_addr_list;  //address list
}
```

从该结构体可以看出，不只返回 IP 地址，还会附带其他信息，各位读者只需关注最后一个成员 h_addr_list。下面是对各成员的说明：

- h_name：官方域名（Official domain name）。官方域名代表某一主页，但实际上一些著名公司的域名并未用官方域名注册。
- h_aliases：别名，可以通过多个域名访问同一主机。同一 IP 地址可以绑定多个域名，因此除了当前域名还可以指定其他域名。
- h_addrtype：gethostbyname () 不仅支持 IPv4，还支持 IPv6，可以通过此成员获取 IP 地址的地址族（地址类型）信息，IPv4 对应 AF_INET，IPv6 对应 AF_INET6。
- h_length：保存 IP 地址长度。IPv4 的长度为 4 个字节，IPv6 的长度为 16 个字节。
- h_addr_list：这是最重要的成员。通过该成员以整数形式保存域名对应的 IP 地址。对于用户较多的服务器，可能会分配多个 IP 地址给同一域名，利用多个服务器进行均衡负载。


hostent 结构体变量的组成如下图所示：

![hostent 结构体的组成](http://c.biancheng.net/uploads/allimg/190219/135F5L39-0.jpg)


下面的代码主要演示 gethostbyname () 的应用，并说明 hostent 结构体的特性：

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

```c
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netdb.h>
#include <stdio.h>

extern int h_errno;

int main(int argc, char **argv)
{
    if (argc != 2)
    {
        printf("Use example: %s www.google.com\n", *argv);
        return -1;
    }

    char *name = argv[1];
    struct hostent *hptr;

    hptr = gethostbyname(name);
    if (hptr == NULL)
    {
        printf("gethostbyname error for host: %s: %s\n", name, hstrerror(h_errno));
        return -1;
    }
    //输出主机的规范名
    printf("\tofficial: %s\n", hptr->h_name);

    //输出主机的别名
    char **pptr;
    char str[INET_ADDRSTRLEN];
    for (pptr = hptr->h_aliases; *pptr != NULL; pptr++)
    {
        printf("\ttalias: %s\n", *pptr);
    }

    //输出ip地址
    switch (hptr->h_addrtype)
    {
    case AF_INET:
        pptr = hptr->h_addr_list;
        for (; *pptr != NULL; pptr++)
        {
            printf("\taddress: %s\n",
                   inet_ntop(hptr->h_addrtype, hptr->h_addr, str, sizeof(str)));
        }
        break;
    default:
        printf("unknown address type\n");
        break;
    }

    return 0;
}
```





#   [Linux/C时间函数]()

```tex
  time () 提供了秒级的精确度，用 time () 函数结合其他函数（如：localtime、gmtime、asctime、ctime）可以获得当前系统时间或是标准时间。如果需要更高的时间精确度，就需要 `struct timespec` 和 `struct timeval `来处理。
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/icRxcMBeJfc8yNwJibB9svwicK8R3aCd9j3Xmop5TsTzAR7HHnicPyyWiaPZX7Ro62d3CmsLtdYXLjKdrGZ2Gpw96iaA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

+ 通过系统调用函数 time () 可以从内核获得一个类型为 time_t 的 1 个值，该值叫 calendar 时间，即从 1970 年 1 月 1 日的 UTC 时间从 0 时 0 分 0 妙算起到现在所经过的秒数。而该时间也用于纪念 UNIX 的诞生。
+ 函数 gmtime ()、localtime () 可以将 calendar 时间转变成 struct tm 结构体类型变量中。通过该结构体成员可以很方便的得到当前的时间信息。我们也可以通过函数 mktime 将该类型结构体的变量转变成 calendar 时间。
+ asctime () 和 ctime () 函数产生形式的 26 字节字符串，这与 date 命令的系统默认输出形式类似：Tue Feb 10 18:27:38 2020/n/0.
+ strftime () 将一个 struct tm 结构格式化为一个字符串。



##   一般时间函数

###   相关结构体

```c
#include<types.h>
#ifndef _CLOCK_T
#define _CLOCK_T
	typedef    long   clock_t;
#endif


// time_t  类型：长整型，一般用来表示从 1970-01-01 00:00:00 时以来的秒数，精确度：秒；由函数 time () 获取；
#define _TIME_T
	typedef   long   time_t;
#endif

#include <sys/timeb.h>
// struct timeb 结构：它有两个主要成员，一个是秒，另一个是毫秒；精确度：毫秒 (10E-3 秒)；
struct  timeb{
    time_t   time;                      /* 为 1970-01-01 至今的秒数 */
    unsigned   short   millitm; /* 千分之一秒即毫秒 */
    short   timezonel;               /* 为目前时区和 Greenwich 相差的时间，单位为分钟 */
    short   dstflag;                   /* 为日光节约时间的修正状态，如果为非 0 代表启用日光节约时间修正 */
};

// struct timespec 有两个成员，一个是秒，一个是纳秒，所以最高精确度是纳秒,精确度：纳秒 (10E-9)。
struct  timespec
{
	time_t    tv_sec;        // 秒
	long       tv_nsec;       //纳秒
};

//struct  timeval 结构，它有两个成员；一个是秒，另一个表示微秒，所以最高精确度是微秒，精确度：微秒 (10E-6)；
struct  timeval
{
    long  tv_sec;    //秒
    long  tv_usec;  // 微秒
};

//timespec和timeval两者的区别是 timespec 的第二个参数是纳秒数，而 timeval 的第二个参数是毫秒数。

//struct  timezone 结构的定义为：
struct  timezone
{
    int  tz_minuteswest;
    int  tz_dsttime;
};

// 结构 tm 的定义为
struct tm
{
    int   tm_sec;    //tm_sec 代表目前秒数，正常范围为 0-59，但允许至 61 秒
    int   tm_min;    // tm_min 代表目前分数，范围 0-59
    int   tm_hour;    //tm_hour 从午夜算起的时数，范围为 0-23
    int   tm_mday;    //tm_mday 目前月份的日数，范围 01-31
    int   tm_mon;    // tm_mon 代表目前月份，从一月算起，范围从 0-11
    int   tm_year;   //tm_year 从 1900 年算起至今的年数
    int   tm_wday;   // tm_wday 一星期的日数，从星期一算起，范围为 0-6
    int   tm_yday;   // tm_yday 从今年 1 月 1 日算起至今的天数，范围为 0-365
    int tm_isdst;    //tm_isdst 日光节约时间的旗标
};

```



###    time函数

```c
// 头文件：time.h
// 函数定义：
 time_t   time (time_t*  lpt)；
```

说明： 返回从1970年1月1日的UTC时间从0时0分0妙算起到现在所经过的秒数。

```c
#include<stdio.h>
#include<time.h>
int main(){
 time_t timep;

 long seconds = time(&timep);
 printf("%ld\n",seconds);
 printf("%ld\n",timep);
 return 0;
}
```







###   ctime函数

```
char *ctime(const time_t *timep);
```

说明：将参数所指的time_t结构中的信息转换成真实世界的时间日期表示方法，然后将结果以字符串形式返回。
注意这个是本地时间。

```c
#include <stdio.h>
#include<time.h>
int main(void) {
 time_t timep;

 time(&timep);
 printf("%s\n",ctime(&timep));
 return 0;
}
```



###    gmtime 函数

```c
struct tm *gmtime(const time_t *timep);
```

说明：将参数timep所指的time_t结构中的信息转换成真实世界所使用的时间日期表示方法，然后将结果由结构tm返回。此函数返回的时间日期未经时区转换，而是UTC时间。





```c
#include <stdio.h>
#include<time.h>

int main(void){
    char *wday[] = {"Sun","Mon","Tue","Wed","Thu","Fri","Sat"};

 	time_t timep;
	struct tm *p;

 	time(&timep);
 	p = gmtime(&timep);
 	printf("%d/%d/%d ",(1900+p->tm_year),(1+p->tm_mon),p->tm_mday);
 	printf("%s %d:%d:%d\n",wday[p->tm_wday],p->tm_hour,p->tm_min,p->tm_sec);
 	return 0;
}
```





###    strftime 函数

```c
#include <time.h>
size_t strftime(char *s, size_t max, const char *format,const struct tm *tm);
```

说明：
类似于snprintf函数，我们可以根据format指向的格式字符串，将struct tm结构体中信息输出到s指针指向的字符串中，最多为max个字节。当然s指针指向的地址需提前分配空间，比如字符数组或者malloc开辟的堆空间。
其中，格式化字符串各种日期和时间的详细的确切表示方法有如下多种，我们可以根据需要来格式化各种各样的含时间字符串。

+ %a 星期几的简写

+  %A 星期几的全称

+ %b 月分的简写

+ %B 月份的全称

+ %c 标准的日期的时间串

+ %C 年份的前两位数字

+ %d 十进制表示的每月的第几天

+ %D 月/天/年

+ %e 在两字符域中，十进制表示的每月的第几天

+ %F 年-月-日

+ %g 年份的后两位数字，使用基于周的年

+  %G 年分，使用基于周的年

+ %h 简写的月份名

+ %H 24小时制的小时

+ %I 12小时制的小时

+ %j 十进制表示的每年的第几天

+ %m 十进制表示的月份

+ %M 十时制表示的分钟数

+ %n 新行符

+ %p 本地的AM或PM的等价显示

+ %r 12小时的时间

+ %R 显示小时和分钟：hh:mm

+ %S 十进制的秒数

+  %t 水平制表符

+ %T 显示时分秒：hh:mm:ss

+ %u 每周的第几天，星期一为第一天 （值从0到6，星期一为0）

+ %U 第年的第几周，把星期日做为第一天（值从0到53）

+ %V 每年的第几周，使用基于周的年

+ %w 十进制表示的星期几（值从0到6，星期天为0）

+ %W 每年的第几周，把星期一做为第一天（值从0到53）

+ %x 标准的日期串

+ %X 标准的时间串

+ %y 不带世纪的十进制年份（值从0到99）

+ %Y 带世纪部分的十制年份

+ %z，%Z 时区名称，如果不能得到时区名称则返回空字符。

+ %% 百分号
  返回值：
  成功的话返回格式化之后s字符串的字节数，不包括null终止字符，但是返回的字符串包括null字节终止字符。否则返回0，s字符串的内容是未定义的。值得注意的是，这是libc4.4.4以后版本开始的。对于一些的老的libc库，比如4.4.1，如果给定的max较小的话，则返回max值。即返回字符串所能容纳的最大字节数。

  ```c
  #include <stdio.h>
  #include <time.h>
  #define BUFLEN 255
  int main(int argc, char **argv)
  {
  	time_t t = time( 0 );
      char tmpBuf[BUFLEN];
      strftime(tmpBuf, BUFLEN, "%Y%m%d%H%M%S", localtime(&t)); //format date a
      printf("%s\n",tmpBuf);
      return 0;
  }
  ```

###    asctime 函数

```c
char *asctime(const struct tm *timeptr);
```

 将参数timeptr所指的struct tm结构中的信息转换成真实时间所使用的时间日期表示方法，结果以字符串形态返回。与ctime()函数不同之处在于传入的参数是不同的结构。
返回值：返回的也是UTC时间。

```c
#include <stdio.h>
#include <stdlib.h>
#include<time.h>
int main(void) {
    time_t timep;
    time(&timep);
    printf("%s\n",asctime(gmtime(&timep)));
    return EXIT_SUCCESS;
}
```



###    localhost 函数

```c
struct tm *localhost(const time_t *timep);
//取得当地目前的时间和日期
```



```c
#include <stdio.h>
#include <stdlib.h>
#include<time.h>

int main(void) {
    char *wday[] = {"Sun","Mon","Tue","Wed","Thu","Fri","Sat"};
 	time_t timep;
 	struct tm *p;

 	time(&timep);
 	p = localtime(&timep);
 	printf("%d/%d/%d ",(1900+p->tm_year),(1+p->tm_mon),p->tm_mday);
 	printf("%s %d:%d:%d\n",wday[p->tm_wday],p->tm_hour,p->tm_min,p->tm_sec);
 	return EXIT_SUCCESS;
}
```



###   mktime 函数

````c
time_t mktime(struct tm *timeptr);
````

 用来将参数timeptr所指的tm结构数据转换成从1970年1月1日的UTC时间从0时0分0妙算起到现在所经过的秒数。

```c
#include <stdio.h>
#include <stdlib.h>
#include<time.h>

int main(void) {
 time_t timep;
 struct tm *p;

 time(&timep);
 printf("time():%ld\n",timep);
 p = localtime(&timep);
 timep = mktime(p);
 printf("time()->localtime()->mktime():%ld\n",timep);
 return EXIT_SUCCESS;
}
```

###   gettimeofday 函数

```c
int  gettimeofday(struct  timeval*  tv，struct  timezone*  tz);
//该函数会提取系统当前时间，并把时间分为秒和微秒两部分填充到结构 struct  timeval 中；同时把当地的时区信息填充到结构 struct  timezone 中；
// 返回值：成功则返回 0，失败返回－1，错误代码存于 errno。附加说明 EFAULT 指针 tv 和 tz 所指的内存空间超出存取权限。


//struct  timeval 结构，它有两个成员；一个是秒，另一个表示微秒，精确度：微秒 (10E-6)；
struct  timeval
{
    long  tv_sec;
    long  tv_usec;
}

//struct  timezone 结构的定义为：
struct  timezone
{
    int  tz_minuteswest;
    int  tz_dsttime;
}
```



```c
#include <stdio.h>
#include <stdlib.h>
#include<time.h>
#include<sys/time.h>

int main(void) {
    struct timeval tv;
    struct timezone tz;
    gettimeofday(&tv,&tz);
    printf("tv_sec :%d\n",tv.tv_sec);
    printf("tv_usec: %d\n",tv.tv_usec);
    printf("tz_minuteswest:%d\n",tz.tz_minuteswest);
    printf("tz_dsttime:%d\n",tz.tz_dsttime);
    return EXIT_SUCCESS;
}
```



###    ftime函数

```c
//表头文件：
#include <sys/timeb.h>
//函数定义：
int ftime (struct timeb *tp);
//函数说明：ftime()将目前日期由 tp 所指的结构返回,ftime () 函数取得目前的时间和日期。

#include <sys/timeb.h>
// struct timeb 结构：它有两个主要成员，一个是秒，另一个是毫秒；精确度：毫秒 (10E-3 秒)；
struct  timeb{
    time_t   time;                      /* 为 1970-01-01 至今的秒数 */
    unsigned   short   millitm;        /* 千分之一秒即毫秒 */
    short   timezonel;               /* 为目前时区和 Greenwich 相差的时间，单位为分钟 */
    short   dstflag;                   /* 为日光节约时间的修正状态，如果为非 0 代表启用日光节约时间修正 */
};

```



###    clock函数

```c
clock_t   clock(void);
// 该函数以微秒的方式返回 CPU 的时间；
```



####   gethrestime、gethrestime_lasttick函数



```c
#include<time_impl.h>
void   gethrestime(timespec_t*);
void   gethrestime_lasttick(timespec_t*);

// struct timespec 有两个成员，一个是秒，一个是纳秒，所以最高精确度是纳秒。
struct  timespec
{
	time_t    tv_sec;
	long       tv_nsec;
};
```









###   定时器相关函数

最强大的定时器接口来自 POSIX 时钟系列，其创建、初始化以及删除一个定时器的行动被分为三个不同的函数：timer_create()(创建定时器)、timer_settime()(初始化定时器) 以及 timer_delete (销毁它)。

linux下timer_t定时器的使用，总共有3个函数，timer_create()  timer_settime()  timer_gettime()。



####   相关结构体

````c
#include <signal.h>

union sigval {          /* Data passed with notification */
    int      sival_int;         /* Integer value */
    void   *sival_ptr;      /* Pointer value */
};

struct sigevent {
    int      sigev_notify; /* Notification method */
    int      sigev_signo;  /* Notification signal */
    union sigval sigev_value;  /* Data passed with notification */
    void (*sigev_notify_function) (union sigval);/* Function used for thread notification (SIGEV_THREAD) */
    void *sigev_notify_attributes;/* Attributes for notification thread (SIGEV_THREAD) */
    pid_t sigev_notify_thread_id; /* ID of thread to signal (SIGEV_THREAD_ID) */
};


struct itimerspec
  {
    struct timespec it_interval;  //该值表示定时器启动后定时周期是多少
    struct timespec it_value;  //该值表示过多久开始启动定时器
  };

// struct timespec 有两个成员，一个是秒，一个是纳秒，所以最高精确度是纳秒。
struct  timespec
{
	time_t    tv_sec;        // 秒
	long       tv_nsec;       //纳秒
};
````





####   time_creat()函数

```c
#include <signal.h>
#include <time.h>
int timer_create(clockid_t clock_id, struct sigevent *evp, timer_t *timerid);
```

函数说明：创建一个 POSIX 标准的定时器，函数返回值：返回 0 表示成功，返回 - 1 表示失败。
参数说明：

+ clock_id：系统时钟的宏，该参数表明了定时器是基于哪个系统时钟来创建的。常见的宏有以下：

  ```c
  #define CLOCK_REALTIME    0
  //表示从1970.1.1到目前系统时间，属于相对时间

  #define CLOCK_MONOTONIC   1
  //单调的时间，也是绝对的时间，表示系统开启到目前的时间

  #define CLOCK_PROCESS_CPUTIME_ID  2
  // 本进程到当前代码系统CPU花费的时间

  #define CLOCK_THREAD_CPUTIME_ID  3
  //本线程到当前代码系统CPU花费的时间
  ```

  除了以上宏，还有七种系统时钟的宏，这里就不一一介绍了，在 time.h 中可以查看。

+ evp: 环境值，结构体struct sigevent变量的地址，其结构体主要成员以下有如下：

  ```c
  struct sigevent {
      int   sigev_notify; /* Notification method */
      int   sigev_signo;  /* Notification signal */
      union   sigval   sigev_value;  /* Data passed with notification */
      void (*sigev_notify_function) (union sigval);/* Function used for thread notification (SIGEV_THREAD) */
      void  *sigev_notify_attributes;/* Attributes for notification thread (SIGEV_THREAD) */
      pid_t  sigev_notify_thread_id; /* ID of thread to signal (SIGEV_THREAD_ID) */
  };
  
  union sigval {          /* Data passed with notification */
      int  sival_int;         /* Integer value */
      void   *sival_ptr;      /* Pointer value */
  };
  ```

  sigev_notify 表示定时器到期后需要采取的行为，它的取值有如下几种：

  ```c
  //sigev_notify 表示定时器到期后需要采取的行为，它的取值有如下几种：
  enum
  {
  	SIGEV_SIGNAL = 0, // 设置该值时说明定时器到期时会产生信号，该信号由sigev_signo指定,向进程发送 sigev_signo 中指定的信号，这涉及到 sigaction 的使用;
  	SIGEV_NONE   // 设置该值防止定时器到期时产生信号,空的提醒，事件发生时不做任何事情;
  	SIGEV_THREAD //通过线程创建传递信号，通知进程在一个新的线程中启动 sigev_notify_function 函数，函数的实参是 sigev_value，系统 API 自动启动一个线程，我们不用显式启动;
  	SIGEV_THREAD_ID //表示信号会发送到指定的线程;
  }
 
  ```

  sigev_signo 表示定时器到期时将会发出的信号。这些信号在 signum.h 中有定义。常用的信号由如下：

  ```c
  #define SIGALRM 		14 // 时钟信号
  #define SIGUSR1		10 //用户自定义信号1
  #define SIGUSR2 		12  //用户自定义信号2
  ...
  ```

  还有最后一个成员是 sigev_value，它则是来绑定定时器的。表示这些设置的环境将会作用到哪个定时器上。

+ 该函数最后一个参数是 timerid，表示定时器的 id，定时器标识符，结构体timer_t变量的地址，定时器创建成功，将会产生一个 id，而该 id 就会被赋值给 timerid。

####   timer_settime函数

```tex
	经过上述函数创建了一个定时器之后，还需要设置定时器的定时周期以及启动时钟周期（即过了多久开始启动时钟）。这项工作交由 timer_settime 接口来完成。
```

函数返回值：返回 0 表示成功，返回 - 1 表示失败。

其函数原型如下：

```c
#include <time.h>
int timer_settime (timer_t timerid, int flags, const struct itimerspec *value, struct itimerspec *old_value);
```

函数参数说明：

+ timer_id：定时器的 ID，指定初始化的定时器，由 timer_create 函数产生。

+ flags：0 表示相对时间，1 表示绝对时间。

+ value：保存一个结构体的地址，该结构体就包含了定时周期以及启动周期。
  结构体如下：

  ```c
  struct itimerspec
  {
      struct timespec it_interval;  //该值表示定时器启动后定时周期是多少
      struct timespec it_value;    //该值表示过多久开始启动定时器
  };
  // it_value 用于指定当前的定时器到期时间。当定时器到期，it_value 的值会被更新成 it_interval 的值。如果 it_interval 的值为 0，则定时器不是一个时间间隔定时器，一旦 it_value 到期就会回到未启动状态。
  ```

  ``` c
    // 而结构体 timespec 则有两个成员，分别是秒和纳秒，如下：
    struct timespec
    {
      __time_t tv_sec;		        /* Seconds.秒  */
      __syscall_slong_t tv_nsec;	/* Nanoseconds.纳秒  */
    };
  ```



可见该定时器的精准度还是非常高的。

+ 参数 old_value 通常情况下都是取 0 值或者 NULL。

#### **timer_gettime**函数

```c
#include <time.h>
int timer_gettime(timer_t timerid, struct itimerspec * curr_value);
```

功能：获得定时器时间值；

参数：

+ timerid 定时器标识；


####   timer_delete函数

任务完成后，不需要定时器则可以使用下面的接口来删除定时器。

函数原型：

```c
int timer_delete (timer_t __timerid);
```

函数只有一个参数，即定时器的 ID，表明删除指定 id 的定时器。

好了，现在定时器有了，并且也可以设置定时器到期时产生的信号，现在就差信号产生时，怎么去触发执行指定的任务了。这就需要 signal 函数介入了。

> 实例
>
> >  接下来看一个简单的小例子来了解一下定时器的使用。该程序的功能就是在 while 中隔 1s 打印一个字符串，10s 后退出 while 结束打印。
>
> ```c
> #include<stdio.h>
> #include<signal.h>
> #include<time.h>
> #include<string.h>
> #include <unistd.h>
> 
> static bool flag = true;
> timer_t timeid;  //定义一个全局的定时器id
> 
> void task(int i)
> {
>     printf("task start\n");
>     flag = false;
> }
> 
> void create_timer()
> {
> /****创建定时器***********/
>     struct sigevent evp;  //环境结构体
>     int ret = 0;
> 
>     memset(&evp, 0, sizeof(struct sigevent));
> 
>     evp.sigev_value.sival_ptr = &timeid;    //绑定i定时器
>     evp.sigev_notify = SIGEV_SIGNAL;  //设置定时器到期后触发的行为是为发送信号
>     evp.sigev_signo = SIGUSR1;  //设置信号为用户自定义信号1
>     signal(SIGUSR1, task);  //绑定产生信号后调用的函数
> 
>     ret = timer_create(CLOCK_REALTIME, &evp, &timeid);  //创建定时器
>     if( ret  == 0)
>     {
>         printf("timer_create ok\n");
>     }
> }
> 
> void init_timer()
> {
>     int ret = 0;
>     struct itimerspec ts;
>     ts.it_interval.tv_sec = 1;  //设置定时器的定时周期是1s
>     ts.it_interval.tv_nsec = 0;
>     ts.it_value.tv_sec = 10;  //设置定时器10s后启动
>     ts.it_value.tv_nsec = 0;
> 
>     ret = timer_settime(timeid, 0, &ts, NULL);  //初始化定时器
>     if( ret ==0)
>         printf("timer_settime ok\n");
> }
> 
> int main()
> {
>     create_timer();
>     init_timer();
>     while(flag)
>     {
>         printf("ss\n");
>         usleep(1000*1000);
>     }
> }
> ```
>
> 






# errno全局变量

[《errno 全局变量及使用细则，C 语言 errno 全局变量完全攻略》](http://c.biancheng.net/c/errno/)

在 C 语言中，对于存放错误码的全局变量 errno，相信大家都不陌生。为防止和正常的返回值混淆，系统调用一般并不直接返回错误码，而是将错误码（是一个整数值，不同的值代表不同的含义）存入一个名为 errno 的全局变量中，errno 不同数值所代表的错误消息定义在 <errno.h> 文件中。如果一个系统调用或库函数调用失败，可以通过读出 errno 的值来确定问题所在，推测程序出错的原因，这也是调试程序的一个重要方法。

配合 perror 和 strerror 函数，还可以很方便地查看出错的详细信息。其中:

- perror 在 <stdio.h> 中定义，用于打印错误码及其消息描述；
- strerror 在 <string.h> 中定义，用于获取错误码对应的消息描述；

一般当进程收到信号时都会中断正在进行的系统调用，而去处理到来的信号，当处理完成时系统调用会返回－1 并且 errno == EINTR。
一般的处理方法：

1. 在你的进程设置系统调用自动恢复处理；这时被中断的系统调用会自动恢复而不会出错返回；
2. 当收到 errno == EINTR 时继续执行系统调用；
3. 在执行系统调用前阻塞信号的到达，在执行完系统调用后放开对信号的阻塞；


需要特别强调的是，并不是所有的库函数都适合使用 errno 全局变量。



> 调用 errno 之前必须先将其清零

在 C 语言中，如果系统调用或库函数被正确地执行，那么 errno 的值不会被清零。换句话说，errno 的值只有在一个库函数调用发生错误时才会被设置，当库函数调用成功运行时，errno 的值不会被修改，当然也不会主动被置为 0。也正因为如此，在实际编程中进行错误诊断会有不少问题。

例如，在一段示例代码中，首先执行函数 A 的调用，如果函数 A 在执行中发生了错误，那么 errno 的值将被修改。接下来，在不对 errno 的值做任何处理的情况下，继续直接执行函数 B 的调用，如果函数 B 被正确地执行，那么 errno 将还保留着函数 A 发生错误时被设置的值。也正是这个原因，我们不能通过测试 errno 的值来判断是否存在错误。

由此可见，在调用 errno 之前，应该首先对函数的返回值进行判断，通过对返回值的判断来检查函数的执行是否发生了错误。如果通过检查返回值确认函数调用发生了错误，那么再继续利用 errno 的值来确认究竟是什么原因导致了错误。

但是，如果一个函数调用无法从其返回值上判断是否发生了错误时，那么将只能通过 errno 的值来判断是否出错以及出错的原因。对于这种情况，必须在调用函数之前先将 errno 的值手动清零，否则，errno 的值将有可能够发生上面示例所展示的情况。

例如，当调用 fopen 函数发生错误时，它将会去修改 errno 的值，这样外部的代码就可以通过判断 errno 的值来区分 fopen 内部执行时是否发生错误，并且根据 errno 值的不同来确定具体的错误类型。如下面的示例代码所示：

```c
int main(void)
{
    /*调用errno之前必须先将其清零*/
    errno=0;
    FILE *fp = fopen("test.txt","r");
    if(errno!=0)
    {
        printf("errno值： %d\n",errno);
        printf("错误信息： %s\n",strerror(errno));
    }
}
```

在这里，假设 “test.txt” 是一个根本不存在的文件。因此，在调用 fopen 函数尝试打开一个并不存在的文件时将发生错误，同时修改 errno 的值。这时，fopen 函数会将 errno 指向的值修改为 2。我们通过 stderror 函数可以看到错误代码 “2” 的意思是 “No such file or directory”，如图 3 所示。

![图 3 示例代码的运行结果](http://c.biancheng.net/uploads/allimg/180910/2-1P910151023426.jpg)
从上面的示例可以看出，使用 errno 来报告错误看起来似乎非常简单完美，但其实情况并非如此。前面也阐述过，在 C99 中，并没有在描述 fopen 时提到 errno。但是，POSIX.1 却声明了当 fopen 遇到一个错误时，它将返回 NULL，并且为 errno 设置一个值以提示这个错误，这就暗示一个遵循了 C99 但不遵循 POSIX 的程序不应该在调用 fopen 之后再继续检查 errno 的值。因此，下面的写法完全合乎要求：

```c
int main(void)
{
    FILE *fp = fopen("test.txt","r");
    if(fp==NULL)
    {
        /*...*/
    }
}
```

但是，上面也说过，在 POSIX 标准中，当 fopen 遇到一个错误的时候将返回 NULL，并且为 errno 设置一个值以提示这个错误。因此，在遵循 POSIX 标准中，应该首先检查 fopen 是否返回 NULL 值，如果返回，再继续检查 errno 的值以确认产生错误的具体信息，如下面的代码所示：

```c
int main(void)
{
    /*调用errno之前必须先将其清零*/
    errno=0;
    FILE *fp = fopen("test.txt","r");
    if(fp==NULL)
    {
        if（errno!=0）
        {
            printf("errno值: %d\n",errno);
            printf("错误信息：%s\n",strerror(errno));
        }
    }
}
```

其实，即使系统调用或者库函数正确执行，也不能够保证 errno 的值不会被改变。因此，在没有发生错误的情况下，fopen 也有可能修改的 errno 值。先检查 fopen 的返回值，再检查 errno 的值才是正确的做法。

除此之外，建议在使用 errno 的值之前，必须先将其值赋给另外一个变量保存起来，因为很多函数（如 fprintf）自身就可能会改变 errno 的值。

>  避免重定义 errno

对于 errno，它是一个由 ISO C 与 POSIX 标准定义的符号。早些时候，POSIX 标准曾经将 errno 定义成 “extern int errno” 这种形式，但现在这种定义方式比较少见了，那是因为这种形式定义的 errno 对多线程来说是致命的。

在多线程环境下，errno 变量是被多个线程共享的，这样就可能引发如下情况：线程 A 发生某些错误而改变了 errno 的值，那么线程 B 虽然没有发生任何错误，但是当它检测 errno 的值时，线程 B 同样会以为自己发生了错误。

我们知道，在多线程环境中，多个线程共享进程地址空间，因此就要求每个线程都必须有属于自己的局部 errno，以避免一个线程干扰另一个线程。其实，现在的大多部分编译器都是通过将 errno 设置为线程局部变量的实现形式来保证线程之间的错误原因不会互相串改。

例如，在 Linux 下的 GCC 编译器中，标准的 errno 在 “/usr/include/errno.h” 中的定义如下：

```c
/* Get the error number constants from the system-specific file.
   This file will test __need_Emath and _ERRNO_H.  */
#include <bits/errno.h>
#undef   __need_Emath
#ifdef   _ERRNO_H
/* Declare the `errno' variable， unless it's defined as a macro by bits/errno.h.  This is the case in GNU， where it is a per-thread variable.  This redeclaration using the macro still works， but it will be a function declaration without a prototype and may trigger a -Wstrict-prototypes warning.  */
#ifndef   errno
extern int errno；
#endif
```

其中，errno 在 “/usr/include/bits/errno.h” 文件中的具体实现如下：

```c
# ifndef __ASSEMBLER__
/* Function to get address of global 'errno' variable.  */
extern int *__errno_location (void) __THROW __attribute__ ((__const__));
# if !defined _LIBC || defined _LIBC_REENTRANT
/* When using threads，errno is a per-thread value.  */
# define errno (*__errno_location ())
# endif
# endif /* !__ASSEMBLER__ */
# endif /* _ERRNO_H */
```

这样，通过 “extern int*__errno_location（void）__THROW__attribute__((__const__));” 与 “#define errno (*__errno_location ())” 定义，使每个线程都有自己的 errno，不管哪个线程修改 errno 都是修改自己的局部变量，从而达到线程安全的目的。

除此之外，如果要在多线程环境下正确使用 errno，首先需要确保 __ASSEMBLER__ 没有被定义，同时 _LIBC 没被定义或定义了 _LIBC_REENTRANT。可以通过下面的程序来在自己的开发环境中测试这几个宏的设置：

```c
int main(void)
{
#ifndef __ASSEMBLER__
    printf( "__ASSEMBLER__ is not defined！\n" );
#else
    printf( "__ASSEMBLER__ is defined！\n" );
#endif
#ifndef __LIBC
    printf( "__LIBC is not defined\n" );
#else
    printf( "__LIBC is defined！\n" );
#endif
#ifndef _LIBC_REENTRANT
    printf( "_LIBC_REENTRANT is not defined\n" );
#else
    printf( "_LIBC_REENTRANT is defined！\n" );
#endif
    return 0;
}
```

该程序的运行结果为：

```bash
__ASSEMBLER__ is not defined！
__LIBC is not defined
_LIBC_REENTRANT is not defined
```



由此可见，在使用 errno 时，只需要在程序中简单地包含它的头文件 “errno.h” 即可，千万不要多此一举，在程序中重新定义它。如果在程序中定义了一个名为 errno 的标识符，其行为是未定义的。

>  避免使用 errno 检查文件流错误

上面已经阐述过，在 POSIX 标准中，可以通过 errno 值来检查 fopen 函数调用是否发生了错误。但是，对特定文件流操作是否出错的检查则必须使用 ferror 函数，而不能够使用 errno 进行文件流错误检查。如下面的示例代码所示：

```c
int main(void)
{
    FILE* fp=NULL;
    /*调用errno之前必须先将其清零*/
    errno=0;
    fp = fopen("Test.txt","w");
    if(fp == NULL)
    {
        if(errno!=0)
        {
            /*处理错误*/
        }
    }
    else
    {
        /*错误地从fp所指定的文件中读取一个字符*/
        fgetc(fp);
        /*判断是否读取出错*/
        if(ferror(fp))
        {
            /*处理错误*/
            clearerr(fp);
        }
        fclose(fp);
        return 0;
    }
}
```

> 多线程安全

有些教程说 errno 展开后是一个 int 类型的全局变量，如下所示：

```c
extern int _errno;
#define errno _errno
```

很多单线程库确实也是这么做的；但是这种方案仅适用于单线程程序，不适用于多线程程序。

`全局变量的作用范围是整个程序，是所有的源文件，而不仅限于当前的源文件。`

在多线程程序中，线程之间是存在竞争的，各个线程交替使用 CPU 时间，将 errno 设置为全局变量会导致一个严重的问题：线程 A 中的函数修改了 errno 的值，还没来得及读取就挂起了，线程 B 获得 CPU 时间后又修改了 errno 的值，等到线程 A “苏醒” 后再读取 errno 的值时，已经找不到原来的值了，只能读取线程 B 设置的值，而线程 A 对此一无所知。

要解决这个问题，就不能定义全局范围内的 errno，而要针对每个线程定义自己的 errno，所以很多支持多线程的库都将 errno 实现为一个函数。如下所示：

```c
#if define _MULTI_THREAD
#define errno (*_errno_func())
#endif
```

_MULTI_THREAD 是开启多线程后定义的宏，通过 _errno_func () 函数可以得到线程内部 errno 变量的地址，在前面加 `*` 就能够得到 errno 变量本身。

`这段代码重在演示原理，使用的宏名和函数名都是假定的，真实的库实现并不使用这些名字。`



# 与内存相关函数

## memcpy函数



```c
#include<string.h>
void *memcpy(void * dest, const void * src, size_t num );
//复制 src 所指的内存内容的前 num 个字节到 dest 所指的内存地址上。
```

参数

- **dest** -- 指向用于存储复制内容的目标数组，类型强制转换为 void* 指针。
- **src** -- 指向要复制的数据源，类型强制转换为 void* 指针。
- **num** -- 要被复制的字节数。

 返回值：该函数返回一个指向目标存储区 dest 的指针。

需要注意的是：

- dest 指针要分配足够的空间，也即大于等于 num 字节的空间。如果没有分配空间，会出现断错误。
- dest 和 src 所指的内存空间不能重叠（如果发生了重叠，使用 [memmove()](http://c.biancheng.net/cpp/html/156.html) 会更加安全）。


与 [strcpy()](http://c.biancheng.net/cpp/html/2540.html) 不同的是，memcpy () 会完整的复制 num 个字节，不会因为遇到 “\0” 而结束。

【返回值】返回指向 dest 的指针。注意返回的指针类型是 void，使用时一般要进行强制类型转换。

> 实例1
>
> ```c
> // 将字符串复制到数组 dest 中
> #include <stdio.h>
> #include <string.h>
> 
> int main ()
> {
> const char src[50] = "http://www.runoob.com";
> char dest[50];
> 
> memcpy(dest, src, strlen(src)+1);
> printf("dest = %s\n", dest);
> 
> return(0);
> }
> 
> //让我们编译并运行上面的程序，这将产生以下结果：
> dest = http://www.runoob.com
> ```
>
> 实例2
>
> ```c
> #include <stdio.h>
> #include<string.h>
> 
> int main()
> 
> {
> char *s="http://www.runoob.com";
> char d[20];
> memcpy(d, s+11, 6);// 从第 11 个字符 (r) 开始复制，连续复制 6 个字符 (runoob)
> // 或者 memcpy (d, s+11*sizeof (char), 6*sizeof (char));
> d[6]='\0';
> printf("%s", d);
> return 0;
> }
> //让我们编译并运行上面的程序，这将产生以下结果：
> runoob
> ```
>
> 实例3
>
> ```c
> #include<stdio.h>
> #include<string.h>
> 
> int main(void)
> {
> char src[] = "***";
> char ddest[] = "abcdefg";
> printf(" 使用 memcpy 前: % s\n", dest);
> memcpy(dest, src, strlen(src));
> printf(" 使用 memcpy 后: % s\n", dest);
> return 0;
> }
> //让我们编译并运行上面的程序，这将产生以下结果：
> 使用 memcpy 前: abcdefg
> 使用 memcpy 后: ***defg
> ```
>
> 

## bzero函数

```c
//bzero () 会将内存块（字符串）的前 n 个字节清零，其原型为：
void bzero(void *s, int n);
```

【参数】s 为内存（字符串）指针，n 为需要清零的字节数。

  bzero () 会将参数 s 所指的内存区域前 n 个字节，全部设为零值。

  实际上，bzero (void *s, int n) 等价于 memset ((void*) s, 0,size_tn)，用来将内存块的前 n 个字节清零，但是 s 参数为指针，又很奇怪的位于 string.h 文件  中，也可以用来清零字符串。

   注意：bzero () 不是标准函数，没有在 ANSI 中定义，笔者在 VC6.0 和 MinGW5 下编译没通过；据称 Linux 下的 GCC 支持，不过笔者没有亲测。鉴于此，还是使用 memset () 替代吧。



# 线程相关函数

##   signal 函数

```c
void (*signal(int sig, void (*func)(int)))(int);
```

函数说明：signal () 会依参数 signum 指定的信号编号来设置该信号的处理函数。当指定的信号到达时就会跳转到参数 handler 指定的函数执行。如果参数 handler 不是函数指针，则必须是下列两个常数之一：

1、SIG_IGN 忽略参数 signum 指定的信号.
2、SIG_DFL 将参数 signum 指定的信号重设为核心预设的信号处理方式.

返回值：该函数返回信号处理程序之前的值，当发生错误时返回 SIG_ERR。

+ 参数说明：

  + **sig** -- 在信号处理程序中作为变量使用的信号码，它可以取除 SIGKILL 和 SIGSTOP 之外的任意信号。下面是一些重要的标准信号常量：

    |   宏    | 信号                                                         |
    | :-----: | :----------------------------------------------------------- |
    | SIGABRT | (Signal Abort) 程序异常终止。                                |
    | SIGFPE  | (Signal Floating-Point Exception) 算术运算出错，如除数为 0 或溢出（不一定是浮点运算）。 |
    | SIGILL  | (Signal Illegal Instruction) 非法函数映象，如非法指令，通常是由于代码中的某个变体或者尝试执行数据导致的。 |
    | SIGINT  | (Signal Interrupt) 中断信号，如 ctrl-C，通常由用户生成。     |
    | SIGSEGV | (Signal Segmentation Violation) 非法访问存储器，如访问不存在的内存单元。 |
    | SIGTERM | (Signal Terminate) 发送给本程序的终止请求信号。              |

  + **func** -- 一个指向函数的指针。第二个参数则是一个函数指针，该函数无返回值，且包含一个 int 型的参数，表明了当产生信号时，函数指针指向的函数将会被调用。它可以是一个由程序定义的函数，也可以是下面预定义函数之一：

    | SIG_DFL | 默认的信号处理程序。 |
    | ------- | -------------------- |
    | SIG_IGN | 忽视信号。           |



>  附加说明：
>
>  >  在信号发生跳转到自定的 handler 处理函数执行后，系统会自动将此处理函数换回原来系统预设的处理方式，如果要改变此操作请改用 sigaction ().



> 实例
>
> ```c
> #include <stdio.h>
> #include <unistd.h>
> #include <stdlib.h>
> #include <signal.h>
> 
> void sighandler(int);
> 
> int main()
> {
>     signal(SIGINT, sighandler);
> 
> while(1)
> {
>        printf("开始休眠一秒钟...\n");
>        sleep(1);
> }
>     return(0);
> }
> 
> void sighandler(int signum)
> {
>     printf("捕获信号 %d，跳出...\n", signum);
>     exit(1);
> }
> ```
> 
>让我们编译并运行上面的程序，这将产生以下结果，且程序会进入无限循环，需使用 CTRL + C 键跳出程序。
> 
>```bash
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 捕获信号 2，跳出...
> ```
> 

##  sigaction函数

 signal 函数的使用方法简单，但并不属于 POSIX 标准，在各类 UNIX 平台上的实现不尽相同，因此其用途受到了一定的限制。而 POSIX 标准定义的信号处理接口是 sigaction 函数，其接口头文件及原型如下：

```c
// sigaction 函数的功能是检查或修改与指定信号相关联的处理动作（可同时两种操作）
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```

+ signum：要操作的信号，signum 参数指出要捕获的信号类型，可以为除 SIGKILL 及 SIGSTOP 外的任何一个特定有效的信号（为这两个信号定义自己的处理函数，将导致信号安装错误），有关信号可以查看[信号1](#信号1)、[信号2](#信号2)，SIGNUM有以下：

  ```bash
  # junjie @ Ubuntu in ~/公共的/c文件/计算机网络 [日期: 周二 3月 16日, 时间:14:23:28]
  $ kill -l
   1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
   6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
  11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
  16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
  21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
  26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
  31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
  38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
  43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
  48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
  53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
  58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
  63) SIGRTMAX-1  64) SIGRTMAX
  ```

  列表中，编号为 1~31 的信号为传统 UNIX 支持的信号，是不可靠信号（非实时的），编号为 32~63 的信号是后来扩充的，称做可靠信号（实时信号）。不可靠信号和可靠信号的区别在于前者不支持排队，可能会造成信号丢失，而后者不会。

  + 1）SIGHUP：本信号在用户终端连接（正常或非正常）结束时发出，通常是在终端的控制进程结束的，通知同一 session 内的各个作用，这是它们与控制终端不再关联。

    登录 Linux 时，系统会分配给登录用户一个终端（Session）。在这个终端运行的所有程序，包括前台进程组和后台进程组，一般都属于这个 Session。当用户退出 Linux 登录时，前台进程和后台有对终端输出的进程将会收到 SIGHUP 信号，这个信号的默认操作为终止进程，因此前台进程组和后台有终端输出的进程就会终止。不过可以捕获这个信号，比如 wget 能捕获 SIGHUP 信号，并忽略它，这样就算退出了 Linux 登录，wget 也能继续下载。

    此外，对于与终端脱离关系的守护进程，这个信号用于通知它重新读取配置文件。

  + 2） SIGINT：程序终止 (interrupt) 信号，在用户键入 INTR 字符 (通常是 Ctrl-C) 时发出，用于通知前台进程组终止进程。

  + 3）SIGQUIT：和 SIGINT 类似，但由 QUIT 字符 (通常是 Ctrl-/) 来控制。进程在因收到 SIGQUIT 退出时会产生 core 文件，在这个意义上类似于一个程序错误信号。

  + 4） SIGILL：执行了非法指令。通常是因为可执行文件本身出现错误，或者试图执行数据段。堆栈溢出时也有可能产生这个信号。
  + 5) SIGTRAP：由断点指令或其它 trap 指令产生。由 debugger 使用。
  + 6) SIGABRT：调用 abort 函数生成的信号。

  + 7) SIGBUS：非法地址，包括内存地址对齐 (alignment) 出错。比如访问一个四个字长的整数，但其地址不是 4 的倍数。它与 SIGSEGV 的区别在于后者是由于对合法存储地址的非法访问触发的 (如访问不属于自己存储空间或只读存储空间)。

  + 8) SIGFPE：在发生致命的算术运算错误时发出。不仅包括浮点运算错误，还包括溢出及除数为 0 等其它所有的算术的错误。

  +   9) SIGKILL：用来立即结束程序的运行。本信号不能被阻塞、处理和忽略。如果管理员发现某个进程终止不了，可尝试发送这个信号。

  + 10) SIGUSR1：留给用户使用

  + 11) SIGSEGV：试图访问未分配给自己的内存，或试图往没有写权限的内存地址写数据.

  + 12) SIGUSR2：留给用户使用

  + 13) SIGPIPE：管道破裂。这个信号通常在进程间通信产生，比如采用 FIFO (管道) 通信的两个进程，读管道没打开或者意外终止就往管道写，写进程会收到 SIGPIPE 信号。此外用 Socket 通信的两个进程，写进程在写 Socket 的时候，读进程已经终止。

  + 14) SIGALRM：时钟定时信号，计算的是实际的时间或时钟时间. alarm 函数使用该信号.

  + 15) SIGTERM：程序结束 (terminate) 信号，与 SIGKILL 不同的是该信号可以被阻塞和处理。通常用来要求程序自己正常退出，shell 命令 kill 缺省产生这个信号。如果进程终止不了，我们才会尝试 SIGKILL。

  + 17) SIGCHLD：子进程结束时，父进程会收到这个信号。

    如果父进程没有处理这个信号，也没有等待 (wait) 子进程，子进程虽然终止，但是还会在内核进程表中占有表项，这时的子进程称为僵尸进程。这种情况我们应该避免 (父进程或者忽略 SIGCHILD 信号，或者捕捉它，或者 wait 它派生的子进程，或者父进程先终止，这时子进程的终止自动由 init 进程来接管)。

  + 18) SIGCONT：让一个停止 (stopped) 的进程继续执行。本信号不能被阻塞。可以用一个 handler 来让程序在由 stopped 状态变为继续执行时完成特定的工作。例如，重新显示提示符

  + 19) SIGSTOP：停止 (stopped) 进程的执行。注意它和 terminate 以及 interrupt 的区别：该进程还未结束，只是暂停执行。本信号不能被阻塞，处理或忽略.

  + 20) SIGTSTP：停止进程的运行，但该信号可以被处理和忽略。用户键入 SUSP 字符时 (通常是 Ctrl-Z) 发出这个信号

  + 21) SIGTTIN：当后台作业要从用户终端读数据时，该作业中的所有进程会收到 SIGTTIN 信号。缺省时这些进程会停止执行.

  + 22) SIGTTOU：类似于 SIGTTIN, 但在写终端 (或修改终端模式) 时收到.

  + 23) SIGURG：有 "紧急" 数据或 out-of-band 数据到达 socket 时产生.

  + 24) SIGXCPU：超过 CPU 时间资源限制。这个限制可以由 getrlimit/setrlimit 来读取 / 改变。

  + 25) SIGXFSZ：当进程企图扩大文件以至于超过文件大小资源限制。

  + 26) SIGVTALRM：虚拟时钟信号。类似于 SIGALRM, 但是计算的是该进程占用的 CPU 时间.

  + 27) SIGPROF：类似于 SIGALRM/SIGVTALRM, 但包括该进程用的 CPU 时间以及系统调用的时间.

  + 28) SIGWINCH：窗口大小改变时发出.

  + 29) SIGIO：文件描述符准备就绪，可以开始进行输入 / 输出操作.

  + 30) SIGPWR：Powerfailure

  + 31) SIGSYS：非法的系统调用。

    在以上列出的信号中，程序不可捕获、阻塞或忽略的信号有：SIGKILL,SIGSTOP
    不能恢复至默认动作的信号有：SIGILL,SIGTRAP
    默认会导致进程流产的信号有：SIGABRT, SIGBUS, SIGFPE, SIGILL, SIGIOT, SIGQUIT, SIGSEGV, SIGTRAP, SIGXCPU, SIGXFSZ
    默认会导致进程退出的信号有：SIGALRM, SIGHUP, SIGINT, SIGKILL, SIGPIPE, SIGPOLL, SIGPROF, SIGSYS, SIGTERM, SIGUSR1, SIGUSR2, SIGVTALRM
    默认会导致进程停止的信号有：SIGSTOP,SIGTSTP,SIGTTIN,SIGTTOU
    默认进程忽略的信号有：SIGCHLD,SIGPWR,SIGURG,SIGWINCH

+ act：要设置的对信号的新处理方式。

+ oldact：原来对信号的处理方式。

+ 返回值：0 表示成功，-1 表示有错误发生。

  错误代码：
  
  + EINVAL 参数 signum 不合法，或是企图拦截 SIGKILL/SIGSTOPSIGKILL 信号。
  + EFAULT 参数 act, oldact 指针地址无法存取。
  + EINTR 此调用被中断。

```c
struct sigaction {
    void (*sa_handler)(int);
    void (*sa_sigaction)(int, siginfo_t *, void *);
    sigset_t sa_mask;
    int sa_flags;
    void (*sa_restorer)(void);
}
```

+ **成员 sa_handler 是一个函数指针**，其含义与 signal 函数中的信号处理函数类似；
+ **成员 sa_sigaction 则是另一个信号处理函数**，它有三个参数，可以获得关于信号的更详细的信息。当 sa_flags 成员的值包含了 SA_SIGINFO 标志时，系统将使用 sa_sigaction 函数作为信号处理函数，否则使用 sa_handler 作为信号处理函数。**在某些系统中，成员 sa_handler 与 sa_sigaction 被放在联合体中，因此使用时不要同时设置。**
+ **成员 sa_mask 用来指定在信号处理函数执行期间需要被屏蔽的信号**，特别是当某个信号被处理时，它自身会被自动放入进程的信号掩码，因此在信号处理函数执行期间这个信号不会再度发生；
+ **成员 sa_flags 用于指定信号处理的行为**，它可以是一下值的 “按位或” 组合：
  + SA_RESTART：使被信号打断的系统调用自动重新发起。
  +  SA_NOCLDSTOP：使父进程在它的子进程暂停或继续运行时不会收到 SIGCHLD 信号。
  + SA_NOCLDWAIT：使父进程在它的子进程退出时不会收到 SIGCHLD 信号，这时子进程如果退出也不会成为僵尸进程。
  + SA_NODEFER：使对信号的屏蔽无效，即在信号处理函数执行期间仍能发出这个信号，一般情况下， 当信号处理函数运行时，内核将阻塞该给定信号。但是如果设置了 SA_NODEFER 标记， 那么在该信号处理函数运行时，内核将不会阻塞该信号；
  + SA_ONESHOT/SA_RESETHAND：信号处理之后重新设置为默认的处理方式。
  + SA_SIGINFO：使用 sa_sigaction 成员而不是 sa_handler 作为信号处理函数。
+ **成员 re_restorer 则是一个已经废弃的数据域**，不要使用。

与sigaction函数相关的函数还有：

```c
#include <signal.h>
sigemptyset (sigset_t *set)
//初始化由 set 指定的信号集，信号集里面的所有信号被清空；错误代码：EFAULT 参数 set 指针地址无法存取。

sigfillset (sigset_t *set)
//调用该函数后，set 指向的信号集中将包含 linux 支持的 64 种信号；

sigaddset (sigset_t *set, int signum)
//在 set 指向的信号集中加入 signum 信号；
//错误代码：
//1、EFAULT 参数 set 指针地址无法存取。
//2、EINVAL 参数 signum 非合法的信号编号。


sigdelset (sigset_t *set, int signum)
//在 set 指向的信号集中删除 signum 信号；

sigismember (const sigset_t *set, int signum)
//判定信号 signum 是否在 set 指向的信号集中。

// 以上函数执行成功则返回 0, 如果有错误则返回 - 1.
```



> [实例1：](https://blog.csdn.net/weibo1230123/article/details/81411827)

```c
//实例1

#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

int main()
{
    struct sigaction newact,oldact;

    /* 设置信号忽略 */
    newact.sa_handler = SIG_IGN; //这个地方也可以是函数
    sigemptyset(&newact.sa_mask);
    newact.sa_flags = 0;
    int count = 0;
    pid_t pid = 0;

    sigaction(SIGINT,&newact,&oldact);//原来的备份到oldact里面

    pid = fork();
    if(pid == 0)
    {
        while(1)
        {
            printf("I'm child gaga.......\n");
            sleep(1);
        }
        return 0;
    }

    while(1)
    {
        if(count++ > 3)
        {
            sigaction(SIGINT,&oldact,NULL);  //备份回来
            printf("pid = %d\n",pid);
            kill(pid,SIGKILL); //父进程发信号，来杀死子进程
        }

        printf("I am father .......... hahaha\n");
        sleep(1);
    }

    return 0;
}
```



> [实例2](https://blog.csdn.net/weibo1230123/article/details/81411827)

```c
// 实例2
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>
void show_handler(int sig)
{
    printf("I got signal %d\n", sig);
    int i;
    for(i = 0; i < 5; i++)
   {
        printf("i = %d\n", i);
        sleep(1);
    }
}

int main(void)
{
    int i = 0;
    struct sigaction act, oldact;
    act.sa_handler = show_handler;
    sigaddset(&act.sa_mask, SIGQUIT);           //见注(1)
    act.sa_flags = SA_RESETHAND | SA_NODEFER;   //见注(2)
    //act.sa_flags = 0;                         //见注(3)

    sigaction(SIGINT, &act, &oldact);
    while(1)
   {
        sleep(1);
        printf("sleeping %d\n", i);
        i++;
    }
}
```

注：
(1) 如果在信号 SIGINT (Ctrl + c) 的信号处理函数 show_handler 执行过程中，本进程收到信号 SIGQUIT (Crt+\\)，将阻塞该信号，直到 show_handler 执行结束才会处理信号 SIGQUIT。

(2) SA_NODEFER 一般情况下， 当信号处理函数运行时，内核将阻塞 < 该给定信号 -- SIGINT>。但是如果设置了 SA_NODEFER 标记， 那么在该信号处理函数运行时，内核将不会阻塞该信号。 SA_NODEFER 是这个标记的正式的 POSIX 名字 (还有一个名字 SA_NOMASK，为了软件的可移植性，一般不用这个名字) 。

 SA_RESETHAND 当调用信号处理函数时，将信号的处理函数重置为缺省值。 SA_RESETHAND 是这个标记的正式的 POSIX 名字 (还有一个名字 SA_ONESHOT，为了软件的可移植性，一般不用这个名字)

(3) 如果不需要重置该给定信号的处理函数为缺省值；并且不需要阻塞该给定信号 (无须设置 sa_flags 标志)，那么必须将 sa_flags 清零，否则运行将会产生段错误。但是 sa_flags 清零后可能会造成信号丢失！



输出如下：

```bash
# junjie @ Ubuntu in ~/公共的/c文件/systemlib [日期: 周二 3月 16日, 时间:09:50:59]
$ ./test
sleeping 0
sleeping 1
sleeping 2
sleeping 3
sleeping 4
sleeping 5
sleeping 6
sleeping 7
sleeping 8
sleeping 9
sleeping 10
sleeping 11
sleeping 12
按下<ctrl + c> got signal 2
i = 0
i = 1
i = 2
i = 3
i = 4
sleeping 13
sleeping 14
sleeping 15
sleeping 16
sleeping 17
sleeping 18
sleeping 19
sleeping 20
sleeping 21
sleeping 22
按下<ctrl + c>
```





> [实例3](https://blog.csdn.net/weibo1230123/article/details/81411827)

```c
//实例3

#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <errno.h>

static void sig_usr(int signum)
{
    if(signum == SIGUSR1)
    {
        printf("SIGUSR1 received\n");
    }
    else if(signum == SIGUSR2)
    {
        printf("SIGUSR2 received\n");
    }
    else
    {
        printf("signal %d received\n", signum);
    }
}

int main(void)
{
    char buf[512];
    int  n;
    struct sigaction sa_usr;
    sa_usr.sa_flags = 0;
    sa_usr.sa_handler = sig_usr;   //信号处理函数

    sigaction(SIGUSR1, &sa_usr, NULL);
    sigaction(SIGUSR2, &sa_usr, NULL);

    printf("My PID is %d\n", getpid());

    while(1)
    {
        if((n = read(STDIN_FILENO, buf, 511)) == -1)
        {
            if(errno == EINTR)
            {
                printf("read is interrupted by signal\n");
            }
        }
        else
        {
            buf[n] = '\0';
            printf("%d bytes read: %s\n", n, buf);
        }
    }

    return 0;
}
```

 在这个例程中使用 sigaction 函数为 SIGUSR1 和 SIGUSR2 信号注册了处理函数，然后从标准输入读入字符。程序运行后首先输出自己的 PID，如：

```bash
My PID is 5904
```


 这时如果从另外一个终端向进程发送 SIGUSR1 或 SIGUSR2 信号，用类似如下的命令：kill -USR1 5904

 则程序将继续输出如下内容：

```bash
 SIGUSR1 received
 read is interrupted by signal
```



 这说明用 sigaction 注册信号处理函数时，不会自动重新发起被信号打断的系统调用。如果需要自动重新发起，则要设置 SA_RESTART 标志，比如在上述例程中可以进行类似一下的设置：sa_usr.sa_flags = SA_RESTART。



> [实例4](http://c.biancheng.net/cpp/html/1142.html)

```c
// 范例4
#include<stdio.h>
#include<stdlib.h>
#include <unistd.h>
#include <signal.h>
void show_handler(struct sigaction * act)
{
    switch(act->sa_flags)
    {
        case SIG_DFL:
        printf("Default action\n");
        break;
        case SIG_IGN:
        printf("Ignore the signal\n");
        break;
        default:
        printf("0x%x\n", act->sa_handler);
    }
}

main()
{
    int i;
    struct sigaction act, oldact;
    act.sa_handler = show_handler;
    act.sa_flags = SA_ONESHOT|SA_NOMASK;
    sigaction(SIGUSR1, &act, &oldact);
    for(i = 5; i < 15; i++)
    {
        printf("sa_handler of signal %2d =", i);
        sigaction(i, NULL, &oldact);
    }
}
```

执行：

```bash
sa_handler of signal 5 = Default action
sa_handler of signal 6 = Default action
sa_handler of signal 7 = Default action
sa_handler of signal 8 = Default action
sa_handler of signal 9 = Default action
sa_handler of signal 10 = 0x8048400
sa_handler of signal 11 = Default
action sa_handler of signal 12 = Default action
sa_handler of signal 13 = Default action
sa_handler of signal 14 = Default action
```



> [实例5](https://ixyzero.com/blog/archives/3431.html)

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <errno.h>
static void sig_usr(int signum)
{
    if(signum == SIGUSR1)
    {
        printf("SIGUSR1 received\n");
    }
    else if(signum == SIGUSR2)
    {
        printf("SIGUSR2 received\n");
    }
    else
    {
        printf("signal %d received\n", signum);
    }
}
int main(void)
{
    char buf[512];
    int  n;
/*
    sigset_t mask;
    sigfillset(&mask); //将参数 mask 信号集初始化，然后把所有的信号加入到此信号集里，在这里表示屏蔽所有信号
    sigdelset(&mask, SIGUSR1); //删除set中的SIGUSR1信号，即——不屏蔽SIGUSR1信号
    sigdelset(&mask, SIGUSR2); //不屏蔽SIGUSR2信号
    sigprocmask(SIG_SETMASK, &mask, NULL); //参数SIG_SETMASK指定屏蔽mask中包含的信号集
*/
    struct sigaction sa_usr;
    sa_usr.sa_flags = 0;
    //sa_usr.sa_flags = SA_RESTART;
    sa_usr.sa_handler = sig_usr;   //信号处理函数
    sigaction(SIGUSR1, &sa_usr, NULL);
    sigaction(SIGUSR2, &sa_usr, NULL);
    printf("My PID is %d\n", getpid());
    while(1)
    {
        if((n = read(STDIN_FILENO, buf, 511)) == -1)
        {
            if(errno == EINTR)
            {
                printf("read is interrupted by signal\n");
            }
        }
        else
        {
            buf[n] = '\0';
            printf("%d bytes read: %s\n", n, buf);
        }
    }
    return 0;
}
```

在这个例子中使用 sigaction 函数为 SIGUSR1 和 SIGUSR2 信号注册了处理函数，然后从标准输入读入字符。程序运行后首先输出自己的 PID，如：

```bash
My PID is 5904
```

这时如果从另外一个终端向进程发送 SIGUSR1 或 SIGUSR2 信号，用类似如下的命令：
kill -USR1 5904

则程序将继续输出如下内容：

```bash
SIGUSR1 received
read is interrupted by signal
```

这说明用 sigaction 注册信号处理函数时，不会自动重新发起被信号打断的系统调用。如果需要自动重新发起，则要设置 SA_RESTART 标志，比如在上述代码中可以进行类似一下的设置：

```c
sa_usr.sa_flags = SA_RESTART;
```

此外，这里的errno非常让人纠结，她到底是read函数的错误代码还是sigaction的错误代码？？

可以在 /usr/include/asm/errno.h 中找到errno的定义。

# 进程相关函数

