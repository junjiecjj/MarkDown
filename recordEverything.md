[TOC]



#  Linux系统



##  linux系统装机配置

###  搜狗输入法安装

1. 去[搜狗输入法官网](https://pinyin.sogou.com/linux/)下载.deb安装包；

2. 去Ubuntu软件商店搜索fcitx，并安装所有搜索结果；
3. sudo dpkg -i sougou-xxx.deb
4. 去字体设置中心，打开设置区域和语言将“键盘输入法系统”从ibus切换为fcitx；



###  安装字体

> 1. 安装文泉驿微米黑字体

   ```bash
   sudo apt-get install fonts-wqy-microhei
   sudo apt-get install fonts-wqy-zenhei
   ```



> 2. 安装微软字体、宋体等

   第一种方法：

   ```bash
   sudo apt-get update
   sudo apt-get install ttf-mscorefonts-installer
   sudo fc-cache -f -v # 更新字体缓存
   ```

   第二种：

   +  sudo mkdir /usr/share/fonts/truetype/windows-fonts
   + 拷贝字体到 windows-fonts 目录下
   + 修改权限，并更新字体缓存
   +  cd /usr/share/fonts/truetype/windows-font
   +  sudo mkfontscale
   +  sudo mkfontdir
   +  sudo fc-cache -fv
   +  重启

> 3. [安装 Nerd Fonts 字体](https://shuhm-gh.github.io/2017/03/23/linux-admin-%E5%AE%89%E8%A3%85nerd-fonts%E5%AD%97%E4%BD%93/)

   ```bash
   # 去https://www.nerdfonts.com/font-downloads下载字体，存放在~/下载/nerdfonts/下
   # Sauce Code Pro Nerd Font; FiraCode Nerd Font; JetBrainsMono Nerd Font
   $: mkdir -p /usr/share/fonts/truetypes/nerdfonts
   $: cd /usr/share/fonts/truetypes/nerdfonts
   $: cp 下载/nerdfonts/*  .
   #:生成字体信息缓存
   $:fc-cache -vf  
   #:查看是否安装成功
   fc-list | grep -i nerd
   ```

> 4. 安装Fira Code 字体

   + 创建 sh 脚本文件

     ```
     vim installfiracode_font.sh
     ```

   + 在文件中写入以下内容,若需要指定字体安装的目录，更改 file_path 的值即可

     ```bash
     #!/usr/bin/env bash

     fonts_dir="${HOME}/.local/share/fonts"
     if [ ! -d "${fonts_dir}" ]; then
         echo "mkdir -p $fonts_dir"
         mkdir -p "${fonts_dir}"
     else
         echo "Found fonts dir $fonts_dir"
     fi

     for type in Bold Light Medium Regular Retina; do
         file_path="${HOME}/.local/share/fonts/FiraCode-${type}.ttf"
         file_url="https://github.com/tonsky/FiraCode/blob/master/distr/ttf/FiraCode-${type}.ttf?raw=true"
         if [ ! -e "${file_path}" ]; then
             echo "wget -O $file_path $file_url"
             wget -O "${file_path}" "${file_url}"
         else
     	echo "Found existing file $file_path"
         fi;
     done

     echo "fc-cache -f"
     fc-cache -f

     ```

   + 运行脚本进行安装

     chmod +x installfiracode_font.sh

     需要注意的是，该脚本会将字体下载到: `~/.local/share/fonts/` 下，如果要设置为系统字体，将该目录中的所有字体复制到系统字体目录中，即

     ````bash
     sudo mkdir /usr/share/fonts/truetype/firacode
     sudo cp ~/.local/share/fonts/FiraCode-*.ttf   /usr/share/fonts/firacode/
     ````

   + 查看本地字体:fc-list,看到这一行就说明已经安装成功了

     ```bash
     ...
     /root/.local/share/fonts/FiraCode-Light.ttf: Fira Code,Fira Code Light:style=Light,Regular
     ...
     ```

   + 完成；



> 5 JetBrains Mono

网址：https://www.jetbrains.com/lp/mono/,下载

```bash
sudo mkdir /usr/share/fonts/truetype/jetbrains
sudo cp ~/下载/JetBrainsMono-2.225/fonts/ttf/*.ttf  /usr/share/fonts/truetype/jetbrains/
cd  /usr/share/fonts/truetype/jetbrains/
sudo fc-cache -f -v
```





###  修改终端提示符

+ 主要是将.bashrc中的PS1改为[github.bashrc](https://github.com/junjiecjj/configure_file/blob/master/bashrc)中此PS1的形式；

###  安装谷歌浏览器

+ 去[谷歌官网](https://www.google.cn/chrome/)下载.deb安装包
+ sudo dpkg -i google-chrome-xxx.deb
+ 打开谷歌浏览器，安装谷歌上网助手；
+ 登陆谷歌账号，自动同步；
+ 完成;

### Notion

Notion目前并没有提供Linux系统上的应用。但是可以使用网页版。

> **步骤**

+ 首先在桌面新建一个notion.sh

```bash
#桌面右键，打开终端，输入
touch notion.sh
```

+ 编辑notion.sh内容

```bash
#在终端中，用文本编辑器打开notion.sh 举例gedit
sudo gedit notion.sh
```

+ 在文本编辑器中输入（此命令为用谷歌浏览器打开，可替换成其他浏览器）

```bash
#!/bin/bash
google-chrome --app=https://www.notion.so
```

+ 保存并关闭，随后再次打开终端，给文件加权限

```bash
chmod u+x　notion.sh
```

+ 双击notion.sh即可打开（记得登陆），或者./notion.sh。

> 将notion作为一个系统命令
>
> ```bash
> sudo cp  ~/notion.sh /usr/local/bin/notion
> sudo chmod 755 notion
> ```
>
> 即可通过终端中输入:`notion`打开。





### wolai

去[wolai官网](https://www.wolai.com/)下载wolai-1.0.34.AppImage

> 将wolai作为一个系统命令
>
> ```bash
> sudo cp  ~/wolai-1.0.34.AppImage /usr/local/bin/wolai
> sudo chmod 755 wolai
> ```
>
> 即可通过终端中输入:`wolai`打开。



###  Markdown编辑器Typora

1.方法一：
  + [**官网下载**]()

  + 解压:tar -zxvf Typora-linux-x64.tar.gz

  + 拷贝到常用的软件安装目录下:sudo cp -ar Typora-linux-x64 /opt

  + 拷贝桌面快捷方式到桌面:

  ```bash
  cd /opt/Typora-linux-x64
  cp Typora.desktoop ~/Desktop/
  ```
2.方法二：

   ```bash
   # or run:
   # sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
   wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
   # add Typora's repository
   sudo add-apt-repository 'deb https://typora.io/linux ./'
   sudo apt-get update
   # install typora
   sudo apt-get install typora
   ```



###  Markdown编辑器remarkable

+ 去[remarkable](http://remarkableapp.github.io/linux/download.html)下载.deb文件
+ sudo dpkg -i remarkable-xxx.deb
+ sudo apt-get install -f
+ *remarkable &*

如果报警说少了 gtkspellcheck：

+ sudo apt-get install python3-gtkspellcheck

###  窗口管理器FVWM

+ sudo apt install fvwm
+ 去[网址](https://github.com/junjiecjj/configure_file)下载fvwm至~/.fvwm文件夹即可

###  VS Code

+ 下载[VSCode](https://code.visualstudio.com/Download)的.deb文件
+ sudo dpkg -i xxx.deb


###  VIM打造IDE

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vim
sudo apt-get install git
#安装bundle
git clone https://github.com/VundleVim/Vundle.vim.git ∼/.vim/bundle/Vundle.vim

cd /home/jack/
```

将[github.vimrc](https://github.com/junjiecjj/configure_file/blob/master/vimrc)中的内容加入到~/.vimrc中，打开 vim，进入命令模式，输入:PluginInstall;

PluginInstall 就是 vundle 的包管理器 Plugin 常用命令：

+ PluginInstall: 安装插件
+ PluginCleqn: 移除不要的插件
+  PluginUpdate: 更新插件
+  PluginList: 列出说所有的安装插件
+ PluginSearch: 查找插件

接下来安装以来工具

+ sudo apt-get install build-essential cmake
+ sudo apt-get install python-dev python3-dev
+ sudo apt-get install libxml2-dev libxslt-dev
+ sduo apt-get install clang
+ sudo apt-get install libclang-dev

接下来进入目录：

```bash
cd .vim/bundle
git clone –recursive git://github.com/Valloric/YouCompleteMe
cd /.vim/bundle/YouCompleteMe/
git submodule update –init –recursive # 获取 YCM 的依赖包
```

此时如果检测完整性, 即输入:

```bash
git submodule update - -init –recursive
```

不会出现任何返回, 因为一开始的 git 加了 recursive 参数。接下来:

```bash
sudo ./install.py –clang-completer –system-libclang
```

或者:

```bash
sudo ./install.py
```

+ +GO 支持：安装 Go 并在调用./install.py 时添加–go-completer

+ +TypeScript 支持：安装 Node.js 和 npm，然后使用 npm install -g typescript 安装 TypeScript SDK

+ +JavaScript 支持：安装 Node.js 和 npm，并在调用./install.py时添加–js-completer

+ +Rust 支持：安装 Rust 并在调用./install.py 时添加–rust-completer

+ +Java 支持：安装 JDK8（需要版本 8），并在调用./install.py 时添加–java-completer



### screenkey

+ sudo apt-get install screenkey

或：

+ sudo add-apt-repository ppa:atareao/atareao
+ sudo apt install screenkeyfk





###  WPS

+ 去[wps官网](https://www.wps.cn/product/wpslinux)下载.deb安装包
+ sudo dpkg -i wps_xxx.deb



### [在 Ubuntu 20.04 上安装 Microsoft Edge 浏览器](https://mp.weixin.qq.com/s/E2EBTzQMy4PjVbGEJ1VM3g)

```bash
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main"
sudo apt install microsoft-edge-dev
#至此，您已经在 Ubuntu 系统上安装了 Edge。
#发布新版本时，可以通过桌面标准软件更新工具或在终端中运行以下命令来更新 Edge 软件包：
sudo apt update
sudo apt upgrade
#也可以通过输入命令行从命令行启动
microsoft-edge
```



### ZSH

```bash
#安装 zsh， on my zsh等

#安装zsh
sudo apt install zsh
#安装on my zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
或
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
或
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

#安装incr
cd .oh-my-zsh/plugins/
mkdir incr
cd incr
wget http://mimosa-pudica.net/src/incr-0.2.zsh

#安装zsh-autosuggestions
git clone git://github.com/zsh-users/zsh-autosuggestions  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

#安装zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

#安装 autojump
sudo apt-get install autojump
或以下三个：
git clone https://github.com/wting/autojump   ~/.oh-my-zsh/plugins/autojump
cd ~/.oh-my-zsh/plugins/autojump
./install.py

#安装nvm
git clone https://github.com/lukechilds/zsh-nvm ~/.oh-my-zsh/plugins/zsh-nvm

#安装pyenv
git clone https://github.com/davidparsson/zsh-pyenv-lazy.git  ~/.oh-my-zsh/plugins/pyenv-lazy

#将~/.zshrc文件变成如下：
```

<https://github.com/junjiecjj/configure_file/blob/master/zshrc_xiong-chiamiov-plus>





###  PDF阅读器

+ sudo apt install okular



### [Simple Terminal终端](https://cloud.tencent.com/developer/article/1771532)

<https://avimitin.com/system/simpleterminal.html>

`linux` 下有许多优秀的 `Termainl`，我在用的有`deepin-terminal`,`alacritty`,‘simple terminal’.`alacritty` 是一款使用显卡渲染的终端模拟器，非常的快并且流畅，并且支持终端显示图片，所以比 `deepin-terminal` 更让我喜欢，然而 `simple terminal` 确实一款十分简单的终端模拟器，虽然简单但功能却一个不少，体积更小。甚至连配置文件都没有，每次更改配置都要修改源代码并且编译生成程序，实在是够简单。 但是在 `deepin` 上无法直接安装，需要安装几个依赖的软件。

```bash
sudo apt install libx11-dev  libxft-dev
```

#### install

官方地址 ：https://st.suckless.org/ 。最后有一个 `Download` 但是不建议直接下载，可能版本比较老，上面有个 git 链接，可以直接克隆源仓库。

```bash
git clone https://git.suckless.org/st
```

如果是 deepin 操作系统，还需要更改源代码中的 `config.mk` 文件，更改刚才安装的依赖软件位置

```bash
# 更改如下
X11INC = /usr/X11R6/X11                                               
X11LIB = /usr/include/X11
```

编译安装：`sudo make clean install` 此时通过 st 命令即可打开。

#### 更改配置

前面也说了，st 没有配置文件，所以我们直接进源码目录，找到 config.h 文件，通过注释来更改自己的内容，一般更改字体跟窗口大小即可，后面可以通过打补丁的方式增加更多的功能。

```bash
# config.h 
static char *font = "JetBrains Mono:pixelsize=24:antialias=true:autohint=true"; # 更改字体跟大小
sudo make clean install # 重新编译并安装，使用 st 命令即可打开。
```

#### 添加补丁

在官方地址左侧存在一列 Patch, 即为补丁列表，其中有更改外观的，比如透明度，颜色。也有增加功能的。 [官方补丁地址](https://st.suckless.org/patches/)

1. 下载补丁到本地，可以使用 `wget ` + 链接来直接下载到源码目录下。
2. 安装补丁：

```bash
patch < fillname # 补丁文件
```

1. 设置配置文件，因为 `config.xxx.h` 文件是模板文件，真正的配置文件是 `config.h` ,推荐删除那个模板文件，当补丁执行后询问配置文件时输入 `config.h` 即可。 如果报权限问题使用 `sudo patch <` 来打补丁。

- 成功：如果补丁成功，输出全部是 `success`，直接编译运行即可查看补丁的实际效果。
- 失败：如果失败，也会响应的输出，打开补丁文件可以发现，所有的补丁文件都是一个 `diff` 文件，文件描述了补丁文件与配置文件的差异，+ 符号代表是需要添加的内容，- 代表需要删除的内容，根据文件描述来手动修改 `config.h` 文件即可，出现错误的原因就是没有自动的完成替换，那就手动完成。

#### 推荐补丁

1. st-alpha : 设置终端透明度
2.  st-anysize : 设置终端大小为占满屏幕
3.  st-copyurl : 对于终端输出的 url，使用 alt + l 快捷键来回选择，回车复制。
4. st-dracula : 终端主题
5.  st-blinking_cursor : 终端光标闪烁
6.  st-desktopentry : 添加终端的应用图标及应用 desktop 文件
7.  st-font2 : 字体代替，如果缺少字体，可以设置多个字体来逐级渲染
8.  st-hidecursor : 终端输入时隐藏光标，防止光标遮挡住字符
9.  st-opencliphboard : 结合 copyurl 使用，将复制的链接直接用浏览器打开
10.  st-rightclickpaste : 右键粘贴
11.  st-scrollback : 快捷键滚动屏幕，默认 shift pageon /shift pageup
12. st-scrollback-mouse : 设置鼠标滚动屏幕输出

#### 前言

Sim­ple Ter­mi­nal 是一个基于 X 的终端，拥有非常棒的 Uni­code 和 Emoji 的支持，同时也支持 256 色，拥有绝大部分的终端特性，但是却极其微小，就算在我打了许多补丁之后，他仍然只占用 108K 的存储空间，快且轻量，是重度终端用户的一个很不错的选择。

本篇文章目的在教你打造一个自己的 st，如果没有需求也可以前往我的[仓库](https://github.com/Avimitin/st)克隆我的源码，直接编译安装就可以了。

#### 下载源码

Sim­ple Ter­mi­nal 的官网：https://st.suckless.org/ ，不需要下载 Down­load 的那个 st ，直接 clone 源码仓库就好：

```bash
git clone https://git.suckless.org/st
```

#### 安装依赖

Sim­ple Ter­mi­nal (以下简称 st) 需要 `libx11-dev` 和 `libxft-dev` 两个包，对于 De­bian 和 Ubuntu 用户来说直接使用 apt 安装即可，Arch 系的大部分发行版都已经包含。

克隆官方的仓库之后，编辑 `config.mk` 文件，编译时 st 会基于这个文件进行配置，一般来说只需要改两行即可：

```bash
X11INC = /usr/local/X11
X11LIB = /usr/local/X11
```

然后用 root 权限执行编译安装：

```bash
sudo make clean install
```

文件会复制到 `/usr/local/bin/` 目录下，一般直接执行就能启动。

#### 配置

一般直接安装就能用了，但是没有人会喜欢一个黑黝黝字体那么丑的终端，所以需要进行一些基础配置。

st 没有配置文件，所有的配置都是直接编译进二进制文件里的，在第一次执行完 `make install` 之后 st 目录下应该有个 `config.def.h` 和 `config.h` 文件，直接删除 `config.def.h` ，然后修改 `config.h`：

- 修改字体

在第一行就能看到一行 `...font...` 字样，你可以修改里面的字体名和文字大小

```c
static char *font = "Liberation Mono:pixelsize=12:antialias=true:autohint=true";
```

建议你使用 Jet­brains Mono 或者 Fira Mono 这类有 nerd font 补丁的等宽字体，nerd fonts 可以在 github 下载：https://github.com/ryanoasis/nerd-fonts/releases 。下载完之后解压到 `~/.config/fonts` 文件夹并执行命令 `fc-cache -fv` 刷新字体缓存，然后回到 `config.h` 文件修改字体：

```diff
-static char *font = "Liberation Mono:pixelsize=12:antialias=true:autohint=true";
+static char *font = "JetbrainsMono Nerd Font:pixelsize=24:antialias=true:autohint=true";
```

然后重新执行 `sudo make clean install` 安装。

- 修改主题

你可以在 http://terminal.sexy/ 上点击 Scheme Browser 选择喜欢的主题，然后点击 Ex­port，For­mat 选择 `Simple Terminal` 然后复制即可。

打开 `config.h` 文件，找到 `/* Terminal Color...` 这行，把复制的文字替换即可。比如我这里选择了 To­mor­row Night 主题：

```diff
-       "black",
-       "red3",
-       "green3",
-       "yellow3",
-       "blue2",
-       "magenta3",
-       "cyan3",
-       "gray90",
-
-       /* 8 bright colors */
-       "gray50",
-       "red",
-       "green",
-       "yellow",
-       "#5c5cff",
-       "magenta",
-       "cyan",
-       "white",
-
-       [255] = 0,
-
-       /* more colors can be added after 255 to use with DefaultXX */
-       "#cccccc",
-       "#555555",
+       [0] = "#1d1f21", /* black   */
+       [1] = "#cc6666", /* red     */
+       [2] = "#b5bd68", /* green   */
+       [3] = "#f0c674", /* yellow  */
+       [4] = "#81a2be", /* blue    */
+       [5] = "#b294bb", /* magenta */
+       [6] = "#8abeb7", /* cyan    */
+       [7] = "#c5c8c6", /* white   */
+
+/* 8 bright colors */
+       [8]  = "#969896", /* black   */
+       [9]  = "#cc6666", /* red     */
+       [10] = "#b5bd68", /* green   */
+       [11] = "#f0c674", /* yellow  */
+       [12] = "#81a2be", /* blue    */
+       [13] = "#b294bb", /* magenta */
+       [14] = "#8abeb7", /* cyan    */
+       [15] = "#ffffff", /* white   */
+
+       /* special colors */
+       [256] = "#1d1f21", /* background */
+       [257] = "#c5c8c6", /* foreground */
+
 };
```

然后编译安装。

- 打补丁

你也可以给源码打上 diff 补丁来增加 st 的功能。补丁可以在官网 patches 页面下寻找：https://st.suckless.org/patches/ ，这里我推荐几个比较实用的补丁，别的可以根据自己需求安装。

1. 背景透明

首先是背景透明：https://st.suckless.org/patches/alpha/ ，通过给背景加上 al­pha 通道实现透明，打开页面下载最新的 0.8.2 版本，放到 st 仓库目录下后，执行命令打上补丁：

```bash
patch < st-alpha-0.8.2.diff
```

它会找不到 con­fig.def.h 然后询问你打到哪，你输入 con­fig.h 即可。

你可能想修改透明度，找到下面这行修改值即可，1 即是不透明

```diff
- float alpha = 0.8;
+ float alpha = 0.2;
```

1. 然后是 [anysize](https://st.suckless.org/patches/anysize/)，可以帮助你的 simple terminal 适应各种大小的拉伸。
2. 如果想在桌面启动 st，可以打上 [DesktopEntry](https://st.suckless.org/patches/desktopentry/) 补丁，他会帮你自动生成 .desktop 文件。
3. 如果你的 st 显示 emoji 很奇怪你可能还会需要 [fontfix 补丁](https://github.com/Avimitin/st/blob/master/patches/st-fontfix.diff)
4. 除此之外你还需要滚动终端显示内容，所以需要打上 [scrollback 补丁](https://st.suckless.org/patches/scrollback/)，默认下摁 Alt + PageUp 或 PageDown 翻页。

如果你先修改上翻下翻的键位，可以参考下面：

```diff
-    { ShiftMask,            XK_Page_Up,     kscrollup,      {.i = -1} },
-    { ShiftMask,            XK_Page_Down,   kscrolldown,    {.i = -1} },
+    { MODKEY,               XK_u,           kscrollup,      {.i = 1} },
+    { MODKEY,               XK_j,           kscrolldown,    {.i = 1} },
+    { MODKEY|ControlMask,   XK_u,           kscrollup,      {.i = -1} },
+    { MODKEY|ControlMask,   XK_j,           kscrolldown,    {.i = -1} },
```

这里我把上翻一行改为了 Mod + u （Mod 键即使 alt 键），下翻一行改为了 Mod + j，上翻一页改为 Mod + Ctrl + u。

1. [font2](https://st.suckless.org/patches/font2/)，可以帮助你把 emoji 显示不正确的问题通过增加 fallback 的形式修复，你就不需要改变喜欢的字体了。

> 如果在打补丁的时候遇到打不上错误的问题，他会在本地生成 con­fig.h.rej 文件你把里面带 + 号的复制到 con­fig.h 的指定位置，把带 - 的在 con­fig.h 里找到删除即可。

#### 最后

你也可以免去所有麻烦直接克隆我配置好的文件：

```bash
git clone git@github.com:Avimitin/st.git
cd st
sudo make clean install
```

或者前往 [release](https://github.com/Avimitin/st/releases)（可能有的修改不会及时上传）下载我预编译的 `linux-amd64` 版本。

### 命令行浏览器w3m


![img](https://img.linux.net.cn/data/attachment/album/202011/22/212553gpd6g2vzk20ve5sg.jpg)

`w3m` 是一个流行的基于文本的开源终端 Web 浏览器。尽管其初始项目已经不再活跃，但另一个开发者 Tatsuya Kinoshita 正在维护着它的一个活跃分支。

`w3m` 相当简单，支持 SSL 连接、色彩，也支持内嵌图片。当然，根据你试图访问的资源，你那边的情况可能会有所不同。根据我的简单测试，它似乎无法加载 [DuckDuckGo](https://duckduckgo.com/)，但我可以[在终端中使用 Google](https://itsfoss.com/review-googler-linux/)就够了。

安装后，你可以简单的在终端中输入 `w3m` 以得到帮助。如果你感兴趣的话，也可以到 [GitHub](https://github.com/tats/w3m) 上去查看它的仓库。

**如何安装和使用 w3m？**

`w3m` 在任何基于 Debian 的 Linux 发行版的默认仓库中都是可用的。如果你有一个基于 Arch 的发行版，但没有直接可用的软件包，你可能需要查看一下 [AUR](https://itsfoss.com/aur-arch-linux/)。

对于 Ubuntu，你可以通过键入以下内容来安装它：

```bash
sudo apt install w3m w3m-img
```

在这里，我们将 w3m 包和图片扩展一起安装，以支持内嵌图片。接下来，要开始安装，你只需要按照下面的命令进行操作即可：

```bash
w3m xyz.com
```

当然，你需要将 `xyz.com` 替换成任何你想浏览或测试的网站。最后，你应该知道，你可以使用键盘上的方向键来导航，当你想采取一个动作时，按回车键。

要退出，你可以按 `SHIFT+Q`，返回上一页是 `SHIFT+B`。其他快捷键包括用 `SHIFT+T` 打开新标签页和用 `SHIFT+U` 打开新的 URL。

你可以通过访问它的手册页来了解更多信息。

### 命令行浏览器lynx


![img](https://img.linux.net.cn/data/attachment/album/202011/22/212611h22p4g4db7k7fbpf.jpg)

Lynx 是另一个开源的命令行浏览器，你可以试试。幸运的是，很多的网站在使用 Lynx 时往往能正常工作，所以我说它在这方面肯定更好。我能够加载 DuckDuckGo，并使其工作。

除此之外，我还注意到它可以让你在访问各种 Web 资源时接受或拒绝 cookie。你也可以将它设置为总是接受或拒绝。所以，这是件好事。

另一方面，在终端上使用时，窗口不能很好地调整大小。我还没有寻找到任何解决方法，所以如果你正在尝试这个，你可能会想要这样做。不论如何，它都很好用，当你在终端启动它时，你会得到所有键盘快捷键的说明。

请注意，它与系统终端主题不匹配，所以无论你的终端看起来如何，它都会看起来不同。

**如何安装 Lynx？**

与 w3m 不同的是，如果你有兴趣尝试的话，确实可以找到一些 Win32 上的安装程序。不过，在 Linux 上，它在大多数的默认仓库中都是可用的。

对于 Ubuntu 来说，你只需要输入：

```bash
sudo apt install lynx
```

要想使用，你只需要按照下面的命令进行操作：

```bash
lynx examplewebsite.com
```

在这里，你只需要将示例网站替换成你想要访问的资源即可。

如果你想找其他 Linux 发行版的软件包，可以查看他们的[官网资源](https://lynx.invisible-island.net/lynx-resources.html)。



### 命令行浏览器Links2


![img](https://img.linux.net.cn/data/attachment/album/202011/22/212634u21h4jihxhyzrh4x.jpg)

Links2 是一款有趣的基于文本的浏览器，你可以在你的终端上轻松使用，用户体验良好。它提供了一个很好的界面，你启动它后，只要输入网址就可以了。

![img](https://img.linux.net.cn/data/attachment/album/202011/22/212700lhq9qivfoiqiod8q.jpg)

值得注意的是，主题将取决于你的终端设置，我设置为“黑绿色”主题，因此你看到的就是这个。当你以命令行浏览器的方式启动它后，你只需要按任意键就会出现 URL 提示，或者按 `Q` 键退出。它相当好用，可以渲染大多数网站的文字。

与 Lynx 不同的是，你没有接受或拒绝 cookie 的功能。除此之外，它似乎工作的还不错。

**如何安装 Links2？**

正如你所期望的，你会发现它在大多数默认的仓库中都有。对于 Ubuntu，你可以在终端输入以下命令来安装它：

```bash
sudo apt install links2
```

如果你想在其他 Linux 发行版上安装它，你可以参考它的[官方网站](http://links.twibright.com/download.php)获取软件包或文档。

### 命令行浏览eLinks


![img](https://img.linux.net.cn/data/attachment/album/202011/22/212735pxmbzpvx4980xxqq.jpg)

eLinks 类似于 Links2，但它已经不再维护了。你仍然可以在各种发行版的默认仓库中找到它，因此，我把它保留在这个列表中。

它不会与你的系统终端主题相融合。所以，如果你需要的话，作为一个没有“黑暗”模式的文本型浏览器，这可能不是一个漂亮的体验。

**如何安装 eLinks？**

在 Ubuntu 上，安装它很容易。你只需要在终端中输入以下内容：

```bash
sudo apt install elinks
```

对于其他 Linux 发行版，你应该可以在标准软件仓库中找到它。但是，如果你在软件仓库中找不到它，你可以参考[官方安装说明](http://elinks.or.cz/documentation/installation.html)。



### [7-zip](https://mp.weixin.qq.com/s/xH175-7w-TctfUOQOmPEjA)

用户可以进入这个链接下载：https://www.linuxmi.com/linux-7-zip.html

用户可以从以下链接下载：

- [适用于 64 位 Linux x86-64（AMD64）的 7-Zip](https://7-zip.org/a/7z2101-linux-x64.tar.xz)
- [适用于 64 位 Linux ARM64 的 7-Zip](https://7-zip.org/a/7z2101-linux-arm64.tar.xz)
- [适用于 32 位 Linux x86 的 7-Zip](https://7-zip.org/a/7z2101-linux-x86.tar.xz)
- [适用于 32 位 Linux armhf 的 7-Zip](https://7-zip.org/a/7z2101-linux-arm.tar.xz)

###  射手影音

+ sudo apt install smplayer
+ sudo apt install vlc



###  截图

> 1.深度截图

+ sudo apt install  deepin-screenshot

> 2. 火焰截图

+ 第一种方法：

+ sudo apt-get install flameshot

+ 第二种：

  ```bash
  # 如果您以前安装过，需要先进行卸载
  sudo apt remove flameshot

  # 卸载完成之后需要先克隆项目到本地(本人安装在  /opt  这个目录)
  sudo git clone https://github.com/lupoDharkael/flameshot.git

  # 如果您没有安装 git 需要先安装 git
  sudo apt install git

  #我把项目克隆在 /opt 目录，所以接下来切换到目录 /opt/flameshot/, 在这个目录下作以下操作：
  sudo mkdir build
  cd build
  sudo qmake ../
  sudo make   # 这个步骤是编译，根据每个人的电脑配置不同，需要的时间也不同
  #编译完成之后执行命令
  sudo make install
  #安装完成！
  ```

+ 第三种：
   + 下载[.deb包](https://www.linuxmi.com/flameshot-0.9.html);
   + sudo dpkg -i xxx.deb



###  qq、微信、百度网盘、迅雷

> 1.安装qq

  + 去[腾讯官网](https://im.qq.com/linuxqq/index.html)下载.deb安装包
  + sudo dpkg -i qq-xxx.deb

> 2. 安装微信

   + 去软件中心下载

> 3. 安装百度网盘、迅雷

   ```bash
   $: git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git
   $: cd ./deepin-wine-for-ubuntu
   $: ./install.sh
   #去[阿里镜像](http://packages.deepin.com/deepin/pool/non-free/d/)下载.deb包
   $:  dpkg -i 命令安装
   ```



###  Latex

+ sudo apt-get install texlive-full
+ sudo apt-get install texlive-xetex
+ sudo apt-get install texlive-lang-chinese
+ sudo apt-get install texstudio
+  编译时需要设置编译器为 XeLaTeX，TeXstudio 中在 Options−→TeXstudio−→Build−→Default Compiler 中更改默认编译器为XeLaTeX 即可。在配置中可以更改软件界面语言，将 Options−→Configure
  TeXstudio−→General−→Language 更改为 zh-CN 即可将界面设置为中文。

###  Lyx

1. Lyx 在 ubuntu 软件中心就有, 直接点击下载. 关键是接下来配置 Lyx 显示、导出中文 PDF，以及安装 Imagemagick 图片转换工具‘;

2. 中文界面：Tools→ Perfrences→ 用户界面语言 → 简体中文；

3. sudo apt-get install texlive-full

4. 显示中文 PDF: 文档 → 首选项 → 文档类 →Document calss→chinese Article(CTeX); 页边距 → 上下内外边距设置 为 2cm; 语言 → 简体中文;fromats→ 输出格式 →PDF(XeTeX)；

5. 重配置，重启;

6. 现在导出中文 PDF 没问题，但是 Lyx 里面的 eps 图形没法显示，导出 PDF 可以显示. 安装 ImageMagick, 但是安装 Imagemagick之前需要先先安装 Ghostscript 和 freetype；

7. 安装 Ghostscript:

   + 首先下载源码包:https://github.com/ArtifexSoftware/ghostpdl-downloads/releasesghostscript-9.27.tar.gz 到/usr/src下，然后解压到/usr/src/ghostscript-0.27/；
   + cd /usr/src/ghostscript-0.27/；
   + sudo ./configure – prefix/usr
   + sudo make all
   + sudo make install
   + 完成 ghostscript 安装

8. 安装 freetype

   +  首先下载源码https://www.freetype.org/download.htmlfreetype-2.10.1.tar.xz 到/usr/src/，解压到/usr/src/freetype-2.10.1/；
   +  cd /usr/src/freetype-2.10.1/
   +  ./configure –prefix=/usr/local/freetype
   +  make
   +  sudo make install
   +  完成

9. 最后安装 imageMagick

   +  首先下载源码https://imagemagick.org/script/install-source.phpImageMagick.tar.gz到/home/jack/下载/，解压到/home/jack/下载/ImageMagick-7.0.8-56/
   +  cd /home/jack/下载/ImageMagick-7.0.8-56/
   +  ./configure –with-magick-plus-plus
   +  make
   +  sudo make install
   +  sudo ldconfig /usr/local/lib
   +  /usr/local/bin/convert logo: logo.gif
   +  make check

   如果没有报错，则成功，可以完美的显示 eps 图像了.

### exa

```bash
curl https://sh.rustup.rs -sSf | sh
wget -c https://github.com/ogham/exa/releases/download/v0.8.0/exa-linux-x86_64-0.8.0.zip
unzip exa-linux-x86_64-0.8.0.zip
sudo mv exa-linux-x86_64 /usr/local/bin/exa
```



> 显示选项

- -1, –oneline：每行显示一个条目
- -G, -grid：将条目显示为网格(默认)
- -l –long：显示扩展的详细信息和属性
- -R, -recurse：递归到目录
- -T, –tree：作为树递归到目录中
- -x, -across：对网格进行排序, 而不是向下排序
- –colo [u] r：何时使用终端颜色
- –colo [u] r-scale：清楚地突出显示文件大小级别

> 筛选选项

- -a, -all：显示隐藏和”点”文件
- -d, –list-dirs：列出目录, 例如常规文件
- -L, –level =(depth)：限制递归深度
- -r, -reverse：反转排序顺序
- -s, –sort = {field)：要排序的字段
- –group-directories-first：在其他文件之前列出目录
- -D, –only-dirs：仅列出目录
- –git-ignore：忽略.gitignore中提到的文件
- -I, –ignore-glob = {globs)：要忽略的文件的glob模式(以管道分隔)

两次–all选项也会显示。和..目录。

> 长视选项

当与–long(-l)一起运行时, 这些选项可用：

- -b, -binary：列出带有二进制前缀的文件大小
- -B, -bytes：列出文件大小(以字节为单位), 不带任何前缀
- -g, –group：列出每个文件的组
- -h, –header：向每列添加标题行
- -H, –links：列出每个文件的硬链接数
- -i, -inode：列出每个文件的inode编号
- -m, –modified：使用修改后的时间戳字段
- -S, -blocks：列出每个文件的文件系统块数
- -t, –time = {field)：使用哪个时间戳字段
- -u, –accessed：使用访问的时间戳字段
- -U, –created：使用创建的时间戳字段
- -@, -extended：列出每个文件的扩展属性和大小
- –git：列出每个文件的Git状态, 如果被跟踪或忽略
- –time-style：如何格式化时间戳
- 有效的–color选项始终, 自动和永不。
- 有效的排序字段将被访问, 创建, 扩展, 扩展, inode, 已修改, 名称, 名称, 大小, 类型和无。以大写字母开头的字段将大写字母排在小写字母之前。修改后的字段的别名为日期, 时间和最新, 而其反向字段的别名为age和oldest。
- https://mp.weixin.qq.com/s?__biz=MzAxODI5ODMwOA==&mid=2666551793&idx=3&sn=6ac63a381a8dacb1f5053baafe1b800c&chksm=80dc9d5ab7ab144c44d56612fcde63208a86ed0f1021a3980dab056a68a56ac86abea73b2f65&mpshare=1&scene=1&srcid=0227Jieh2QmUYAMx3hO6QiPb&sharer_sharetime=1616854286536&sharer_shareid=0d5c82ce3c8b7c8f30cc9a686416d4a8#rd有效时间字段被修改, 访问和创建。
- 有效的时间样式是默认, iso, long-iso和full-iso。



### [ranger]()

ranger 是一款独特且非常方便的文件系统导航器，它允许你在 Linux 文件系统中移动，进出子目录，查看文本文件内容，甚至可以在不离开该工具的情况下对文件进行修改。

它运行在终端窗口中，并允许你按下方向键进行导航。它提供了一个多级的文件显示，让你很容易看到你在哪里、在文件系统中移动、并选择特定的文件。

要安装 `ranger`，请使用标准的安装命令（例如，`sudo apt install ranger`）。要启动它，只需键入 `ranger`。它有一个很长的、非常详细的手册页面，但开始使用 `ranger` 非常简单。

#### ranger 的显示方式

你需要马上习惯的最重要的一件事就是 `ranger` 的文件显示方式。一旦你启动了 `ranger`，你会看到四列数据。第一列是你启动 `ranger` 的位置的上一级。例如，如果你从主目录开始，`ranger` 将在第一列中列出所有的主目录。第二列将显示你的主目录（或者你开始的目录）中的目录和文件的第一屏内容。

这里的关键是超越你可能有的任何习惯，将每一行显示的细节看作是相关的。第二列中的所有条目与第一列中的单个条目相关，第四列中的内容与第二列中选定的文件或目录相关。

与一般的命令行视图不同的是，目录将被列在第一位（按字母数字顺序），文件将被列在第二位（也是按字母数字顺序）。从你的主目录开始，显示的内容可能是这样的：

```bash
shs@dragonfly /home/shs/backups     <== current selection
 bugfarm   backups            0  empty
 dory      bin               59
 eel       Buttons           15
 nemo      Desktop            0
 shark     Documents          0
 shs       Downloads          1
   ^         ^                ^      ^
   |         |                |      |
 homes     directories    # files    listing
           in selected    in each    of files in
           home           directory  selected directory
```

`ranger` 显示的最上面一行告诉你在哪里。在这个例子中，当前目录是 `/home/shs/backups`。我们看到高亮显示的是 `empty`，因为这个目录中没有文件。如果我们按下方向键选择 `bin`，我们会看到一个文件列表：

```bash
shs@dragonfly /home/shs/bin      <== current selection
 bugfarm   backups            0    append
 dory      bin               59    calcPower
 eel       Buttons           15    cap
 nemo      Desktop            0    extract
 shark     Documents          0    finddups
 shs       Downloads          1    fix
   ^         ^                ^      ^
   |         |                |      |
 homes     directories    # files    listing
           in selected    in each    of files in
           home           directory  selected directory
```

每一列中高亮显示的条目显示了当前的选择。使用右方向键可移动到更深的目录或查看文件内容。

如果你继续按下方向键移动到列表的文件部分，你会注意到第三列将显示文件大小（而不是文件的数量）。“当前选择”行也会显示当前选择的文件名，而最右边的一列则会尽可能地显示文件内容。

```bash
shs@dragonfly /home/shs/busy_wait.c   <== current selection
 bugfarm   BushyRidge.zip    170 K  /*
 dory      busy_wait.c       338 B   * program that does a busy wait
 eel       camper.jpg       5.55 M   * it's used to show ASLR, and that's it
 nemo      check_lockscreen   80 B   */
 shark     chkrootkit-output 438 B  #include <stdio.h>
   ^         ^                ^       ^
   |         |                |       |
 homes     files            sizes    file content
```

在该显示的底行会显示一些文件和目录的详细信息：

```bash
-rw-rw-r—- shs shs 338B 2019-01-05 14:44    1.52G, 365G free  67/488  11%
```

如果你选择了一个目录并按下回车键，你将进入该目录。然后，在你的显示屏中最左边的一列将是你的主目录的内容列表，第二列将是该目录内容的文件列表。然后你可以检查子目录的内容和文件的内容。

按左方向键可以向上移动一级。

按 `q` 键退出 `ranger`。

#### 做出改变

你可以按 `?` 键，在屏幕底部弹出一条帮助行。它看起来应该是这样的：

```
View [m]an page, [k]ey bindings, [c]commands or [s]ettings?  (press q to abort)
```

按 `c` 键，`ranger` 将提供你可以在该工具内使用的命令信息。例如，你可以通过输入 `:chmod` 来改变当前文件的权限，后面跟着预期的权限。例如，一旦选择了一个文件，你可以输入 `:chmod 700` 将权限设置为 `rwx------`。

输入 `:edit` 可以在 `nano` 中打开该文件，允许你进行修改，然后使用 `nano` 的命令保存文件。

#### 配置

启动之后 ranger 会创建一个目录 ~/.config/ranger/. 可以使用以下命令复制默认配置文件到这个目录:

```bash
ranger --copy-config=all
```

输出为：

```bash
# jack @ unix in ~/公共的/c文件 [日期: 周六 3月 27日, 时间: 22:27:45]
$ ranger --copy-config=all
creating: /home/jack/.config/ranger/rifle.conf
creating: /home/jack/.config/ranger/commands.py
creating: /home/jack/.config/ranger/commands_full.py
creating: /home/jack/.config/ranger/rc.conf
creating: /home/jack/.config/ranger/scope.sh

> Please note that configuration files may change as ranger evolves.
  It's completely up to you to keep them up to date.

> To stop ranger from loading both the default and your custom rc.conf,
  please set the environment variable RANGER_LOAD_DEFAULT_RC to FALSE.

```

+ rc.conf - 选项设置和快捷键
+ commands.py - 能通过 : 执行的命令
+ rifle.conf - 指定不同类型的文件的默认打开程序

> 基本操作

| 操作                                                         | 键位     |
| ------------------------------------------------------------ | -------- |
| 上一级列表                                                   | `h`      |
| 下一级列表                                                   | `l`      |
| 上一个文件                                                   | `k`      |
| 下一个文件                                                   | `j`      |
| go home                                                      | `gh`     |
| 新建标签                                                     | `gn`     |
| 查找                                                         | `f`      |
| 搜素                                                         | `/`      |
| 快速进入目录                                                 | `g`      |
| 进入文件                                                     | `Enter`  |
| 退出                                                         | `q`      |
| 显示隐藏文件                                                 | `zh`     |
| 复制                                                         | `yy`     |
| 粘贴                                                         | `pp`     |
| 剪切                                                         | `dd`     |
| 删除                                                         | `delete` |
| 目录顶端                                                     | `gg`     |
| 目录末尾                                                     | `G`      |
| 重命名                                                       | `cw`     |
| 文件排序                                                     | `o`      |
| 根据文件名进行排序(natural/basename)                         | `on/ob`  |
| 根据改变时间进行排序 (Change Time 文件的权限组别和文件自身数据被修改的时间) | `oc`     |
| 根据文件大小进行排序(Size)                                   | `os`     |
| 根据后缀名进行排序 (Type)                                    | `ot`     |
| 根据访问时间进行排序 (Access Time 访问文件自身数据的时间)    | `oa`     |
| 根据修改进行排序 (Modify time 文件自身内容被修改的时间)      | `om`     |

------

> 插件

`ranger`也有很多预览时用的插件 :

```bash
sudo apt-get install caca-utils # img2txt 图片
sudo apt-get install highlight  # 代码高亮
sudo apt-get install atool　    # 存档预览
sudo apt-get install w3m        # html页面预览
sudo apt-get install mediainfo  # 多媒体文件预览
sudo apt-get install catdoc     # doc预览
sudo apt-get install docx2txt   # docx预览
sudo apt-get install xlsx2csv   # xlsx预览

sudo apt-get install caca-utils highlight atool w3m mediainfo catdoc docx2txt xlsx2csv 
```





#### 总结

使用 `ranger` 的方法比本篇文章所描述的更多。该工具提供了一种非常不同的方式来列出 Linux 系统上的文件并与之交互，一旦你习惯了它的多级的目录和文件列表方式，并使用方向键代替 `cd` 命令来移动，就可以很轻松地在 Linux 的文件中导航。

### [Glances](https://mp.weixin.qq.com/s/C7qXS7gXH385n-yJjBxprQ)

top 命令是 Linux 中的实时任务管理器，也是 GNU/Linux 发行版中最常用的系统监控工具，用于查找系统中与性能相关的瓶颈，这有助于我们采取纠正措施。它具有一个很好的极简主义界面，并提供了一些合理的选项，使我们能够快速地更好地了解整体系统性能。

但是，有时要找到一个消耗大量系统资源的应用程序 / 过程非常棘手，这在 top 很难实现。由于 top 命令无法高亮显示占用大量 CPU，RAM 和其他资源的程序。

为了实现这种方法，我们引入了一个功能强大的名为 Glances 的系统监控程序，该程序自动高亮显示正在利用最高系统资源并提供有关 Linux/Unix 服务器的最大信息的程序。

**什么是 Glances？**

Glances 是使用 Python 语言编写的基于跨平台命令行 curses 的系统监视工具，该工具使用 psutil 库从系统中获取信息。使用 Glance，我们可以监视 CPU，平均负载，内存，网络接口，磁盘 I/O，进程和文件系统空间利用率。

Glances 是一个免费工具，并根据 GPL 许可可监视 GNU/Linux 和 FreeBSD 操作系统。Glances 中也提供了许多有趣的选项。在 Glances 中看到的主要功能之一是，我们可以在配置文件中设置阈值（小心，警告和严重），并且信息将以颜色显示，这表明系统中的瓶颈。

**Glances 功能**

- CPU 信息（与用户相关的应用程序，系统核心程序和空闲程序）。
- 总内存信息，包括 RAM，交换，可用内存等。
- 过去 1 分钟，5 分钟和 15 分钟的平均 CPU 负载。
- 网络连接的网络下载 / 上载速率。
- 进程总数，活动进程，睡眠进程等。
- 磁盘 I/O 相关（读或写）速度详细信息
- 当前安装的设备磁盘使用情况。
- 排名靠前的进程及其 CPU / 内存使用情况，名称和应用程序位置。
- 在底部显示当前日期和时间。
- 以红色高亮显示消耗最高系统资源的进程。

以下是 Glances 的示例屏幕截图。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6Gw1yibycMusdad5pwhqliaBOWaxmsMVZM1z2yDBFI8EojhamNrrtfiaIDG3IpLFkfgH9p9qJ6XFnISg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**在 Linux / Unix 系统中安装 Glances**

尽管它是一个较新的实用程序，但是您可以通过打开 EPEL 存储库，然后在终端上运行以下命令，在基于 Red Hat 的系统中安装 “Glances”。

**在 RHEL/CentOS/Fedora 上**

yum install -y glances

**在 Debian/Ubuntu/Linux Mint 上**

```bash
sudo apt-add-repository ppa:arnaud-hartmann/glances-stable
sudo apt-get update
sudo apt-get install glances
```



**Glances 使用**

首先，在终端上启动 glances。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6Gw1yibycMusdad5pwhqliaBOXMsNlUTLAcLkU3ib8BtwZIeKZra1LGd2qYOicLORsiaakQVLlrdcVmsTA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

按 “q” 或（“ ESC” 或 “ Ctrl＆C” 也可以）从 Glances 终端退出。

默认情况下，间隔时间设置为 “1” 秒。但是，您可以在从终端运行 glances 时定义自定义间隔时间。

glances -t 2

**glances 颜色代码**

Glances 颜色代码的含义：

- 绿色：OK（一切都很好）
- 蓝色：CAREFUL 小心（需要注意）
- 紫色：WARNING 警告（警报）
- 红色：CRITICAL 严重（危险）

我们可以在配置文件中设置阈值。默认情况下，阈值设置为（careful=50, warning=70 and critical=90），我们可以根据需要进行自定义。默认配置文件位于 “/etc/glances/glances.conf”。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6Gw1yibycMusdad5pwhqliaBOpdA5yt3unjdRQoD3p8TDic8lERHTLW7FXokBVIBL9UTYGecP7p2DL9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**glances 选项**

除了几个命令行选项外，扫视还提供了更多的热键，可在扫视运行时查找输出信息。以下是几个热键的列表。

- a - 自动对进程进行排序
- c - 按 CPU％排序进程
- m - 按 MEM％排序过程
- p - 按名称对进程进行排序
- i - 按 I/O 速率对进程进行排序
- d - 显示 / 隐藏磁盘 I/ O 统计信息
- f - 显示 / 隐藏文件系统
- n - 显示 / 隐藏网络统计信息
- s - 显示 / 隐藏传感器统计信息
- y - 显示 / 隐藏 hddtemp 统计信息
- l - 显示 / 隐藏日志
- b - 网络 I/Oools 的字节或位
- w - 删除警告日志
- x - 删除警告和重要日志
- 1 - 全局 CPU 或每个 CPU 的统计信息
- h - 显示 / 隐藏此帮助屏幕
- t - 查看网络 I/O 的组合
- u - 查看累积的网络 I/O
- q - 退出（Esc 和 Ctrl-C 也可以）

**在远程系统上使用 Glances**

使用 Glances，您甚至还可以监视远程系统。要在远程系统上使用 “glances”，请在服务器上运行 “ glances -s”（-s 启用服务器 / 客户端模式）命令。

\# glances -s

Define the password for the Glances server
Password:
Password (confirm):
Glances server is running on 0.0.0.0:61209

注意：发出 “glances” 命令后，它将提示您定义 Glances 服务器的密码。定义密码并按 Enter，您将看到端口 61209 上运行的内容。

现在，转到远程主机并执行以下命令，通过指定 IP 地址或主机名来连接到 Glances 服务器，如下所示。这是我的 glances 服务器 IP 地址 “172.16.27.56”。

\# glances -c -P 172.16.27.56

以下是用户在服务器 / 客户端模式下使用 Glances 时必须知道的一些要点。

\* 在服务器模式下，可以设置绑定地址 - B ADDRESS 和侦听 TCP 端口 - p PORT。
\* 在客户端模式下，可以设置服务器的 TCP 端口 - p PORT。
\* 默认绑定地址为 0.0.0.0，但它在端口 61209 上的所有网络接口上侦听。
\* 在服务器 / 客户端模式下，限制由服务器端设置。
\* 您还可以定义密码来访问服务器 - P 密码。

**总结**

对大多数用户来说，glance 是一个资源友好型工具。但是，如果您是一个系统管理员，希望通过浏览命令行来快速了解系统的总体 “想法”，那么这个工具将是系统管理员必须拥有的工具。





### [sysstat](https://mp.weixin.qq.com/s/zfVcyDdBTiy5y8wFVd7x3A)

### 简介

sysstat 包含了许多商用 Unix 通用的各种工具，用于监视系统性能和活动情况：

- iostat，统计设备和分区的 CPU 信息以及 IO 信息
- mpstat，统计处理器相关的信息
- pidstat，统计 Linux 进程的相关信息：IO、CPU、内存等
- tapstat，统计磁盘驱动器的相关信息
- cifsiostat，统计 CIFS 信息

sysstat 还包含使用 cron 或 systemd 执行定时任务的工具（默认的采样时间是 10 分钟，可以修改。），用来收集历史性能和活动数据：

- sar，统计并保存系统活动信息
- sadc，sar 的后端，是系统活动数据的收集齐
- sa1，收集二进制数据并将其村粗在系统活动每日数据文件中，是使用 cron 或 systemd 运行的 sar 前端
- sa2，汇总日常系统活动，是使用 cron 或 systemd 运行的 sar 前端
- sadf，以多种格式显示 sar 收集的数据，如 CSV、XML、JSON 等，并可以用来与其他程序进行数据交换。

sar 收集的系统统计信息包括：

- 输入 / 输出和传输速率统计信息
- CPU 统计信息，包括对虚拟化体系结构的支持
- 内存、交换空间利用率的统计信息
- 虚拟内存、分页和故障统计
- 进程创建活动信息
- 中断信息统计，包括 APIC 中断，硬件中断，软件中断
- 网络统计信息，包括网络接口活动，网络设备故障，IP、TCP、UDP、ICMP 协议的流量统计，支持 IPv6
- 光纤通道流量统计
- 基于软件的网络统计信息
- NFS 服务器和客户端活动
- 套接字统计
- 运行队列和系统负载统计
- 内核利用率统计信息
- 交换统计
- TTY 设备活动
- 电源管理统计信息
- USB 设备事件
- 文件系统利用率（节点和块）
- 失速信息统计

sysstat 的主要功能包括：

- 在报告中显示平均统计值。

- 检测动态创建或注册的新设备（磁盘，网络接口等）。

- 支持 UP 和 SMP 计算机，包括具有超线程或多核处理器的计算机。

- 支持热插拔 CPU 和 tickless 的 CPU，自动检测正在动态禁用或启用的处理器。

- 适用于许多不同的体系结构，无论是 32 位还是 64 位。

- 占用很少的 CPU 时间（用 C 编写）。

- 可以将 sar/sadc 收集的系统统计信息保存在文件中。

- 可以以各种不同的格式（CSV，XML，JSON，SVG 等）导出由 sar/sadc 收集的系统统计信息。

- iostat 可以显示由用户空间中的驱动程序管理的设备的统计信息。

- 彩色输出，易于阅读和理解。

  

  ![图片](https://mmbiz.qpic.cn/mmbiz_png/DSU8cv1j3ibTjriaLlaIQPUzEnFhkkJDjB2VI3pFusQUy7Yia8ibWic2U2WNPatJrAW1yiasvR3DiaTYPpAKibPKSQWibgw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  

- 国际化支持，systat 已经被翻译为多种不同的语言。

- 可以自动选择用于显示尺寸的单位，以便于阅读，参阅选项 --human

  

  ![图片](https://mmbiz.qpic.cn/mmbiz_png/DSU8cv1j3ibTjriaLlaIQPUzEnFhkkJDjBSrM1Nr1TfLFZdcWUXLtD7qiaGrXljYAyibicmUfbrpAhrVHXTYdFQ2D6g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  

- 可以生成 SVG 图形，并显示在浏览器中。

  

  ![图片](https://mmbiz.qpic.cn/mmbiz_jpg/DSU8cv1j3ibTjriaLlaIQPUzEnFhkkJDjBfSsUo7lx1an52y9foXbicSIpNWibY1Z00UQt55FnX1ngW8iaPPm48j57w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  

  ![图片](https://mmbiz.qpic.cn/mmbiz_png/DSU8cv1j3ibTjriaLlaIQPUzEnFhkkJDjBmWQFkbNlPtKKdVs4eHHt2h0PCkJbMztprrbvH9aTbewXWJVZzgy2Qg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  

  ![图片](https://mmbiz.qpic.cn/mmbiz_png/DSU8cv1j3ibTjriaLlaIQPUzEnFhkkJDjBO66yujCQ8ktprwwsGhxekQAReWwX0t17YiaaqyibeCsCuw0UhiaCib4n0Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

项目地址：

https://github.com/sysstat/sysstat

### 安装使用

sysstat 的安装使用非常简单，安装包后，启动服务即可。

- REHL/Fedora/CentOS 使用以下命令安装：

```bash
$ sudo yum install sysstat
```

CentOS 和 Fedora 系统使用 /etc/cron.d 中的 cron 作业来调用收集器进程，并且默认情况下已启用它。在最新版本中，使用 systemd 代替 cron。可能需要启用 sysstat 服务：

```bash
$ sudo systemctl enable sysstat
$ sudo systemctl start sysstat
```

- Ubuntu 使用以下命令安装：

```bash
$ sudo apt-get install sysstat
```

然后启用数据收集功能：

```bash
// 编辑/etc/default/sysstat配置文件，将ENABLED="false"改为ENABLED="true"，保存即可
$ sudo vi /etc/default/sysstat
```

重新启动 syastat 服务：

```bash
$ sudo service sysstat restart
```

- 源代码安装：下载源代码：

```bash
$ git clone git://github.com/sysstat/sysstat
```

编译安装：

```bash
$ cd sysstat
$ ./configure
$ make
$ sudo make install
```



###  dstat

+ sudo apt install dstat

###  bpytop

+ python3 -m pip install psutil
+ pip3 install bpytop --upgrade
+ sudo snap install bpytop

配置文件：/home/jack/.config/bpytop/bpytop.conf

###  duf

+ wget https://github.com/muesli/duf/releases/download/v0.5.0/checksums.txt
+ wget https://github.com/muesli/duf/releases/download/v0.5.0/duf_0.5.0_linux_amd64.deb
+ sha256sum --ignore-missing -c checksums.txt
+ sudo dpkg -i duf_0.5.0_linux_amd64.deb

###  plots

+ `sudo add-apt-repository ppa:apandada1/plots`
+ `sudo apt update`
+ `sudo apt install plots`




###  窗口管理器i3



###  窗口管理器awesome





### WSL



```json
//这是windows terminal 的配置文件
{
  // 默认打开的 Profile GUID（下面会详细介绍）
  "defaultProfile": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
  // 终端窗口默认大小
  "initialCols": 120,
  "initialRows": 30,
  // 亮色或暗色主题，可选值 "light", "dark", "system"
  "requestedTheme": "dark",
  // 合并标题栏和标签栏
  "showTabsInTitlebar": true,
  // 如果 showTabsInTitlebar 与本值同为 false 时，自动隐藏标签栏
  "alwaysShowTabs": true,
  // 在标题栏上显示当前活动标签页的标题
  "showTerminalTitleInTitlebar": true,
  // 双击选择时用于分词的字符
  "wordDelimiters": " /\\()\"'-.,:;<>~!@#$%^&*|+=[]{}~?\u2502",
  // 选择时复制到剪贴板
  "copyOnSelect": true,
  // 标签页宽度不固定
  "tabWidthMode": "titleLength",
  //***************************************************************************************
  "$schema": "https://aka.ms/terminal-profiles-schema",
  "copyFormatting": false,

  "profiles": {
    "defaults": {
      // 所有 Profile 共用的设置可以放这里，就不用写多次了
      // 字体设置
      "fontFace": "Cascadia Code",
      //"fontFace": "DejaVu Sans Mono",
      //"fontFace": "Monospace Regular",
      // "fontFace": "文泉驿微米黑",
      "fontFace": "DroidSansMono NF",
      "fontSize": 9,
      // 光标类型，可选值 "vintage" ( ▃ ), "bar" ( ┃ ), "underscore" ( ▁ ), "filledBox" ( █ ), "emptyBox" ( ▯ )
      "cursorShape": "bar",
      // 是否开启背景亚克力透明效果（窗口失去焦点时无效）
      "useAcrylic": false,
      "cursorColor": "#0cee32",
      "historySize": 2001,
      "snapOnInput": true,
      "acrylicOpacity": 0.8,
      "backgroundImageOpacity": 0.1,
      "foreground": "#A7B191",
      "background": "#000000",
      //"background": "#013456",
      "padding": "0, 0, 0, 0",
      "hidden": false,
      //"startingDirectory": "//wsl$/Ubuntu-20.04/home/junjie",
      "commandline": "wsl -d Ubuntu-20.04 -e bash -c \"cd ~;bash\"",
      "colorScheme": "Snazzy"
      //"colorScheme": "Tango Dark"
      //"colorScheme": "Homebrew"
      //"colorScheme": "Solarized Dark"
      //"colorScheme": "Solarized Light"
      //"colorScheme": "Night Owlish Light"
      //"colorScheme": "Campbell"
      //"colorScheme": "Snazzy"

    },
    "list": [
      {
        "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
        "hidden": false,
        "name": "Ubuntu-20.04",
        "tabTitle": "Ubuntu20.04",
        "source": "Windows.Terminal.Wsl"
      },
      {
        "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
        "hidden": false,
        "name": "Ubuntu",
        "tabTitle": "Ubuntu",
        "source": "Windows.Terminal.Wsl"
      },
      {
        // Make changes here to the powershell.exe profile.
        "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
        "name": "Windows PowerShell",
        "tabTitle": "PowerShell",
        "commandline": "powershell.exe",
        "backgroundImageOpacity": 0.1
      },
      {
        // Make changes here to the cmd.exe profile.
        "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
        "name": "命令提示符",
        "commandline": "cmd.exe",
        "tabTitle": "命令提示符",
        "hidden": false
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      }
    ]
  },

  // Add custom color schemes to this array.
  // To learn more about color schemes, visit https://aka.ms/terminal-color-schemes
  "schemes": [
    {
      "name": "Tango Dark",
      "foreground": "#A7B191",
      "background": "#0C0C0C",
      "black": "#000000",
      "red": "#cc0000",
      "green": "#4e9a06",
      "yellow": "#c4a000",
      "blue": "#3465a4",
      "purple": "#75507b",
      "cyan": "#06989a",
      "white": "#d3d7cf",
      "brightBlack": "#555753",
      "brightRed": "#ef2929",
      "brightGreen": "#8ae234",
      "brightYellow": "#fce94f",
      "brightBlue": "#729fcf",
      "brightPurple": "#ad7fa8",
      "brightCyan": "#34e2e2",
      "brightWhite": "#eeeeec",
      "background": "#000000",
      "foreground": "#D3D7CF"
    },
    {
      "name": "Snazzy",
      "black": "#000000",
      "red": "#fc4346",
      "green": "#50fb7c",
      "yellow": "#f0fb8c",
      "blue": "#49baff",
      "purple": "#fc4cb4",
      "cyan": "#8be9fe",
      "white": "#ededec",
      "brightBlack": "#555555",
      "brightRed": "#fc4346",
      "brightGreen": "#50fb7c",
      "brightYellow": "#f0fb8c",
      "brightBlue": "#49baff",
      "brightPurple": "#fc4cb4",
      "brightCyan": "#8be9fe",
      "brightWhite": "#ededec",
      "background": "#000000",
      "foreground": "#ebece6"
    },
    {
      "name": "Homebrew",
      "black": "#000000",
      "red": "#FC5275",
      "green": "#00a600",
      "yellow": "#999900",
      "blue": "#6666e9",
      "purple": "#b200b2",
      "cyan": "#00a6b2",
      "white": "#bfbfbf",
      "brightBlack": "#666666",
      "brightRed": "#e50000",
      "brightGreen": "#00d900",
      "brightYellow": "#e5e500",
      "brightBlue": "#0000ff",
      "brightPurple": "#e500e5",
      "brightCyan": "#00e5e5",
      "brightWhite": "#e5e5e5",
      "background": "#283033",
      "foreground": "#00ff00"
    },
    {
      "name": "Solarized Dark",
      "foreground": "#FDF6E3",
      "background": "#073642",
      "colors": [
        "#073642",
        "#D30102",
        "#859900",
        "#B58900",
        "#268BD2",
        "#D33682",
        "#2AA198",
        "#EEE8D5",
        "#002B36",
        "#CB4B16",
        "#586E75",
        "#657B83",
        "#839496",
        "#6C71C4",
        "#93A1A1",
        "#FDF6E3"
      ]
    },
    {
      "name": "Solarized Light",
      "foreground": "#073642",
      "background": "#FDF6E3",
      "colors": [
        "#073642",
        "#D30102",
        "#859900",
        "#B58900",
        "#268BD2",
        "#D33682",
        "#2AA198",
        "#EEE8D5",
        "#002B36",
        "#CB4B16",
        "#586E75",
        "#657B83",
        "#839496",
        "#6C71C4",
        "#93A1A1",
        "#FDF6E3"
      ]
    },
    {
      "name": "Night Owlish Light",
      "background": "#FFFFFF",
      "black": "#011627",
      "blue": "#4876D6",
      "brightBlack": "#7A8181",
      "brightBlue": "#5CA7E4",
      "brightCyan": "#00C990",
      "brightGreen": "#49D0C5",
      "brightPurple": "#697098",
      "brightRed": "#F76E6E",
      "brightWhite": "#989FB1",
      "brightYellow": "#DAC26B",
      "cyan": "#08916A",
      "foreground": "#403F53",
      "green": "#2AA298",
      "purple": "#403F53",
      "red": "#D3423E",
      "white": "#7A8181",
      "yellow": "#DAAA01"
    },
    {
      "name": "Campbell",
      "foreground": "#A7B191",
      "background": "#0C0C0C",
      "colors": [
        "#0C0C0C",
        "#C50F1F",
        "#13A10E",
        "#C19C00",
        "#0037DA",
        "#881798",
        "#3A96DD",
        "#CCCCCC",
        "#767676",
        "#E74856",
        "#16C60C",
        "#F9F1A5",
        "#3B78FF",
        "#B4009E",
        "#61D6D6",
        "#F2F2F2"
      ]
    }
  ],

  "keybindings": [
    {
      "command": {
        "action": "copy",
        "singleLine": false
      },
      "keys": "ctrl+c"
    },
    {
      "command": "paste",
      "keys": "ctrl+v"
    },
    //    {
    //      "command": "find",
    //     "keys": "ctrl+shift+f"
    //   },
    {
      "command": {
        "action": "splitPane",
        "split": "auto",
        "splitMode": "duplicate"
      },
      "keys": "alt+shift+d"
    },

    {
      "command": "newTab",
      "keys": ["ctrl+N"]
    },
    {
      "command": "closeTab",
      "keys": ["ctrl+w"]
    },
    {
      "command": "closePane",
      "keys": ["ctrl+w"]
    },
    {
      "command": "find",
      "keys": ["ctrl+f"]
    }, //搜索框

    {
      "command": "increaseFontSize",
      "keys": ["ctrl+]"]
    }, //增大字体
    {
      "command": "decreaseFontSize",
      "keys": ["ctrl+["]
    }, //减小字体
    {
      "command": "duplicateTab",
      "keys": ["ctrl+shift+d"]
    },
    {
      "command": "newTabProfile0",
      "keys": ["ctrl+shift+1"]
    },
    {
      "command": "newTabProfile1",
      "keys": ["ctrl+shift+2"]
    },
    {
      "command": "newTabProfile2",
      "keys": ["ctrl+shift+3"]
    },
    {
      "command": "newTabProfile3",
      "keys": ["ctrl+shift+4"]
    },
    {
      "command": "newTabProfile4",
      "keys": ["ctrl+shift+5"]
    },
    {
      "command": "newTabProfile5",
      "keys": ["ctrl+shift+6"]
    },
    {
      "command": "newTabProfile6",
      "keys": ["ctrl+shift+7"]
    },
    {
      "command": "newTabProfile7",
      "keys": ["ctrl+shift+8"]
    },
    {
      "command": "newTabProfile8",
      "keys": ["ctrl+shift+9"]
    },
    {
      "command": "openNewTabDropdown",
      "keys": ["ctrl+shift+space"]
    },
    {
      "command": "openSettings",
      "keys": ["ctrl+,"]
    }, //打开配置文件
    {
      "command": "nextTab",
      "keys": ["ctrl+tab"]
    }, //上一个tab
    {
      "command": "prevTab",
      "keys": ["ctrl+shift+tab"]
    }, //下一个tab
    {
      "command": "scrollDown",
      "keys": ["ctrl+shift+down"]
    }, //向下滚动
    {
      "command": "scrollUp",
      "keys": ["ctrl+shift+up"]
    }, //向上滚动
    {
      "command": "scrollUpPage",
      "keys": ["pgup"]
    }, //向上翻页
    {
      "command": "scrollDownPage",
      "keys": ["pgdn"]
    }, //向下翻页
    {
      "command": "switchToTab0",
      "keys": ["ctrl+alt+1"]
    },

    //系统默认的是：水平分屏：Alt + Shift + 减号，垂直分屏：Alt + Shift + 加号
    //但是这里变为：水平分屏：ctrl + Shift + 减号，垂直分屏：ctrl + Shift + 加号
    {
      "command": {
        "action": "splitPane",
        "split": "vertical",
        "splitMode": "duplicate"
      },
      "keys": "ctrl+shift+="
    },
    {
      "command": {
        "action": "splitPane",
        "split": "horizontal",
        "splitMode": "duplicate"
      },
      "keys": "ctrl+shift+-"
    },
    {
      "command": { "action": "moveFocus", "direction": "down" },
      "keys": ["alt+down"]
    },
    {
      "command": { "action": "moveFocus", "direction": "left" },
      "keys": ["alt+left"]
    },
    {
      "command": { "action": "moveFocus", "direction": "right" },
      "keys": ["alt+right"]
    },
    {
      "command": { "action": "moveFocus", "direction": "up" },
      "keys": ["alt+up"]
    },
    {
      "command": { "action": "resizePane", "direction": "down" },
      "keys": ["alt+shift+down"]
    },
    {
      "command": { "action": "resizePane", "direction": "left" },
      "keys": ["alt+shift+left"]
    },
    {
      "command": { "action": "resizePane", "direction": "right" },
      "keys": ["alt+shift+right"]
    },
    {
      "command": { "action": "resizePane", "direction": "up" },
      "keys": ["alt+shift+up"]
    }
  ]
}

```





------









##  linux系统其它软件



###  Python IDE Anaconda



###  Java IDE IntelliJ



###  Matlab



###  QT



###  Code::Block



###  Eclipse



###  Emacs



###









##  linux软件自动安装脚本

根据以上软件的安装流程，可以写一个bash脚本实现装机软件自动化安装流程，这是比较高效的方式，大幅度释放劳动力，但是前期此脚本的撰写是费事的，但是提升了bash脚本的编写能力。



# suckless 套装

## fatal error: X11/XX.h: No such file or directory

linux系统源码安装软件经常会遇到库文件不存在，错误信息大多如下：

```bash
BBoard.c:27:28: error: X11/IntrinsicP.h: No such file or directory
BBoard.c:28:27: error: X11/Intrinsic.h: No such file or directory
BBoard.c:29:23: error: X11/Xutil.h: No such file or directory
BBoard.c:30:28: error: X11/StringDefs.h: No such file or directory
```



安装库文件libx11-dev

+ sudo apt-get install libx11-dev

安装依赖文件

```bash
sudo apt-get install apt-file
sudo apt-file update
apt-file search XXXX.h
```

如：安装Intrinsic.h

```bash
sudo apt-get install apt-file
sudo apt-file update
apt-file search Intrinsic.h
```

搜索结果如下：

```bash
libxt-dev: usr/include/X11/Intrinsic.h
```

因此，只需安装libxt-dev即可：

```bash
sudo apt-get install libxt-dev
```

在安装dmenu/st/dwm过程中会发现少了:

```bash
dwm.c:40:10: fatal error: X11/extensions/Xinerama.h: 没有那个文件或目录
   40 | #include <X11/extensions/Xinerama.h>
      |          ^~~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
make: *** [Makefile:18：dwm.o] 错误 1


```

通过上述方法可以发现需要安装以下

```bash
sudo apt install x11-xserver-utils libxrandr-dev libimlib2-dev
sudo apt install libharfbuzz-dev
```

## 安装st

```bash
$: git clone  https://github.com/junjiecjj/st-1.git  或
$: git clone  https://github.com/junjiecjj/st-2.git
$: cd st-1
$: sudo make clean install 
```



## 安装dmenus

dmenu 类似于 kde 下菜单栏里的应用启动器，但是比起 kde 来要方便快捷多了。使用方法是按下 win+s 键，dwm 顶部的菜单栏里就会出现 dmenu，然后输入你想打开的图形 gui 程序名，dmenu 会根据你输入的字符迅速自动搜索出当前系统里所有符合输入内容的程序名，按键盘的左右方向键选择想要打开的程序，最后按 Enter 键即可打开。

使用 dmenu，你可以很方便快捷地打开 kde、gnome、xfce 等主流桌面环境里自带的应用程序。由于我之前装的是 kde plasma，所以也习惯了用 kde 家的 dolphin 文件管理器、kate 文本编辑器、okular PDF 阅读器，kde 的自带应用做得确实不错。

注意 dmenu 只能打开带图形界面 gui 的程序，没有 gui 的程序用 dmenu 打开是看不到反应的；没有 gui 的纯命令行程序还是通过 alacritty 终端输入程序名打开吧。

```bash
$: git clone https://github.com/junjiecjj/dmenu.git
$: cd dmenu
$: sudo make clean install 
```



## 安装dwm



###  xcompmgr+transset-df

xcompmgr+transset-df 可以实现阴影、原生窗口透明(配合 transset 工具)等特效.

```bash
$: sudo apt-get install xcompmgr libxcomposite1 libxcomposite-dev libxfixes3 libxfixes-dev libxdamage1 libxdamage-dev libxrender1 libxrender-dev
# http://forchheimer.se/transset-df/ 下载transset-df压缩包,download transset-df from this page
$: tar zxf transset-df-X.tar.gz //X为版本号
$: cd transset-df-X/
$: make
$: sudo make install  
```

在`.xinitrc`中添加：



### picom

picom 可以实现阴影、原生窗口透明(配合 transset 工具)等特效.

在`.xinitrc`中添加：`picom -b`就可以使用compton

```bash
$: sudo apt install libxext-dev libxcb1-dev libxcb-damage0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev libxcb-randr0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev libxcb-xinerama0-dev libxcb-glx0-dev libpixman-1-dev libdbus-1-dev libconfig-dev libgl1-mesa-dev libpcre2-dev libpcre3-dev libevdev-dev uthash-dev libev-dev libx11-xcb-dev

$ git clone https://github.com/jonaburg/picom
$ cd picom
$ git submodule update --init --recursive
$ meson --buildtype=release . build
$ LDFLAGS="-L/path/to/libraries" CPPFLAGS="-I/path/to/headers" meson --buildtype=release . build
$ ninja -C build
# To install the binaries in /usr/local/bin (optional)
$ sudo ninja -C build install

```





### compton

compton 可以实现阴影、原生窗口透明(配合 transset 工具)等特效

若要禁用所有的阴影特效，需要加上 `-C` 和 `-G` 这两个参数:

```bash
compton -CG
```

若要在登录 X 会话的过程中，以后台进程（[Daemon](https://wiki.archlinux.org/index.php/Daemon)）的形式自动运行 compton，必须加上 `-b` 参数：

```bash
compton -b
```

将前面的参数一起用，效果也就一起有了:

```bash
compton -CGb
```

最后这个例子演示了如何使用需要指定数值的参数:

```bash
compton -cCGfF -o 0.38 -O 200 -I 200 -t 0 -l 0 -r 3 -D2 -m 0.88
```

在`.xinitrc`中添加：`compton -cCGfF -o 0.38 -O 200 -I 200 -t 0 -l 0 -r 3 -D2 -m 0.88`就可以使用compton。



### ploybar

```bash
$ sudo apt update
$ sudo apt-get install cmake cmake-data libcairo2-dev libxcb1-dev libxcb-ewmh-dev libxcb-icccm4-dev libxcb-image0-dev libxcb-randr0-dev libxcb-util0-dev libxcb-xkb-dev pkg-config python3-xcbgen xcb-proto libxcb-xrm-dev i3-wm libasound2-dev libmpdclient-dev libiw-dev libcurl4-openssl-dev libpulse-dev
$ sudo apt install libxcb-composite0-dev
$ sudo apt install libjsoncpp-dev
$ sudo ln -s /usr/include/jsoncpp/json/ /usr/include/json

$ git clone https://github.com/jaagr/polybar.git

$ cd polybar && ./build.sh
//启动polybar
$ polybar example


或者
$ vim  /etc/apt/sources.list
增加以下
deb http://cz.archive.ubuntu.com/ubuntu groovy main universe
然后
$ sudo apt update
$ sudo apt install polybar
```





### 其他服务软件

```bash
#基础依赖
$ sudo apt-get install suckless-tools libx11-dev libxft-dev libxinerama-dev gcc make

#背光灯调整工具
$ sudo apt install light
#为背光灯调整工具设置 sudo 免密码
$ sudo visudo
#然后在文本最后加入如下代码
jack ALL=NOPASSWD:/usr/bin/light

#安装截图工具
$ sudo apt install  

#安装数字键盘工具, 用于进入dwm桌面后自动开启数字键盘
$ sudo apt install numlockx

#virtualbox
$ sudo apt-get install virtualbox-guest-utils virtualbox-guest-X11

#电源监控工具
$ sudo apt install acpi acpitool

#透明配置支持
$ sudo apt install compton
#或者用下面的工具
$ sudo apt install xcompmgr

#背景图片设置工具
$ sudo apt install feh

#用于显示笔记本电脑电池的电量
$ sudo apt install acpi     

#用于屏幕亮度的调节
$ sudo apt install xbacklight

#屏幕前显示键盘触发键
$: sudo apt install screenkey

//安装 nm-applet
$: sudo apt-get install network-manager-gnome


//锁屏
$: sudo apt install slimlock

//rofi 是一个快捷的程序启动器
$: sudo apt install rofi
```

linux中设置状态栏的命令为 `xsetroot`  ： 定制、显示简易的系统状态栏(电池电量、音量、日期、时间等)；
linux中调节音量的命令为` amixer`  ： 用于系统音量的调节，在alsa-utils包中，`alsa-utils` 声音管理



### 安装dwm

先安装dwmstatus

```bash
$: git clone git://git.suckless.org/dwmstatus
$: cd dwmstatus
$: make
$: sudo ake PREFIX=/usr install
# 增加 dwmstatus 2>&1 >/dev/null &  到~/.xinitrc
```

再安装dwm

```bash
$: git clone  https://github.com/junjiecjj/dwm.git
$: cd dmenu
$: sudo make clean install 
```



### 输入法、开机启动

fcitx -d&,(在 exec dwm 之前) 这样使用 slim 或者 startx 后，输入法就可用了,

vim ~/.xinitrc：    #不要sudo！！

```bash
# .xinitrc

# twm &   #注释掉或直接删掉
# xclock -geometry 50x50-1+1 &   #注释掉或直接删掉
# xterm -geometry 80x50+494+51 &   #注释掉或直接删掉
# xterm -geometry 80x20+494-0 &   #注释掉或直接删掉
# exec xterm -geometry 80x66+0+0 -name login   #注释掉或直接删掉
#xrandr --setprovideroutputsource modesetting NVIDIA-0
#xrandr --auto   #关于xrandr的这两行配置，每一行后面都不要加上"&"，否则nvidia驱动不能正常加载，会导致黑屏
eval "$(dbus-launch --sh-syntax --exit-with-session)" &
export GTK_IM_MODULE=fcitx &
export QT_IM_MODULE=fcitx &
export XMODIFIERS=@im=fcitx &
fcitx &   #使用fcitx中文输入法
picom -CGb &   #使用picom窗口渲染器
while xsetroot -name "Wifi:$(cat /sys/class/net/<你的无线网卡名>/operstate)|Ethernet:$(cat /sys/class/net/<你的有线网卡名>/operstate)|Battery:$(acpi -b | awk '{print $4}' | cut -d"," -f1)|Volume:$(amixer get Master | awk -F'[][]' 'END{ print $4":"$2 }')|$(LC_ALL='C' date +'%F[%b %a] %R')"   #在dwm的菜单栏里显示一个简易的系统状态栏，包括wifi、有线网、电池电量、音量、日期和时间(24小时制)。喜欢功能丰富的同学可以自己去安装配置polybar、conky、i3status、slstatus等，从中选一个。
do
sleep 60   #每隔1分钟刷新一次状态栏
done &
exec  ~/.config/polybar/launch.sh
exec_always --no-startup-id $HOME/.config/polybar/launch.sh
#while true; do
#xsetroot -name "Bat.$(acpi -b | awk '{print $4}') | Vol.$(amixer get Master| tail -n 1 | awk ‘{print $5}' | tr -d '[]') $(LC_ALL='C' date +'%F[%b %a] %R')"
#sleep 20
#done &

while habak -ms -hi ~/tupian/   #让habak从你的壁纸目录中随机选择一张屏幕壁纸显示
do
sleep 600   #让habak每隔10分钟随机切换一张屏幕壁纸
done &
numlockx &   #自动自动开启数字键盘
exec dwm   #万事具备，启动dwm


```





### 两种启动方式

1.登陆管理器 DM;

 2.通过 startx 脚本命令进入

> 使用 display manager 启动,

以 ubuntu 20.04 为例，ubuntu 20.04 使用 gdm3 做为 display manager，配置完成之后可以在登录界面选择 dwm 作为桌面启动，如下图：

![img](https://pic4.zhimg.com/80/v2-4882d922beda138d5e779beff552187b_720w.jpg)

具体配置方式，进入 `/usr/share/xsessions/` 目录，新建文件 `dwm.desktop`, 输入内容：


```bash
[Desktop Entry]
Encoding=UTF-8
Name=Dwm
Comment=Dynamic window manager
Exec=dwm
Icon=dwm
Type=XSession
```



>  使用 startx 命令从文字界面启动 (推荐)

此方式开机更加快速，使用更加灵活，系统资源占用更少。

**首先将系统改为默认进入文字界面**

修改 grub 配置，打开文件 `/etc/default/grub`, 将 `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` 的改为 `GRUB_CMDLINE_LINUX_DEFAULT="text"` 然后执行命令

```bash
$ sudo update-grub
```

将启动等级改为多用户等级，执行如下命令：

```bash
$ systemctl set-default multi-user.target 
// 如果想改回启动图形界面执行下面
$ systemctl set-default graphical.target
```

最后修改 `~/.xsession` 文件（如果没有就新建），在最后一行加入

```bash
exec dwm
```

重启电脑，执行 startx 命令，直接进入 dwm，同时也可以执行 `sudo systemctl start gdm.service` 命令，打开 gdm3 的用户登录界面。

<font face="黑体" color=red size=7>或</font>

```bash
sudo rc-update delete xdm default   #禁用开机启动登陆管理器DM

sudo reboot

重启后进入简陋的tty界面，输入用户名和密码登陆进系统后

startx   #不要sudo！
```

即可进入 dwm。此方法脱离了登陆管理器 DM，更加灵活。



## 快捷键

```c


XK_apostrophe ,    英文是'号，引号

 XF86XK_AudioLowerVolume ,

XK_period,      {".", XK_period}, 点号

XK_comma,   逗号,

XF86XK_AudioMute, 
XF86XK_AudioRaiseVolume, 
XK_bracketleft,    "["
XK_backslash,      {"\", XK_backslash},   
XK_bracketright,   "]"
XK_Print,     PrtSc
XK_semicolon,    ;    {";", XK_semicolon},  分号
XK_Scroll_Lock      {"SCR", XK_Scroll_Lock},
XK_grave      {"`", XK_grave}, 反引号



{"BAC", XK_BackSpace},        {"BS", XK_BackSpace},        {"BKS", XK_BackSpace},
{"BRE", XK_Break},        {"CAN", XK_Cancel},         {"CAP", XK_Caps_Lock},
{"DEL", XK_Delete},        {"DOW", XK_Down},        {"END", XK_End},
{"ENT", XK_Return},        {"ESC", XK_Escape},        {"HEL", XK_Help},
{"HOM", XK_Home},        {"INS", XK_Insert},        {"LEF", XK_Left},
{"NUM", XK_Num_Lock},        {"PGD", XK_Next},        {"PGU", XK_Prior},
{"PRT", XK_Print},        {"RIG", XK_Right},        {"SCR", XK_Scroll_Lock},
{"TAB", XK_Tab},        {"UP", XK_Up},            {"F1", XK_F1},
{"F2", XK_F2},            {"F3", XK_F3},            {"F4", XK_F4},
{"F5", XK_F5},            {"F6", XK_F6},            {"F7", XK_F7},
{"F8", XK_F8},            {"F9", XK_F9},            {"F10", XK_F10},
{"F11", XK_F11},        {"F12", XK_F12},        {"SPC", XK_space},
{"SPA", XK_space},        {"LSK", XK_Super_L},         {"RSK", XK_Super_R},
{"MNU", XK_Menu},        {"~", XK_asciitilde},        {"_", XK_underscore},
{"[", XK_bracketleft},        {"]", XK_bracketright},        {"!", XK_exclam},
{"\"", XK_quotedbl},         {"#", XK_numbersign},        {"$", XK_dollar},
{"%", XK_percent},        {"", XK_ampersand},         {"'", XK_quoteright},
{"*", XK_asterisk},        {"+", XK_plus},            {",", XK_comma},
{"-", XK_minus},        {".", XK_period},         {"?", XK_question},
{"<", XK_less},            {">", XK_greater},        {"=", XK_equal},
{"@", XK_at},            {":", XK_colon},        {";", XK_semicolon},
{"\\", XK_backslash},         {"`", XK_grave},        {"{", XK_braceleft},
{"}", XK_braceright},        {"|", XK_bar},            {"^", XK_asciicircum},
{"(", XK_parenleft},        {")", XK_parenright},         {" ", XK_space},
{"/", XK_slash},        {"\t", XK_Tab},            {"\n", XK_Return},
{"LSH", XK_Shift_L},        {"RSH", XK_Shift_R},        {"LCT", XK_Control_L},
{"RCT", XK_Control_R},        {"LAL", XK_Alt_L},        {"RAL", XK_Alt_R},
{"LMA", XK_Meta_L},        {"RMA", XK_Meta_R}

#define MODKEY Mod1Mask
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },

/* helper for spawning shell commands in the pre dwm-5.0 fashion */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

/* commands */
static char dmenumon[2] = "0"; /* component of dmenucmd, manipulated in spawn() */
static const char *dmenucmd[] = { "dmenu_run", "-m", dmenumon, "-fn", dmenufont, "-nb", col_gray1, "-nf", col_gray3, "-sb", col_cyan, "-sf", col_gray4, NULL };
static const char *termcmd[]  = { "st", NULL };
static const char scratchpadname[] = "scratchpad";
static const char *scratchpadcmd[] = { "st", "-t", scratchpadname, "-g", "120x34", NULL };
static const char *slimlockcmd[]  = { "slimlock", NULL };

#include "movestack.c"
static Key keys[] = {
	/* modifier                     key        function        argument */
	{ MODKEY|ShiftMask,             XK_l,      spawn,          {.v = slimlockcmd } },  //调用 slimlockcmd 进行锁屏
	{ MODKEY,                       XK_p,      spawn,          {.v = dmenucmd } },  //打开 dmen 面板（应用菜单）
	{ MODKEY|ControlMask,           XK_space,  spawn,          {.v = dmenucmd } },  //打开 dmen 面板（应用菜单）
	{ MODKEY|ShiftMask,             XK_Return, spawn,          {.v = termcmd } },  //新建一个窗格，（开一个终端应用（st））
	{ MODKEY|Mod4Mask,              XK_s,      spawn,          SHCMD("transset-df -a --dec .1") },  //减少当前窗格应用的透明度
	{ MODKEY|Mod4Mask,              XK_d,      spawn,          SHCMD("transset-df -a --inc .1") },  //增加当前窗格应用的透明度
	{ MODKEY|Mod4Mask,              XK_f,      spawn,          SHCMD("transset-df -a .75") },  //恢复当前窗格应用的初始默认的透明度
	{ MODKEY,                       XK_grave,  togglescratch,  {.v = scratchpadcmd } },  //打开一个命令行终端小窗格窗口
	{ MODKEY,                       XK_b,      togglebar,      {0} },  //显示与隐藏状态栏
	{ MODKEY|ShiftMask,             XK_j,      rotatestack,    {.i = +1 } },  //顺时针循环滚动当前窗口的窗格位置	
	{ MODKEY|ShiftMask,             XK_k,      rotatestack,    {.i = -1 } },  //逆时针循环滚动当前窗口的窗格位置
	{ MODKEY|ShiftMask,             XK_d,      movestack,      {.i = +1 } },  //将窗口与下一个窗格互换
	{ MODKEY|ShiftMask,             XK_u,      movestack,      {.i = -1 } },  //将当前窗口与排在上一窗格互换
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } },  //将光标焦点移动到下一个窗格
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } },  //将光标焦点移动到上一个窗格
	{ MODKEY|ShiftMask,             XK_h,      hidewin,        {0} },  // 最小化/隐藏藏 当前窗格
	{ MODKEY|ShiftMask,             XK_r,      restorewin,     {0} },  // 恢复当前窗口下隐藏的窗格,非全部，一次一个恢复
	{ MODKEY,                       XK_i,      incnmaster,     {.i = +1 } },  //插入主窗格的堆栈
	{ MODKEY,                       XK_d,      incnmaster,     {.i = -1 } },  //减少主窗格的堆栈数
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} },  //将当前的窗格宽度减向左扩展或缩小
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} },  //将当前的窗格宽度向右边扩展或缩小
	{ MODKEY|Mod4Mask,              XK_h,      incrgaps,       {.i = +1 } },  //
	{ MODKEY|Mod4Mask,              XK_l,      incrgaps,       {.i = -1 } },  //
	{ MODKEY|Mod4Mask|ShiftMask,    XK_h,      incrogaps,      {.i = +1 } },  //
	{ MODKEY|Mod4Mask|ShiftMask,    XK_l,      incrogaps,      {.i = -1 } },  //
	{ MODKEY|Mod4Mask|ControlMask,  XK_h,      incrigaps,      {.i = +1 } },  //
	{ MODKEY|Mod4Mask|ControlMask,  XK_l,      incrigaps,      {.i = -1 } },  //
	{ MODKEY|Mod4Mask,              XK_0,      togglegaps,     {0} },  //
	{ MODKEY|Mod4Mask|ShiftMask,    XK_0,      defaultgaps,    {0} },  //
	{ MODKEY,                       XK_y,      incrihgaps,     {.i = +1 } },  //
	{ MODKEY,                       XK_o,      incrihgaps,     {.i = -1 } },  //
	{ MODKEY|ControlMask,           XK_y,      incrivgaps,     {.i = +1 } },  //
	{ MODKEY|ControlMask,           XK_o,      incrivgaps,     {.i = -1 } },  //
	{ MODKEY|Mod4Mask,              XK_y,      incrohgaps,     {.i = +1 } },  //
	{ MODKEY|Mod4Mask,              XK_o,      incrohgaps,     {.i = -1 } },  //
	{ MODKEY|ShiftMask,             XK_y,      incrovgaps,     {.i = +1 } },  //
	{ MODKEY|ShiftMask,             XK_o,      incrovgaps,     {.i = -1 } },  //
	{ MODKEY,                       XK_Return, zoom,           {0} },  //将当前窗口与主窗口互换、若是当前是主窗口则将其与上一个窗格互换，并聚焦在主窗格
	{ MODKEY,                       XK_Tab,    view,           {0} },  //查看桌面标签。最后参数可以是NULL（全局查看）或是tags[数字]指定的桌面标签
	{ MODKEY|ShiftMask,             XK_c,      killclient,     {0} },  //关闭当前窗口，强制关闭窗口。最后参数NULL
	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} },  //将当前窗口的模式改为排版,主格居左，副窗格居右，新增窗格水平分割
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} },  //
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} },  //改变当前窗口的模式为浮动
	{ MODKEY,                       XK_u,      setlayout,      {.v = &layouts[3]} },  //将当前窗口的副窗格堆模式改为垂直排列布局方式,主窗堆在上，副窗堆在下，副窗格垂直分割
	{ MODKEY,                       XK_o,      setlayout,      {.v = &layouts[4]} },  //将当前窗口的副窗格堆布局模式改为底部水平排列局方式,主窗堆在上，副窗堆在下，副窗格水平分割
	{ MODKEY|ControlMask,           XK_u,      setlayout,      {.v = &layouts[5]} },  //将当前窗口的副窗格堆模式改为垂直排列布局方式
	{ MODKEY|ControlMask,           XK_o,      setlayout,      {.v = &layouts[6]} },  //将当前窗口的窗格模式改为中心排列布局方式,主窗格在中心占大位，副窗口分列在左右水平分割
	{ MODKEY,                       XK_g,      setlayout,      {.v = &layouts[7]} },  //将当前窗口的窗格模式改为网格模式,一列两行、两行两列、三行三列..
	{ MODKEY|ShiftMask,             XK_f,      fullscreen,     {0} },  //将当前窗口铺满全屏幕（最大化）
	{ MODKEY,                       XK_space,  setlayout,      {0} },  //
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },  //切换是否浮动。最后参数NULL
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },  //查看桌面标签。最后参数可以是NULL（全局查看）或是tags[数字]指定的桌面标签
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },  //切换指定的窗口到达指定的桌面标签。最后参数tags[数字]指定的桌面标签
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } },  //
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } },  //
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } },  //
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } },  //
	TAGKEYS(                        XK_1,                      0)        //切换 1～9 的窗口
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)
	{ MODKEY|ShiftMask,             XK_q,      quit,           {0} }, //退出 dwm
};

```

```c
#include <X11/XF86keysym.h>

/* See LICENSE file for copyright and license details. */

/* appearance */
static const unsigned int borderpx  = 2;        /* border pixel of windows */
static const unsigned int snap      = 32;       /* snap pixel */
static const unsigned int systraypinning = 0;   /* 0: sloppy systray follows selected monitor, >0: pin systray to monitor X */
static const unsigned int systrayspacing = 2;   /* systray spacing */
static const int systraypinningfailfirst = 1;   /* 1: if pinning fails, display systray on the first monitor, False: display systray on the last monitor*/
static const int showsystray        = 1;     /* 0 means no systray */
static const unsigned int gappih    = 8;       /* horiz inner gap between windows */
static const unsigned int gappiv    = 8;       /* vert inner gap between windows */
static const unsigned int gappoh    = 8;       /* horiz outer gap between windows and screen edge */
static const unsigned int gappov    = 8;       /* vert outer gap between windows and screen edge */
static const int smartgaps          = 1;        /* 1 means no outer gap when there is only one window */
static const int showbar            = 1;        /* 0 means no bar */
static const int topbar             = 0;        /* 0 means bottom bar */
static const Bool viewontag         = True;     /* Switch view on tag switch */
static const char *fonts[]          = { "JetBrains Mono:size=12" };
static const char dmenufont[]       = "JetBrains Nerd Font Mono:size=12";
static const char col_gray1[]       = "#222222";
static const char col_gray2[]       = "#444444";
static const char col_gray3[]       = "#bbbbbb";
static const char col_gray4[]       = "#ffffff";
static const char col_cyan[]        = "#37474F";
static const char col_border[]        = "#42A5F5";
static const unsigned int baralpha = 0xd0;
static const unsigned int borderalpha = OPAQUE;
static const char *colors[][3]      = {
	/*               fg         bg         border   */
	[SchemeNorm] = { col_gray3, col_gray1, col_gray2 },
	[SchemeSel]  = { col_gray4, col_cyan,  col_border  },
	[SchemeHid]  = { col_cyan,  col_gray1, col_border  },
};
static const unsigned int alphas[][3]      = {
	/*               fg      bg        border     */
	[SchemeNorm] = { OPAQUE, baralpha, borderalpha },
	[SchemeSel]  = { OPAQUE, baralpha, borderalpha },
};

/* tagging */
// static const char *tags[] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };
// static const char *tags[] = { "一", "二", "三", "四", "五", "六", "七", "八", "九" };
// static const char *tags[] = { "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖" };
static const char *tags[] = { "\uf120", "\uf7ae", "\uf121", "\uf04b", "\ue62e", "\uf251", "\ue727", "\uf537", "\uf684" };

static const Rule rules[] = {
	/* xprop(1):
	 *	WM_CLASS(STRING) = instance, class
	 *	WM_NAME(STRING) = title
	 */
	/* class      instance    title       tags mask     isfloating   monitor */
	{ "Gimp",     NULL,       NULL,       0,            1,           -1 },
	{ "Android Emulator", NULL,       NULL,       0,            1,           -1 },
	{ "Emulator", NULL,       NULL,       0,            1,           -1 },
	{ "quemu-system-i386", NULL,       NULL,       0,            1,           -1 },
	{ "Firefox",  NULL,       NULL,       1 << 8,       0,           -1 },
};

/* layout(s) */
static const float mfact     = 0.5; /* factor of master area size [0.05..0.95] */
static const int nmaster     = 1;    /* number of clients in master area */
static const int resizehints = 1;    /* 1 means respect size hints in tiled resizals */

static const Layout layouts[] = {
	/* symbol     arrange function */
	{ "Tile",      tile },    /* first entry is default */
	{ "><>",      NULL },    /* no layout function means floating behavior */
	{ "[M]",      monocle },
    
	{ "[]{",      tile },    /* first entry is default */
	{ ">v<",      NULL },    /* no layout function means floating behavior */
	{ "TTT",      bstack },
	{ "===",      bstackhoriz },
	{ ":M:",      centeredmaster },
	{ "|M|",      centeredfloatingmaster },
	{ "HHH",      grid },
};

/* key definitions */
#define MODKEY Mod4Mask
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },

/* helper for spawning shell commands in the pre dwm-5.0 fashion */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

/* commands */
static char dmenumon[2] = "0"; /* component of dmenucmd, manipulated in spawn() */
static const char *dmenucmd[] = {"dmenu_run", "-m", dmenumon, "-fn", dmenufont, "-nb", col_gray1, "-nf", col_gray3, "-sb", col_cyan, "-sf", col_gray4, NULL};
static const char *termcmd[] = {"st", NULL};
static const char *xtermcmd[] = {"xterm", NULL};
static const char *typoracmd[] = {"typora", NULL};
static const char *browsercmd[] = {"google-chrome-stable", NULL};
static const char *trayercmd[] = {"/home/jack/scripts/trayer-toggle.sh", NULL};
static const char *upvol[] = {"/home/jack/scripts/vol-up.sh", NULL};
static const char *downvol[] = {"/home/jack/scripts/vol-down.sh", NULL};
static const char *mutevol[] = {"/home/jack/scripts/vol-toggle.sh", NULL};
//static const char *wpcmd[] = {"/home/jack/scripts/wp-change.sh", NULL};
static const char *wpcmd[] = {"feh", "--recursive", "--randomize", "--bg-fill", "~/图片/wallpapers/ghibili", NULL};      
static const char *sktogglecmd[] = {"/home/jack/scripts/sck-tog.sh", NULL};
static const char scratchpadname[] = "scratchpad";
static const char *scratchpadcmd[] = {"st", "-t", scratchpadname, "-g", "80x24", NULL};
static const char *setcolemakcmd[] = {"/home/jack/scripts/setxmodmap-colemak.sh", NULL};
static const char *setqwertycmd[] = {"/home/jack/scripts/setxmodmap-qwerty.sh", NULL};
//static const char *suspendcmd[] = {"/home/jack/scripts/suspend.sh", NULL};
static const char *suspendcmd[] = {"systemctl","suspend", NULL};
static const char *screenshotcmd[] = {"flameshot", "gui", NULL};
//以下是增加的
static const char *volup[] = {"amixer", "-qM", "set", "Master", "2%+", "umute", NULL};
static const char *voldown[] = {"amixer", "-qM", "set", "Master", "2%-", "umute", NULL}; //#定义系统音量大小调节的快捷键功能
static const char *mute[] = {"amixer", "-qM", "set", "Master", "toggle", NULL};			 //#定义开 / 关静音的快捷键功能
static const char *lightup[] = {"xbacklight", "-inc", "2", NULL};
static const char *lightdown[] = {"xbacklight", "-dec", "2", NULL};					  //#定义屏幕亮度调节的快捷键功能
static const char *chromium[] = {"google-chrome-stable", "--disk-cache-dir=/tmp/google-chrome-stable", NULL}; //#定义chrome浏览器的快捷键功能
static const char *dolphin[] = {"dolphin", NULL};	 	  //#定义dolphin文件管理器的快捷键功能

static Key keys[] = {
	/* modifier            key                      function        argument */
	{ MODKEY,              XK_s,                    spawn,          {.v = dmenucmd } }, // win+s打开 dmen 面板（应用菜单）呼出dmenu应用程序启动器
	{ 0,                   XK_Print,                spawn,          {.v = screenshotcmd } },  //PrtSc 截图
	{ MODKEY,              XK_f,                    spawn,          {.v = screenshotcmd } },  //win+f  截图
	{ MODKEY,              XK_Return,               spawn,          {.v = termcmd } },// win+回车新建一个窗格，（开一个终端应用（st））
	{ MODKEY,              XK_grave,                togglescratch,  {.v = scratchpadcmd } },//win+` 打开一个命令行终端小窗格窗口
	{ MODKEY,              XK_x,               	spawn,          {.v = xtermcmd } },// win+x 新建一个窗格，（开一个终端应用（xterm））
	{ MODKEY,              XK_t,               	spawn,          {.v = typoracmd } },// win+t 新建一个窗格，（开一个终端应用（typora））
	{ MODKEY,              XK_c,                    spawn,          {.v = browsercmd } },// win+c 呼出chromium浏览器
 	{ MODKEY|ShiftMask,    XK_t,                    spawn,          {.v = trayercmd } },// win+shift+t 呼出系统托盘
	{ MODKEY|ShiftMask,    XK_s,                    spawn,          {.v = sktogglecmd } },//调出screenkey
	{ 0,                   XF86XK_AudioLowerVolume, spawn,          {.v = downvol } },//降低音量
	{ 0,                   XF86XK_AudioMute,        spawn,          {.v = mutevol } },  //静音
	{ 0,                   XF86XK_AudioRaiseVolume, spawn,          {.v = upvol   } },//增加音量
	{ MODKEY,              XK_bracketleft,          spawn,          {.v = downvol } },// win+[ 降低音量
	{ MODKEY,              XK_backslash,            spawn,          {.v = mutevol } },// win+\ 静音
	{ MODKEY,              XK_bracketright,         spawn,          {.v = upvol   } },// win+] 增加音量
	{ MODKEY|ControlMask,  XK_b,              	spawn,          {.v = wpcmd } }, //win+b换壁纸
	{ MODKEY|ShiftMask|ControlMask,    XK_q,        spawn,          {.v = setqwertycmd } },//win+shift+ctrl+q 键盘模式为qwerty
	{ MODKEY|ShiftMask|ControlMask,    XK_c,        spawn,          {.v = setcolemakcmd } },//win+shift+ctrl+c 键盘模式为colemal
	{ MODKEY|ControlMask,  XK_s,                    spawn,          {.v = suspendcmd } },  // win+ctrl+s休眠
	{ MODKEY|ShiftMask,    XK_j,                    rotatestack,    {.i = +1 } }, //顺时针循环滚动当前窗口的窗格位置	
	{ MODKEY|ShiftMask,    XK_k,                    rotatestack,    {.i = -1 } },//逆时针循环滚动当前窗口的窗格位置
    	{ MODKEY,              XK_Up,     		focusstack,     {.i = +1 } },	//将光标焦点移动到下一个窗格
    	{ MODKEY,              XK_Down,   		focusstack,     {.i = -1 } },   //将光标焦点移动到上一个窗格 
	{ MODKEY,    	       XK_j,                    incnmaster,     {.i = +1 } },//插入主窗格的堆栈，窗口竖向排列
	{ MODKEY,    	       XK_k,                    incnmaster,     {.i = -1 } },//减少主窗格的堆栈数，窗口横向排列
    	{ MODKEY,              XK_Left,   		setmfact,       {.f = -0.05} },//将当前的窗格宽度减向左扩展或缩小
    	{ MODKEY,              XK_Right,  		setmfact,       {.f = +0.05} },   // win+左/右方向键，调整程序窗口的大小
	{ MODKEY,              XK_m,                    hidewin,        {0} }, // win+m 最小化/隐藏藏 当前窗格
	{ MODKEY|ShiftMask,    XK_m,                    restorewin,     {0} }, // win+shift+m 恢复当前窗口下隐藏的窗格,非全部，一次一个恢复
	{ MODKEY,              XK_o,                    hideotherwins,  {0}},  // win+ o 最小化/隐藏藏除当前外的其他窗格
	{ MODKEY|ShiftMask,    XK_o,                    restoreotherwins, {0}},// win+shift+o 恢复当前窗口下隐藏除当前外的其他窗格
	{ MODKEY|ShiftMask,    XK_Return,               zoom,           {0} }, //win+shift+回车，窗口位置互换， 将当前窗口与主窗口互换，若是当前是主窗口则将其与上一个窗格互换，并聚焦在主窗格
	{ MODKEY,              XK_Tab,                  view,           {0} }, //查看桌面标签。最后参数可以是NULL（全局查看）或是tags[数字]指定的桌面标签
    	{ MODKEY,              XK_0,                    view,           {.ui = ~0 } },
	{ MODKEY|Mod1Mask,     XK_0,      		setlayout,     {.v = &layouts[0]} },
	{ MODKEY|Mod1Mask,     XK_1,      		setlayout,     {.v = &layouts[1]} },
	{ MODKEY|Mod1Mask,     XK_2,      		setlayout,     {.v = &layouts[2]} },  //将当前窗口的模式改为排版,主格居左，副窗格居右，新增窗格水平分割
	{ MODKEY|Mod1Mask,     XK_3,      		setlayout,     {.v = &layouts[3]} },  //
	{ MODKEY|Mod1Mask,     XK_4,      		setlayout,     {.v = &layouts[4]} },  //改变当前窗口的模式为浮动
	{ MODKEY|Mod1Mask,     XK_5,      		setlayout,     {.v = &layouts[5]} },  //将当前窗口的副窗格堆模式改为垂直排列布局方式,主窗堆在上，副窗堆在下，副窗格垂直分割
	{ MODKEY|Mod1Mask,     XK_6,      		setlayout,     {.v = &layouts[6]} },  //将当前窗口的副窗格堆布局模式改为底部水平排列局方式,主窗堆在上，副窗堆在下，副窗格水平分割
	{ MODKEY|Mod1Mask,     XK_7,      		setlayout,      {.v = &layouts[7]} },  //将当前窗口的副窗格堆模式改为垂直排列布局方式
	{ MODKEY|Mod1Mask,     XK_8,      		setlayout,      {.v = &layouts[8]} },  //将当前窗口的窗格模式改为中心排列布局方式,主窗格在中心占大位，副窗口分列在左右水平分割
	{ MODKEY,              XK_space,  		setlayout,      {0} },   //窗口模式切换,Alt + 空格,Alt + shift + 空格
	{ MODKEY|ShiftMask,    XK_space,  		togglefloating, {0} },//切换是否浮动。最后参数NULL
	{ MODKEY,              XK_9,     		setlayout,      {.v = &layouts[9]} },  //将当前窗口的窗格模式改为网格模式,一列两行、两行两列、三行三列..
	{ MODKEY|ShiftMask,    XK_f,                    fullscreen,     {0} },  // win+shift+f全屏
	{ MODKEY|ShiftMask,    XK_0,                    tag,            {.ui = ~0 } },//切换指定的窗口到达指定的桌面标签。最后参数tags[数字]指定的桌面标签
	{ MODKEY,              XK_comma,                focusmon,       {.i = -1 } },  //win+,  在主副屏之间移动焦点# 移动焦点至左边屏幕，
	{ MODKEY,              XK_period,               focusmon,       {.i = +1 } },  //win+.  # 移动焦点至右边屏幕
	{ MODKEY|ShiftMask,    XK_comma,                tagmon,         {.i = -1 } },	//win+shift+,  在主副屏之间移动窗口 # 移动窗口至左边屏幕
	{ MODKEY|ShiftMask,    XK_period,               tagmon,         {.i = +1 } },	//win+shift+. # 移动窗口至右边屏幕
	{ MODKEY,              XK_h,                    viewtoleft,     {0} },
	{ MODKEY,              XK_l,                    viewtoright,    {0} },
	{ MODKEY|ShiftMask,    XK_h,                    tagtoleft,      {0} },
	{ MODKEY|ShiftMask,    XK_l,                    tagtoright,     {0} },
    	//{ MODKEY|ShiftMask,    XK_1, 			tag, 		tags[0] },     //Mod + shift + num,移动窗口至某标签页
    	//{ MODKEY|ShiftMask,    XK_2, 			tag, 		tags[1] },
    	//{ MODKEY|ShiftMask,    XK_3,			tag, 		tags[2] },
    	//{ MODKEY|ShiftMask,    XK_4, 			tag, 		tags[3] },
    	//{ MODKEY|ShiftMask,    XK_5, 			tag, 		tags[4] },
	TAGKEYS(               XK_1,                      0)     // win+1/2/3/4/5/6/7/8/9，切换到不同的dwm顶部菜单栏的标签里,切换标签页
	TAGKEYS(               XK_2,                      1)
	TAGKEYS(               XK_3,                      2)
	TAGKEYS(               XK_4,                      3)
	TAGKEYS(               XK_5,                      4)
	TAGKEYS(               XK_6,                      5)
	TAGKEYS(               XK_7,                      6)
	TAGKEYS(               XK_8,                      7)
	TAGKEYS(               XK_9,                      8)
	{ MODKEY|Mod1Mask,     XK_Down,      		spawn,          SHCMD("transset-df -a --dec .1") },  //减少当前窗格应用的透明度
	{ MODKEY|Mod1Mask,     XK_Up,        		spawn,          SHCMD("transset-df -a --inc .1") },  //增加当前窗格应用的透明度
	{ MODKEY|Mod1Mask,     XK_Home,      		spawn,          SHCMD("transset-df -a .75") },  //恢复当前窗格应用的初始默认的透明度
    	{ MODKEY|ShiftMask,    XK_q,          		killclient,     {0} },//关闭当前窗口，强制关闭窗口。最后参数NULL
	{ MODKEY|ControlMask,  XK_q,    		quit,           {0} }, 	//Ctrl+Alt+del，关闭并退出整个dwm桌面，且强制关闭所有当前运行于dwm下的程序
	//以下是增加的
    	{ MODKEY|ShiftMask,   XK_Up,     		spawn,          {.v = lightup} },
    	{ MODKEY|ShiftMask,   XK_Down,   		spawn,          {.v = lightdown} },  // Shift+win+上/下方向键，调整屏幕亮度
    	{ MODKEY|ShiftMask,   XK_Right,  		spawn,          {.v = volup} },
    	{ MODKEY|ShiftMask,   XK_Left,   		spawn,          {.v = voldown} },   // Shift+win+左/右方向键，调整音量大小
    	{ MODKEY|ShiftMask,   XK_m,      		spawn,          {.v = mute} },  	   // Shift+win+m，开启/关闭静音
    	{ MODKEY,             XK_d,      		spawn,          {.v = dolphin } },   // win+d，呼出dolphin文件管理器
    	{ MODKEY,             XK_g,      		spawn,          {.v = chromium } },   // win+j，呼出chromium浏览器

};

/* button definitions */
/* click can be ClkTagBar, ClkLtSymbol, ClkStatusText, ClkWinTitle, ClkClientWin, or ClkRootWin */
static Button buttons[] = {
	/* click                event mask      button          function        argument */
	{ ClkLtSymbol,          0,              Button1,        setlayout,      {0} },
	{ ClkLtSymbol,          0,              Button3,        setlayout,      {.v = &layouts[2]} },
	{ ClkWinTitle,          0,              Button1,        togglewin,      {0} },
	{ ClkWinTitle,          0,              Button2,        zoom,           {0} },
	{ ClkStatusText,        0,              Button2,        spawn,          {.v = termcmd } },
	{ ClkClientWin,         MODKEY,         Button1,        movemouse,      {0} },
	{ ClkClientWin,         MODKEY,         Button2,        togglefloating, {0} },
	{ ClkClientWin,         MODKEY,         Button3,        resizemouse,    {0} },
	{ ClkTagBar,            0,              Button1,        view,           {0} },
	{ ClkTagBar,            0,              Button3,        toggleview,     {0} },
	{ ClkTagBar,            MODKEY,         Button1,        tag,            {0} },
	{ ClkTagBar,            MODKEY,         Button3,        toggletag,      {0} },
};



```



再来说说快捷键，和 i3wm 相同，dwm 的各种快捷键的起手操作都是 “MOD”，这个 MOD 键可以自己定义，默认是 MOD4, 即 Windows 键，此处笔者选择改为了 MOD1, 即 Alt 键。



#  vim/Neovim配置

```bash
# junjie @ Ubuntu in ~/.config/nvim [日期: 周五 4月 02日, 时间:15:10:59]
$ ll
总用量 456K
drwxr-xr-x 1 junjie junjie 4.0K 4月   1 15:39 ./
drwx------ 1 junjie junjie 4.0K 4月   2 10:15 ../
drwxr-x--- 1 junjie junjie 4.0K 4月   2 14:46 autoload/    # plug.vim在这里面
-rw-r--r-- 1 junjie junjie 162K 4月   1 15:39 init.vim    # nvim的配置文件

#################################################################################################
#  vim-plug
" :PlugStatus  检查状态：
" :PlugInstall 输入下面的命令，然后按回车键安装之前在配置文件中声明的插件。
" :PlugUpdate  要更新插件，请运行：
" :PlugClean  删除一个插件删除或注释掉你以前在你的 vim 配置文件中添加的 plug 命令
" :PlugUpgrade   要升级 vim-plug 本身，



# bundle
" :PluginInstall:安装插件
" :PluginClean:移除不要的插件
" :PluginUpdate:更新插件
" :PluginList:列出所有安装的插件
" :PluginSearch:查找插件
################################vim用户, vim-plug安装插件#######################################################
# 如果用的是vim-plug，则安装vim-plug
$ curl -fLo ~/.vim/autoload/plug.vim  --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
# 对于vim配置文件，如果是通过vim-plug安装插件，则在.vimrc中如下：
if empty(glob('~/.vim/autoload/plug.vim'))
	silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
				\ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
	autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif
call plug#begin('~/.vim/plugged')
Plug 'itchyny/lightline.vim'
call plug#end()

#也就是~/.vim/autoload/和~/.vim/plugged/两个目录是vim用户vim-plug安装插件时所需的
# junjie @ Ubuntu in ~/.vim [日期: 周五 4月 02日, 时间:15:57:23]
$ ll
总用量 0
drwxr-x--- 1 junjie junjie 4.0K 6月  20  2020 ./
drwxr-xr-x 1 junjie junjie 4.0K 4月   2 14:59 ../
drwxr-x--- 1 junjie junjie 4.0K 6月  19  2020 autoload/
drwxrwxrwx 1 junjie junjie 4.0K 4月   2 15:01 plugged/
################################vim用户, bundle安装插件#######################################################
# 安装bundle
#vim用户
git clone https://github.com/VundleVim/Vundle.vim.git  ∼/.vim/bundle/Vundle.vim

# 对于vim配置文件，如果是通过vundle安装插件，则在.vimrc中如下：
" 你在此设置运行时路径 
set rtp+=~/.vim/bundle/Vundle.vim  
" vundle初始化 
call vundle#begin()  
" 这应该始终是第一个 
Plugin 'gmarik/Vundle.vim'
"每个插件都应该在这一行之前  
call vundle#end()  
#也就是~/.vim/bundle一个目录是vim用户bundle安装插件时所需的
# junjie @ Ubuntu in ~/.vim [日期: 周五 4月 02日, 时间:15:57:23]
$ ll
总用量 0
drwxr-x--- 1 junjie junjie 4.0K 6月  20  2020 ./
drwxr-xr-x 1 junjie junjie 4.0K 4月   2 14:59 ../
drwxrwxrwx 1 junjie junjie 4.0K 4月   2 15:01 bundle/
################################  Neovim用户, vim-plug安装插件#######################################################
#Neovim 用户可以使用以下命令安装 Vim-plug：
$ curl -fLo ~/.config/nvim/autoload/plug.vim  --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

#在~/.config/nvim/init.vim文件中增加如下：
if empty(glob('~/.config/nvim/autoload/plug.vim'))
	silent !curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs
				\ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
	autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif
call plug#begin('~/.config/nvim/plugged')
Plug 'lambdalisue/suda.vim' " do stuff like :sudowrite
call plug#end()

# junjie @ Ubuntu in ~/.config/nvim [日期: 周五 4月 02日, 时间:15:27:23]
$ ll
总用量 256K
drwxr-xr-x 1 junjie junjie 4.0K 4月   2 15:13 ./
drwx------ 1 junjie junjie 4.0K 4月   2 10:15 ../
drwxr-x--- 1 junjie junjie 4.0K 4月   2 14:46 autoload/
-rw-r--r-- 1 junjie junjie 162K 4月   1 15:39 init.vim
drwxrwxrwx 1 junjie junjie 4.0K 4月   2 15:01 plugged/


########################################Neovim用户, bundle安装插件#########################################################
# Neovim 用户可以使用以下命令安装:sudo apt-get install neovim
$ git clone https://github.com/VundleVim/Vundle.vim.git  ∼/.nvim/bundle/Vundle.vim

# 创建 neovim 的配置文件: 
$ nvim  ~/.nvimrc

set rtp+=~/.nvim/bundle/Vundle.vim
call vundle#begin()
Plugin 'gmarik/Vundle.vim'
Plugin 'mattn/emmet-vim'     “emmet的插件
Plugin 'scrooloose/nerdtree'  ”nerdtree插件
Plugin 'ervandew/supertab'   “superTab插件
call vundle#end()

#   启动 neovim
# :BundleInstall
#######################################################

```

> neovim + vim-plug

为了使用coc.nvim插件先安装以下软件：

1. 安装python3

   nvim 中使用 **:checkhealth**

   ```bash
   pip3 install --user --upgrade pynvim
   ```

2. 安装yarn:

```bash
$: curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
$: echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
$: sudo apt update
$: sudo apt install yarn
```



3. 安装npm

+ sudo apt install npm

4. 安装ccls

```bash
$: git clone --depth=1 --recursive https://github.com/MaskRay/ccls
$: cd ccls
$: cmake -H. -BRelease -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_PREFIX_PATH=/path/to/clang+llvm
$: sudo cmake --build Release --target install
```



5. [去网站](https://github.com/junjiecjj/nvim)下载配置文件至~/.config/nvim/init.vim

6. 颜色：

```bash
$: cd .config/nvim
$: mkdir colors
$: cd colors
$: sudo cp /usr/share/vim/vim81/colors/*.vim  .
```





# Linux小技巧

## [Linux性能分析](https://mp.weixin.qq.com/s?__biz=MzU5NDg5MzM5NQ==&mid=2247488166&idx=1&sn=c8d47fd8deb89bc05ca56991341225f2&chksm=fe7b1d9ac90c948c56c44f03bcf7802e62c4e8f9fd2a2c3b74b1bd0a229c2e422f5f052dcbd8&mpshare=1&scene=1&srcid=0327IfGLSkVkySNHS4U61MID&sharer_sharetime=1616804779201&sharer_shareid=0d5c82ce3c8b7c8f30cc9a686416d4a8#rd)



## 终端快捷键

> 移动光标

+ Ctrl – a ：移到行首
+ Ctrl – e ：移到行尾
+ Ctrl – b ：往回(左)移动一个字符
+ Ctrl – f ：往后(右)移动一个字符
+ Alt – b ：往回(左)移动一个单词
+ Alt – f ：往后(右)移动一个单词

> 编辑、删除

+ Ctrl – h ：删除光标左方位置的字符

- Ctrl – d ：删除光标右方位置的字符（注意：当前命令行没有任何字符时，会注销系统或结束终端）
- Ctrl – w ：由光标位置开始，往左删除单词，一个一个单词删除，往行首删；
- Alt – d ：由光标位置开始，往右删除单词，一个一个单词删除，往行尾删
- Ctrl – k ：由光标所在位置开始，删除右方所有的字符，直到该行结束。
- Ctrl – u ：由光标所在位置开始，删除左方所有的字符，直到该行开始。

> 查找

+ Ctrl – p ：显示当前命令的上一条历史命令
+ Ctrl – n ：显示当前命令的下一条历史命令
+ Ctrl – r ：搜索历史命令，随着输入会显示历史命令中的一条匹配命令，Enter键执行匹配命令；ESC键在命令行显示而不执行匹配命令。

- Ctrl – g ：从历史搜索模式（Ctrl – r）退出。

## vim快捷键

```vim

" "########################################################
" "vim 原本的快捷键
" "########################################################

" "------------------- 光标移动命令----------------------------
" 单个字符移动：
" h 或退格：光标左移一个字符；
" l 或空格：光标右移一个字符；
" j : 光标下移一行；
" k : 光标上移一行；
" xh:  向左移动x个字符距离

" 单词移动：
" w 跳到下一个字首，按标点或单词分割
" W 跳到下一个字首，长跳，如end-of-line被认为是一个字
" e 跳到下一个字尾
" E 跳到下一个字尾，长跳
" b 跳到上一个字
" B 跳到上一个字，长跳
" ge:  将光标移动到上一个单词的词末
" 2w:  指定移动的次数

" 行移动：
" $:  将光标移动到当前行的行尾
" 0:  将光标移动到当前行的行首
" ^:  将光标移动到当前行的第一个非空字符（行首和当前行非空字符不是一个位置）
" 2|:  移到当前行的第2列
" fc : 把光标移到同一行的下一个 c 字符处
" Fc : 把光标移到同一行的上一个 c 字符处
" tc : 把光标移到同一行的下一个 c 字符前
" Tc : 把光标移到同一行的上一个 c 字符后
" 3fx: 将光标移动到当前行的第3个字符x上
" tx:   将光标移动到目标字符x的前一个字符上
" fx和tx可以通过;和,进行重复移动，一个是正向重复，一个是反向重复
" ; 重复上一个f命令，而不用重复的输入fx
" * 查找光标所在处的单词，向下查找
" # 查找光标所在处的单词，向上查找
" %:  用于符号间的移动，它会在一对()、[]、{}之间跳跃

" 文本块移动：
" (：  移到当前句子的开头
" ):  移到下一个句子的开头
" {:  移到当前一段的开头
" }:  移到下一段的开头
" [[:  移到当前这一节的开头
" ]]:  移到下一节的开头

" 在屏幕中移动
" nG:  跳转到指定的第n行，G移动到文件按末尾，``（2次单引号)返回到跳转前的位置
" gg:  移动到文件开头
" G 调至文尾
" 5gg/5G 调至第5行
" gd 跳至当前光标所在的变量的声明处
" 光标在当前行的基础上再跳 20 行：20+enter 键
" x%:  移动到文件中间，就使用50%
" H : 把光标移到屏幕最顶端一行。
" M : 把光标移到屏幕中间一行。
" L : 把光标移到屏幕最底端一行。
" ctrl+G:  查看当前的位置状态


" "--------------------------滚屏与跳转---------------------------------
" 半屏滚动:  ctrl+u/ctrl+d
" 全屏滚动:  ctrl+f/ctrl+b
" ctrl-f 下翻一页,f = forward
" ctrl-b 上翻一页,b = backward
" ctrl-u 上翻半页,u = up
" ctrl-d 下翻半页,d = down
" 定位光标的位置
" Ctrl-e    向下滚动一行；
" Ctrl-y    向上滚动一行；
" zz 让光标所在的行居屏幕中央
" zt 让光标所在的行居屏幕最上一行 t=top
" zb 让光标所在的行居屏幕最下一行 b=bottom

" 设置跳转标记
" mx,my,mz设置三个位置
" `x,`y,`z跳转到设置


" "---------------------文本插入操作---------------------------
" i:  在当前光标的前面插入字符
" a:  在当前光标的后面进入插入模式，追加字符
" o:  在当前光标的下一行行首插入字符
" I:  在一行的开头添加文本
" A:  在一行的结尾处进入插入模式，添加文本
" O:  在光标当前行的上一行插入文本
" s:  删除当前光标处的字符并进入到插入模式
" S:  删除光标所在处的行，并进入到插入模式
" u:  撤销修改


""-------------------文本删除操作--------------------------
" 字符删除
" x:  删除当前光标所在处的字符
" X:  删除当前光标左边的字符

" 单词删除
" dw:  删除一个单词(从光标处到空格)
" daw:  无论光标在什么位置，删除光标所在的整个单词(包括空白字符)
" diw:  删除整个单词文本，但是保留空格字符不删除
" d2w:  删除从当前光标开始处的2个单词
" d$:  删除从光标到一行末尾的整个文本
" d0:  删除从光标到一行开头的所有单词
" dl:  删除当前光标处的字符=x
" dh:  删除当前光标左边的字符=X

" 行删除
" dd:  删除当前光标处的一整行=D
" 5dd:  删除从光标开始处的5行代码
" dgg:  删除从光标到文本开头
" dG:  删除从光标到文本结尾

" 行合并
" J:  删除一个分行符，将当前行与下一行合并


"------------------------ 文本复制、剪切与粘贴---------------------------
" y:  复制，p:粘贴
" yw:  复制一个单词
" y2w:  复制2个单词
" y$:  复制从当前光标到行结尾的所有单词
" y0:  复制从当前光标到行首的所有单词
" yy:  复制一整行
" 2yy:  复制从当前光标所在行开始的2行

" 复制文本块
"     1.首先进入visual模式：v
"     2.移动光标选择文本
"     3.复制与粘贴的操作

"--------------------- 文本的修改与替换-------------------------------
" cw:  删除从光标处到单词结尾的文本并进入到插入模式
" cb:  删除从光标处到单词开头的文本并进入到插入模式
" cc 删除当前行并进入编辑模式
" c$ 擦除从当前位置至行末的内容，并进入编辑模式
" s 删除当前字符并进入编辑模式
" S 删除光标所在行并进入编辑模式
" ~： 修改光标下字符的大小写
" r:  替换当前光标下的字符
" R:  进入到替换模式
" xp:  交换光标所在字符和下一个字符
" >> 将当前行右移一个单位
" << 将当前行左移一个单位(一个tab符)
" == 自动缩进当前行

"------------------------- 文本的查找与替换-------------------------
" /string   正向查找
" ?string   反向查找
"  n 下一个匹配(如果是/搜索，则是向下的下一个，?搜索则是向上的下一个)
"  N 上一个匹配(同上)
"  /\CWord ： 区分大小写的查找
"  /\cword ： 不区分大小写的查找

" 设置高亮显示
"     :set hls
"     *按键将当前光标处的单词高亮显示，使用n浏览下一个查找高亮的结果
" :s/old/new   将当前行的第一个字符串old替换为new
" :s/old/new/g   将当前行的所有字符串old替换为new
" :90s/old/new/g  将指定行的所有字符串old替换为new
" :90,93s/old/new/g  将指定范围的行的所有字符串old替换为new
"  :%s/old/new/g 搜索整个文件，将所有的old替换为new
"  :%s/old/new/gc 搜索整个文件，将所有的old替换为new，每次都要你确认是否替换
" :%s/^struct/int/g   将所有以struct开头的字符串替换为int


"---------------------------- 撤销修改、重做与保存---------------------------------
" u:  撤销上一步的操作。
" Ctrl+r:  将原来的插销重做一遍
" U：  恢复一整行原来的面貌（文件打开时的文本状态）
" q:  若文件没有修改，直接退出
" q!:  文件已经被修改，放弃修改退出
" wq:  文件已经被修改，保存修改并退出
" e!:  放弃修改，重新回到文件打开时的状态


"---------------------------- 编辑多个文件----------------------------------------
" 文件和缓冲区的区别
" 文件是保存在磁盘上的，而打开的文件的文件是在内存中，在内存中有一个缓冲区，用来存放打开的文件。vim每次打开文件时都会创建一个缓冲区，vim支持打开多个文件
" 缓冲区
" :buffers或:ls或:files 显示缓冲区列表。
" ctrl+^：在最近两个缓冲区间切换。
" :bn -- 下一个缓冲区。
" :bp -- 上一个缓冲区。
" :bl -- 最后一个缓冲区。
" :b[n]或:[n]b -- 切换到第n个缓冲区。
" :nbw(ipeout) -- 彻底删除第n个缓冲区。
" :nbd(elete) -- 删除第n个缓冲区，并未真正删除，还在unlisted列表中。
" :ba[ll] -- 把所有的缓冲区在当前页中打开，每个缓冲区占一个窗口。
" :bfirst/blast  分别调到缓冲区列表的开头和结尾
" :write   将缓冲区的修改保存到磁盘上


" 文档操作
" :e file --关闭当前编辑的文件，并开启新的文件。 如果对当前文件的修改未保存，vi会警告。
" :e! file --放弃对当前文件的修改，编辑新的文件。
" :e+file -- 开始新的文件，并从文件尾开始编辑。
" :e+n file -- 开始新的文件，并从第n行开始编辑。
" :enew --编译一个未命名的新文档。(CTRL-W n)
" :e -- 重新加载当前文档。
" :e! -- 重新加载当前文档，并丢弃已做的改动。
" :e#或ctrl+^ -- 回到刚才编辑的文件，很实用。
" :f或ctrl+g -- 显示文档名，是否修改，和光标位置。
" :f filename -- 改变编辑的文件名，这时再保存相当于另存为。
" gf -- 打开以光标所在字符串为文件名的文件。
" :w -- 保存修改。
" :n1,n2w filename -- 选择性保存从某n1行到另n2行的内容。
" :wq -- 保存并退出。
" ZZ -- 保存并退出。
" :x -- 保存并退出。
" :q[uit] ——退出当前窗口。(CTRL-W q或CTRL-W CTRL-Q)
" :saveas newfilename -- 另存为


"---------------------------------标签页与折叠栏-----------------------------------------
" 多标签编辑
" vim -p files: 打开多个文件，每个文件占用一个标签页。
" :tabe, tabnew -- 如果加文件名，就在新的标签中打开这个文件， 否则打开一个空缓冲区。
" ^w gf -- 在新的标签页里打开光标下路径指定的文件。
" :tabn -- 切换到下一个标签。Control + PageDown，也可以。
" :tabp -- 切换到上一个标签。Control + PageUp，也可以。
" [n] gt -- 切换到下一个标签。如果前面加了 n ， 就切换到第n个标签。第一个标签的序号就是1。
" :tab split -- 将当前缓冲区的内容在新页签中打开。
" :tabc[lose] -- 关闭当前的标签页。
" :tabo[nly] -- 关闭其它的标签页。
" :tabs -- 列出所有的标签页和它们包含的窗口。
" :tabm[ove] [N] -- 移动标签页，移动到第N个标签页之后。 如 tabm 0 当前标签页，就会变成第一个标签页。
" 创建一个折叠
"     zf200G:将光标和200行之间的代码折叠起来
" 折叠的打开与关闭
"     za:  打开和关闭折叠
"     zr/zm: 一层一层地打开和关闭折叠
"     zR/zM: 分别打开和关闭所有的折叠
" 折叠键的光标移动
"     zj: 跳转到下一个折叠处
"     zk: 跳转到上一个折叠处
" 删除折叠
"     zd: 删除光标下的折叠
"     zD: 删除光标下的折叠以及嵌套的折叠
"     zE: 删除所有的折叠标签
"     创建的折叠当退出vim之后就失效了。


"----------------------- 多窗口操作---------------------------
" 分割窗口
" :split/vsplit filename
" 窗口间跳转
" ctrl+w hjkl   ,先键入Ctrl+w, 松开后再键入ARROW(h,j,k,l或方向键)
" CTRL-W h        跳转到左边的窗口
" CTRL-W j        跳转到下面的窗口
" CTRL-W k        跳转到上面的窗口
" CTRL-W l        跳转到右边的窗口

" CTRL-W t        跳转到最顶上的窗口
" CTRL-W b        跳转到最底下的窗口

" ctrl+w w        循环跳转

" 移动窗口
"     ctrl+w HJKL
" CTRL-W K    窗口将被移到最上面,如果你用的是垂直分割，CTRL-W K 会使当前窗口移动到上面并扩展到整屏的宽度。
" CTRL-W H        把当前窗口移到最左边
" CTRL-W J        把当前窗口移到最下边
" CTRL-W L        把当前窗口移到最右边


" 调整窗口尺寸
"     ctrl+w +/-  调整窗口的高度
"     ctrl+w </>  调整窗口的宽度
"     ctrl+w = 所有的窗口设置相同的尺寸
"     :resize n将当前窗口尺寸调整为N行
" 关闭窗口
"     :close: 关闭一个窗口
"     :qall: 退出所有窗口
"     :qall!: 放弃修改，退出所有窗口
"     :wqall: 保存并退出所有窗口
"     :wall: 保存所有窗口

"++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
"-----------------------------vim快捷键结束----------------------------------------------
"++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


" 在开发中，可能会遇到一次性给多行代码增加注释的情况。
" 操作步骤如下：
" 在命令模式下，移动到要添加注释代码的第一行，按下 "^" 将光标移动到行首。
" 按下 "Ctrl + v"， 命令模式 --> 可视块模式。
" 使用 "j" 向下连续选中要添加的代码行。
" 输入 "I"， 可视块模式 --> 编辑模式。（注意：必须使用 "I"）
" 输入 "#" 字符，也就是注释的符号。
" 按下 "Esc"， 编辑模式 --> 命令模式。
```








## .xprofile等文件

然后根据个人需求可以修改以下文件：
～/.bashrc: 每次终端时读取并运用里面的设置 
～/.profile：每次启动系统的读取并运用里面的配置 
～/.xinitrc: 每次startx启动X界面时读取并运用里面的设置 
～/.xprofile: 每次使用lightdm等图形登录管理器时读取并运用里面的设置 

在在 `~/.xinitrc` 、 `~/.xprofile` 和 `~/.bashrc 或 ~/.zshrc` 中加入：

```bash
export LC_ALL=”zh_CN.UTF-8”
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```





## [Linux 文件的颜色代码](https://mp.weixin.qq.com/s?__biz=MzA4NzQzMzU4Mg==&mid=2652947251&idx=2&sn=43c7eaa497fc5e3bfeccfd788112f64e&chksm=8bedeb32bc9a62240c11c595926e46337a96b93d0485a89bf80c2c1ae977986dd9245f32b67e&scene=132#wechat_redirect)

在 Linux 中使用颜色代码来区分文件类型，通常情况下目录、链接、文件的颜色将不同。在终端中使用 ls 命令时，会发现一些带有颜色的文件。

`ls`命令使用环境变量`LS_COLORS`来确定文件名的显示颜色。你可以通过调用`LS_COLORS`变量来查看文件类型及其颜色代码的列表。

```bash
[root@localhost ~]# echo $LS_COLORS
rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=01;05;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=01;36:*.au=01;36:*.flac=01;36:*.m4a=01;36:*.mid=01;36:*.midi=01;36:*.mka=01;36:*.mp3=01;36:*.mpc=01;36:*.ogg=01;36:*.ra=01;36:*.wav=01;36:*.oga=01;36:*.opus=01;36:*.spx=01;36:*.xspf=01;36:
```

默认的颜色代码在`/etc/DIR_COLORS`配置文件中。

它为文件使用三种类型的颜色代码：

- 属性代码：代码范围 00-08
- 文字颜色代码：代码范围 30-37,90-97
- 背景颜色代码：代码范围 40-47,100-107

> **文件类型代码列表**

下面是常用文件类型代码的列表：

| Code        | File Types         |
| :---------- | :----------------- |
| di          | 目录               |
| fi          | 文件               |
| ex          | 可执行文件         |
| ln          | 符号链接文件       |
| so          | 套接字             |
| bd          | 块设备             |
| cd          | 字符设备           |
| mi          | 丢失文件           |
| *.extension | 例如：*.mp3,*.jpeg |



> **属性代码列表**

下面表格是属性代码：

```bash
+--------------+--------------------+
|    Code      |      Attributes    |
+--------------+--------------------+
|      00      | None               |
|      01      | Bold               |
|      04      | Underscore         |
|      05      | Blink              |
|      07      | Reverse            |
|      08      | Concealed 隐藏     |
+--------------+--------------------+
```

>**文本颜色代码**

下面表格是字体颜色的代码：

```bash
+--------------+--------------------+     +--------------+--------------------+
|    Code      |      Text Color    |     |    Code      |      Text Color    |
+--------------+--------------------+     +--------------+--------------------+
|      30      | Black              |     |      90      | dark grey          |
|      31      | Red                |     |      91      | light red          |
|      32      | Green              |     |      92      | light green        |
|      33      | Yellow             |     |      93      | yellow             |
|      34      | Blue               |     |      94      | light blue         |
|      35      | Magenta            |     |      95      | light purple       |
|      36      | Cyan               |     |      96      | turquoise          |
|      37      | White              |     |      97      | white              |
+--------------+--------------------+     +--------------+--------------------+
```

> **背景颜色代码**

下面表格是背景颜色的代码：

```bash
+--------------+--------------------+     +--------------+--------------------+
|    Code      | Background Color   |     |    Code      | Background Color   |
+--------------+--------------------+     +--------------+--------------------+
|      40      | Black              |     |     100      | dark grey          |
|      41      | Red                |     |     101      | light red          |
|      42      | Green              |     |     102      | light green        |
|      43      | Yellow             |     |     103      | yellow             |
|      44      | Blue               |     |     104      | light blue         |
|      45      | Magenta            |     |     105      | light purple       |
|      46      | Cyan               |     |     106      | turquoise  宝石绿  |
|      47      | White              |     |     107      | white              |
+--------------+--------------------+     +--------------+--------------------+
```



> 如何在 Linux 中为文件设置自定义颜色

默认的文件夹颜色为 “蓝色”，在这里我们将文件夹配色方案给为黄色 93 和 04 下换线，这种组合代码是`LS_COLORS="di=4;93"`
如果使其生效，可在`~/.bashrc`中添加上面代码。

```bash
[root@localhost ~]# echo "LS_COLORS=\"di=04;93\"" >> ~/.bashrc
[root@localhost ~]# source ~/.bashrc

LS_COLORS="di=04;33:fi=01;37;40:ln=01;36:so=00;36:bd=05;95:cd=05;95:mi=00;90:*.md=01;36:*.docx=01;92:*.doc=01;92:*.pdf=01;92:*.tex=01;92:*.c=01;34:*.cpp=01;34:*.ex=00;91"
```



#  linux命令大全

## [dstat](https://mp.weixin.qq.com/s?__biz=MzAwMzc4MTExOA==&mid=2649798734&idx=1&sn=057c6137a17fcd1650eb2e744a0325c9&chksm=8331e0dfb44669c9c8f6c5a1adfeaccaf8ca4999f5262ba26cdd7ac541dee409bbc2d7f58acd&mpshare=1&scene=1&srcid=0129uc7hno0Q4seLXt2JWRCe&sharer_sharetime=1611916489458&sharer_shareid=0d5c82ce3c8b7c8f30cc9a686416d4a8#rd)

安装方法

Ubuntu/Mint 和 Debin 系统：

本地软件库中有相关安装包，你可以用下面命令安装：

\# sudo apt-get install dstat

RHEL/Centos 和 Fedora 系统:
你可以在 romforge 软件库中添加有相关安装包，参照指导，使用如下命令很简单就能进行安装：

\# yum install dstat

ArchLinux 系统：
相关软件包在社区资源库中，你可以用这个命令来安装：

\# pacman -S dstat

使用方法
dstat 的基本用法就是输入 `dstat` 命令，输出如下：

这是默认输出显示的信息：

CPU 状态：CPU 的使用率。这项报告更有趣的部分是显示了用户，系统和空闲部分，这更好地分析了 CPU 当前的使用状况。如果你看到"wait" 一栏中，CPU 的状态是一个高使用率值，那说明系统存在一些其它问题。当 CPU 的状态处在 "waits" 时，那是因为它正在等待 I/O 设备（例如内存，磁盘或者网络）的响应而且还没有收到。

磁盘统计（dsk）：磁盘的读写操作，这一栏显示磁盘的读、写总数。

网络统计（net）：网络设备发送和接受的数据，这一栏显示的网络收、发数据总数。

分页统计（paging）：系统的分页活动。分页指的是一种内存管理技术用于查找系统场景，一个较大的分页表明系统正在使用大量的交换空间，或者说内存非常分散，大多数情况下你都希望看到 page in（换入）和 page out（换出）的值是 0 0。

系统统计（system）：这一项显示的是中断（int）和上下文切换（csw）。这项统计仅在有比较基线时才有意义。这一栏中较高的统计值常表示大量的进程造成拥塞，需要对 CPU 进行关注。你的服务器一般情况下都会运行运行一些程序，所以这项总是显示一些数值。默认情况下，dstat 每秒都会刷新数据。如果想退出 dstat，你可以按 "CTRL+C" 键。

需要注意的是报告的第一行，通常这里所有的统计都不显示数值的。

这是由于 dstat 会通过上一次的报告来给出一个总结，所以第一次运行时是没有平均值和总值的相关数据。

但是 dstat 可以通过传递 2 个参数运行来控制报告间隔和报告数量。例如，如果你想要 dstat 输出默认监控、报表输出的时间间隔为 3 秒

钟，并且报表中输出 10 个结果，你可以运行如下命令：

\# `dstat 3 10`

在 dstat 命令中有很多参数可选，你可以通过 man dstat 命令查看，大多数常用的参数有这些：
-c：显示 CPU 系统占用，用户占用，空闲，等待，中断，软件中断等信息。

-C：当有多个 CPU 时候，此参数可按需分别显示 cpu 状态，例：-C 0,1 是显示 cpu0 和 cpu1 的信息。

-d：显示磁盘读写数据大小。

-D hda,total：include hda and total。

-n：显示网络状态。

-N eth1,total：有多块网卡时，指定要显示的网卡。

-l：显示系统负载情况。

-m：显示内存使用情况（包括 used，buffer，cache，free 值）。

-g：显示页面使用情况。

-p：显示进程状态。

-s：显示交换分区使用情况。

-S：类似 D/N。

-r：I/O 请求情况。

-y：系统状态。
-t ：将当前时间显示在第一行

--ipc：显示 ipc 消息队列，信号等信息。

--socket：用来显示 tcp udp 端口状态。

-a：此为默认选项，等同于 - cdngy。

-v：等同于 -pmgdsc -D total。
–socket ：显示网络统计数据
–tcp ：显示常用的 TCP 统计
–udp ：显示监听的 UDP 接口及其当前用量的一些动态数据–fs ：显示文件系统统计数据（包括文件总数量和 inodes 值）
–nocolor ：不显示颜色（有时候有用）

--output 文件：此选项也比较有用，可以把状态信息以 csv 的格式重定向到指定的文件中，以便日后查看。例：dstat --output /root/dstat.csv & 此时让程序默默的在后台运行并把结果输出到 /root/dstat.csv 文件中。



当然不止这些用法，dstat 附带了一些插件很大程度地扩展了它的功能。你可以通过查看 /usr/share/dstat 目录来查看它们的一些使用方法，常用的有这些：

-–disk-util ：显示某一时间磁盘的忙碌状况

-–freespace ：显示当前磁盘空间使用率

-–proc-count ：显示正

在运行的程序数量
-–top-bio ：指出块 I/O 最大的进程
-–top-cpu ：图形化显示 CPU 占用最大的进程
-–top-io ：显示正常 I/O 最大的进程
-–top-mem ：显示占用最多内存的进程
举一些例子：
查看全部内存都有谁在占用：

\# `dstat -g -l -m -s --top-mem`


显示一些关于 CPU 资源损耗的数据：

\# `dstat -c -y -l --proc-count --top-cpu`

您可以将多个内部 dstat 插件与外部 dstat 插件一起使用，以查看所有可用插件的列表，请运行以下命令：

$ `dstat --list`

## du、df、fdisk

https://www.runoob.com/linux/linux-filesystem.html
Linux 磁盘管理好坏直接关系到整个系统的性能问题。

Linux 磁盘管理常用三个命令为 df、du 和 fdisk。

df：列出文件系统的整体磁盘使用量
du：检查磁盘空间使用量
fdisk：用于磁盘分区


df
df 命令参数功能：检查文件系统的磁盘空间占用情况。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

语法：

df [-ahikHTm] [目录或文件名]
选项与参数：

-a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
-k ：以 KBytes 的容量显示各文件系统；
-m ：以 MBytes 的容量显示各文件系统；
-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-H ：以 M=1000K 取代 M=1024K 的进位方式；
-T ：显示文件系统类型，连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
-i ：不用硬盘容量，而以 inode 的数量来显示
实例 1
将系统内所有的文件系统列出来！

[root@www ~]# df
Filesystem      1K-blocks      Used Available Use% Mounted on
/dev/hdc2         9920624   3823112   5585444  41% /
/dev/hdc3         4956316    141376   4559108   4% /home
/dev/hdc1          101086     11126     84741  12% /boot
tmpfs              371332         0    371332   0% /dev/shm
在 Linux 底下如果 df 没有加任何选项，那么默认会将系统内所有的 (不含特殊内存内的文件系统与 swap) 都以 1 Kbytes 的容量来列出来！

实例 2
将容量结果以易读的容量格式显示出来

[root@www ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/hdc2             9.5G  3.7G  5.4G  41% /
/dev/hdc3             4.8G  139M  4.4G   4% /home
/dev/hdc1              99M   11M   83M  12% /boot
tmpfs                 363M     0  363M   0% /dev/shm
实例 3
将系统内的所有特殊文件格式及名称都列出来

[root@www ~]# df -aT
Filesystem    Type 1K-blocks    Used Available Use% Mounted on
/dev/hdc2     ext3   9920624 3823112   5585444  41% /
proc          proc         0       0         0   -  /proc
sysfs        sysfs         0       0         0   -  /sys
devpts      devpts         0       0         0   -  /dev/pts
/dev/hdc3     ext3   4956316  141376   4559108   4% /home
/dev/hdc1     ext3    101086   11126     84741  12% /boot
tmpfs        tmpfs    371332       0    371332   0% /dev/shm
none   binfmt_misc         0       0         0   -  /proc/sys/fs/binfmt_misc
sunrpc  rpc_pipefs         0       0         0   -  /var/lib/nfs/rpc_pipefs

实例 4
将 /etc 底下的可用的磁盘容量以易读的容量格式显示

[root@www ~]# df -h /etc
Filesystem            Size  Used Avail Use% Mounted on
/dev/hdc2             9.5G  3.7G  5.4G  41% /


du
Linux du 命令也是查看使用空间的，但是与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看，还是和 df 命令有一些区别的，这里介绍 Linux du 命令。

语法：

du [-ahskm] 文件或目录名称
选项与参数：

-a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
-h ：以人们较易读的容量格式 (G/M) 显示；
-s ：列出总量而已，而不列出每个各别的目录占用容量；
-S ：不包括子目录下的总计，与 -s 有点差别。
-k ：以 KBytes 列出容量显示；
-m ：以 MBytes 列出容量显示；

实例 1
只列出当前目录下的所有文件夹容量（包括隐藏文件夹）:

[root@www ~]# du
8       ./test4     <==每个目录都会列出来
8       ./test2
....中间省略....
12      ./.gconfd   <==包括隐藏文件的目录
220     .           <==这个目录(.)所占用的总量
直接输入 du 没有加任何选项时，则 du 会分析当前所在目录的文件与目录所占用的硬盘空间。

实例 2
将文件的容量也列出来

[root@www ~]# du -a
12      ./install.log.syslog   <==有文件的列表了
8       ./.bash_logout
8       ./test4
8       ./test2
....中间省略....
12      ./.gconfd
220     .
实例 3
检查根目录底下每个目录所占用的容量

[root@www ~]# du -sm /*
7       /bin
6       /boot
.....中间省略....
0       /proc
.....中间省略....
1       /tmp
3859    /usr     <==系统初期最大就是他了啦！
77      /var
通配符 * 来代表每个目录。

与 df 不一样的是，du 这个命令其实会直接到文件系统内去搜寻所有的文件数据。

fdisk
fdisk 是 Linux 的磁盘分区表操作工具。

语法：

fdisk [-l] 装置名称
选项与参数：

-l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。
实例 1
列出所有分区信息

[root@AY120919111755c246621 tmp]# fdisk -l

Disk /dev/xvda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

    Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *           1        2550    20480000   83  Linux
/dev/xvda2            2550        2611      490496   82  Linux swap / Solaris

Disk /dev/xvdb: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x56f40944

    Device Boot      Start         End      Blocks   Id  System
/dev/xvdb2               1        2610    20964793+  83  Linux

实例 2
找出你系统中的根目录所在磁盘，并查阅该硬盘内的相关信息

[root@www ~]# df /            <==注意：重点在找出磁盘文件名而已
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/hdc2              9920624   3823168   5585388  41% /

[root@www ~]# fdisk /dev/hdc  <==仔细看，不要加上数字喔！
The number of cylinders for this disk is set to 5005.
There is nothing wrong with that, but this is larger than 1024,
and could in certain setups cause problems with:
1) software that runs at boot time (e.g., old versions of LILO)
2) booting and partitioning software from other OSs
   (e.g., DOS FDISK, OS/2 FDISK)

Command (m for help):     <==等待你的输入！
输入 m 后，就会看到底下这些命令介绍

Command (m for help): m   <== 输入 m 后，就会看到底下这些命令介绍
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition            <==删除一个partition
   l   list known partition types
   m   print this menu
   n   add a new partition           <==新增一个partition
   o   create a new empty DOS partition table
   p   print the partition table     <==在屏幕上显示分割表
   q   quit without saving changes   <==不储存离开fdisk程序
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit  <==将刚刚的动作写入分割表
   x   extra functionality (experts only)
离开 fdisk 时按下 q，那么所有的动作都不会生效！相反的， 按下 w 就是动作生效的意思。

Command (m for help): p  <== 这里可以输出目前磁盘的状态

Disk /dev/hdc: 41.1 GB, 41174138880 bytes        <==这个磁盘的文件名与容量
255 heads, 63 sectors/track, 5005 cylinders      <==磁头、扇区与磁柱大小
Units = cylinders of 16065 * 512 = 8225280 bytes <==每个磁柱的大小

   Device Boot      Start         End      Blocks   Id  System
/dev/hdc1   *           1          13      104391   83  Linux
/dev/hdc2              14        1288    10241437+  83  Linux
/dev/hdc3            1289        1925     5116702+  83  Linux
/dev/hdc4            1926        5005    24740100    5  Extended
/dev/hdc5            1926        2052     1020096   82  Linux swap / Solaris

### 装置文件名 启动区否 开始磁柱    结束磁柱  1K大小容量 磁盘分区槽内的系统

Command (m for help): q
想要不储存离开吗？按下 q 就对了！不要随便按 w 啊！

使用 p 可以列出目前这颗磁盘的分割表信息，这个信息的上半部在显示整体磁盘的状态。

磁盘格式化
磁盘分割完毕后自然就是要进行文件系统的格式化，格式化的命令非常的简单，使用 mkfs（make filesystem） 命令。

语法：

mkfs [-t 文件系统格式] 装置文件名
选项与参数：

-t ：可以接文件系统格式，例如 ext3, ext2, vfat 等 (系统有支持才会生效)
实例 1
查看 mkfs 支持的文件格式

[root@www ~]# mkfs[tab][tab]
mkfs         mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.msdos   mkfs.vfat
按下两个 [tab]，会发现 mkfs 支持的文件格式如上所示。

实例 2
将分区 /dev/hdc6（可指定你自己的分区） 格式化为 ext3 文件系统：

[root@www ~]# mkfs -t ext3 /dev/hdc6
mke2fs 1.39 (29-May-2006)
Filesystem label=                <==这里指的是分割槽的名称(label)
OS type: Linux
Block size=4096 (log=2)          <==block 的大小配置为 4K 
Fragment size=4096 (log=2)
251392 inodes, 502023 blocks     <==由此配置决定的inode/block数量
25101 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=515899392
16 block groups
32768 blocks per group, 32768 fragments per group
15712 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Writing inode tables: done
Creating journal (8192 blocks): done <==有日志记录
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 34 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
### 这样就创建起来我们所需要的 Ext3 文件系统了！简单明了！

磁盘检验


fsck（file system check）用来检查和维护不一致的文件系统。

若系统掉电或磁盘发生问题，可利用 fsck 命令对文件系统进行检查。

语法：

fsck [-t 文件系统] [-ACay] 装置名称
选项与参数：

-t : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数
-s : 依序一个一个地执行 fsck 的指令来检查
-A : 对 /etc/fstab 中所有列出来的 分区（partition）做检查
-C : 显示完整的检查进度
-d : 打印出 e2fsck 的 debug 结果
-p : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行
-R : 同时有 -A 条件时，省略 / 不检查
-V : 详细显示模式
-a : 如果检查有错则自动修复
-r : 如果检查有错则由使用者回答是否修复
-y : 选项指定检测每个文件是自动输入 yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。
实例 1
查看系统有多少文件系统支持的 fsck 命令：

[root@www ~]# fsck[tab][tab]
fsck         fsck.cramfs  fsck.ext2    fsck.ext3    fsck.msdos   fsck.vfat
实例 2
强制检测 /dev/hdc6 分区:

[root@www ~]# fsck -C -f -t ext3 /dev/hdc6 
fsck 1.39 (29-May-2006)
e2fsck 1.39 (29-May-2006)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
vbird_logical: 11/251968 files (9.1% non-contiguous), 36926/1004046 blocks
如果没有加上 -f 的选项，则由于这个文件系统不曾出现问题，检查的经过非常快速！若加上 -f 强制检查，才会一项一项的显示过程。

磁盘挂载与卸除
Linux 的磁盘挂载使用 mount 命令，卸载使用 umount 命令。

磁盘挂载语法：

mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n]  装置文件名  挂载点
实例 1
用默认的方式，将刚刚创建的 /dev/hdc6 挂载到 /mnt/hdc6 上面！

[root@www ~]# mkdir /mnt/hdc6
[root@www ~]# mount /dev/hdc6 /mnt/hdc6
[root@www ~]# df
Filesystem           1K-blocks      Used Available Use% Mounted on
.....中间省略.....
/dev/hdc6              1976312     42072   1833836   3% /mnt/hdc6
磁盘卸载命令 umount 语法：

umount [-fn] 装置文件名或挂载点
选项与参数：

-f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；
-n ：不升级 /etc/mtab 情况下卸除。
卸载 /dev/hdc6

[root@www ~]# umount /dev/hdc6     



## [curl和wget](https://mp.weixin.qq.com/s/VprxpaxQihFVHlJB3a9xkQ)

Linux 命令行比 GUI 提供了更多的灵活性和控制力。与 GUI 相比，许多人更喜欢使用命令行，因为它比 GUI 更加易于使用和快捷。使用命令行可以更轻松地使用一行自动执行任务。另外，它比 GUI 使用更少的资源。

下载文件是一项日常任务，通常每天执行，其中包括 ZIP，TAR，ISO，PNG 等文件类型。您可以使用命令行终端简单快速地执行此任务。只需要使用键盘即可。因此，今天，我将向您展示如何在 Linux 中使用命令行下载文件。通常有两种已知的方法可以做到这一点，即使用 wget 和 curl 工具。对于本文，我将使用 Ubuntu 20.04 LTS 来描述该过程。但是相同的命令也可以在其他 Linux 发行版（如 Debian，Gentoo 和 CentOS）上运行。

### **使用 Curl 下载文件**

Curl 可用于通过多种协议传输数据。它使用 Curl 支持许多协议，包括 HTTP ， HTTPS ， FTP ， TFTP ， TELNET，SCP 等。您可以下载任何远程文件。它也支持暂停和恢复功能。

首先，您需要安装 curl。

**安装 curl**

通过按 Ctrl + Alt + T 组合键在 Ubuntu 终端中启动命令行应用程序。然后输入以下命令以使用 sudo 安装 curl。

linuxmi@linuxmi:~/www.linuxmi.com$ sudo apt install curl

当提示 [sudo] linuxmi 的密码：时请输入密码。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXSkdaVAEDwmnSZWrFQDCRUgKK1mRgo4wiauCW1HRVdyzMwe6CZQ3OicTA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装完成后，输入以下命令下载文件。

**使用源文件名下载并保存文件**

要使用与原始源文件相同的名称将文件保存在远程服务器上，请使用 - O（大写 O），然后使用 curl，如下所示：

$ curl -O [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXNwR4rseuHkbHiacWTZEQUZVLYEuaskwcMEdoDpJOGpEMiawP2RVWeJpw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

除了 - O 外，您还可以指定 “–remote-name”，如下所示。两者的工作原理相同。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXt4sUwic9272abDw2CB8LDJwNQS9twa0cundOoYtabL8fJ6CTVUTVUMQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**用其他名称下载并保存文件**

如果要下载文件并将其保存为与远程服务器中文件名不同的名称，请使用 - o（小写的 o），如下所示。当远程 URL 在 URL 中不包含文件名时，这将很有用，如下例所示。

$ curl –o [filename] [URL]

[filename] 是输出文件的新名称。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXaDmjATDfl5XxRGrib5LIe3pE7arALursj6ou1jZH3jsicYtzppMRawnQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

curl --remote-name https://github.com/chrishunt/color-schemes/archive/master.zip

curl -o linuxmi https://github.com/chrishunt/color-schemes/archive/master.zip

**下载多个文件**

要下载多个文件，请使用以下语法输入命令：

$ curl -O [URL1] -O [URL2]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXtEw6rGjq9cqiaCU9pofYInzUX58icVGcrtVSBIic6RsRfZsLWSs9bLFaA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**从 FTP 服务器下载文件**

要从 FTP 服务器下载文件，请使用以下语法输入命令：

$ curl -O ftp://ftp.linuxmi.com/www.linuxmi.com.zip

要从经过用户身份验证的 FTP 服务器下载文件，请使用以下语法：

$ curl -u [ftp_user]:[ftp_passwd] -O [ftp_URL]

**暂停并继续下载**

在下载文件时，您可以使用 Ctrl + C 手动将其暂停，或者有时由于某种原因它会自动被中断和停止，您可以恢复它。导航到您先前下载文件的目录，然后使用以下语法输入命令：

$ curl –c [选项] [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXu5a4vZpyoG1Ce89JvghjxRZCvuZiaP5PSs9y3IM2ffW4JsnbOnoG8Nw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### **使用 Wget 下载文件**

使用 wget，您可以从 Web 和 FTP 服务器下载文件和内容。Wget 是 www 和 get 的组合。它支持 FTP，SFTP，HTTP 和 HTTPS 等协议。它还支持递归下载功能。如果您要下载整个网站以供脱机查看或生成静态网站的备份，则此功能非常有用。另外，您可以使用它从各种 Web 服务器检索内容和文件。

**安装 wget**

通过按 Ctrl + Alt + T 组合键在 Ubuntu 终端中启动命令行应用程序。然后输入以下命令以使用 sudo 安装 wget。

linuxmi@linuxmi:~/www.linuxmi.com$ sudo apt install wget

当提示您输入密码时，输入 sudo 密码。

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXntbtBdZIliaSn0Sb7D7W9zaXEC4BVE7gjia9Gw4QNx6kRXyD9hjibtAXQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**使用 wget 下载文件或网页**

要下载文件或网页，请打开终端并以以下语法输入命令：

$ wget [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXA8fqNNIWW5XUzYicFRtepVibasPW7vru7nbeWDHiaryEGoOichYrhxbzFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

要保存单个网页，请使用以下语法输入命令：

$ wget [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJX7OVkpAdfia1x76U27Fx14nvicGxhZhuHsouvLFibNaGVLlSMhbpIeZkbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**下载其他名称的文件**

如果要下载和保存文件的名称与原始远程文件的名称不同，请使用 - O（大写 O），如下所示。这对您很有用，尤其是当您下载自动以名称 “index.html” 保存的网页时。

要下载其他名称的文件，请使用以下语法输入命令：

$ wget -O [文件名] [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXl1MXlj9w8WBDdPtPEanp4vMrbeSHydWNDp8yssUaiaWdf5zezYLwklQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**通过 FTP 下载文件**

要从 FTP 服务器下载文件，请使用以下语法键入命令：

$ wget [ftp_link]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXG7SuyRoPJjCgE60miaJjOtVZacBdDpiblBCoicjb6eX5KicU13HV7eLbrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

要从经过用户身份验证的 FTP 服务器下载文件，请使用以下语法：

$ wget -u [ftp_user]：[ftp_passwd] -O [ftp_URL]

**递归下载文件**

您可以使用递归下载功能来下载指定目录下的所有内容，无论是网站还是 FTP 站点。要使用递归下载功能，请使用以下语法输入命令：

$ wget –r [URL]

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJX8hXKb1Me2SDCuLFDoibCSsouaLggZnLJ1r6SWSm08icgQds4PVplHnTA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**下载多个文件**

您可以使用 wget 下载多个文件。创建一个带有文件 URL 列表的文本文件，然后使用以下语法的 wget 命令下载该列表。

$ wget –i [filename.txt]

例如，我有一个名为 “linuxmi.txt” 的文本文件，其中有两个要使用 wget 下载的 URL 列表。您可以在下图中看到我的文本文件内容。

我将使用以下命令下载文本文件中包含的文件链接：

$ wget –i linuxmi.txt

使用包含网址的文件作为下载列表

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJX7Avibm5rhMBg2Z04gR6cJib9pX6CmwcwqGzlwjSFrPtF4XcXy125a1iaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

您会看到它正在一个接一个地下载两个链接。

**暂停和恢复下载**

您可以按 Ctrl + C 暂停下载。要恢复暂停的下载，请转至先前下载文件的目录，并在 wget 之后使用– c 选项，如以下语法所示：

$ wget -c filename.zip

![图片](https://mmbiz.qpic.cn/mmbiz_png/jhtEbpg4m6G0sNV0L82GicyxOBrguDlJXW8u79iaJI6m3SAZ1Zx1rXdftJOGejPA48G7o9w1FiayUXsVz2POr1r9g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

使用以上命令，您会注意到下载已从暂停位置恢复。

**总结**

在本文中，我们讨论了 Linux 下两种命令行方法的基本用法，您可以使用它们下载文件。需要注意的一件事是，如果您在下载文件时未指定目录，则文件将下载到您正在使用的当前目录中。



## [tcpdump](https://mp.weixin.qq.com/s/iSqeA2hWbllEyq8yg-MPvQ)



今天要分享的是 `tcpdump`，它是 Linux 系统中特别有用的网络工具，通常用于故障诊断、网络分析，功能非常的强大。



相对于其它 Linux 工具而言，`tcpdump` 是复杂的。当然我也不推荐你去学习它的全部，学以致用，能够解决工作中的问题才是关键。



本文会从*应用场景*和*基础原理*出发，提供丰富的*实践案例*，让你快速的掌握 `tcpdump` 的核心使用方法，足以应对日常工作的需求。

#### 应用场景

在日常工作中遇到的很多网络问题都可以通过 tcpdump 优雅的解决：

*1.* 相信大多数同学都遇到过 SSH 连接服务器缓慢，通过 tcpdump 抓包，可以快速定位到具体原因，一般都是因为 DNS 解析速度太慢。



*2.* 当我们工程师与用户面对网络问题争执不下时，通过 tcpdump 抓包，可以快速定位故障原因，轻松甩锅，毫无压力。



*3.* 当我们新开发的网络程序，没有按照预期工作时，通过 tcpdump 收集相关数据包，从包层面分析具体原因，让问题迎刃而解。



*4.* 当我们的网络程序性能比较低时，通过 tcpdump 分析数据流特征，结合相关协议来进行网络参数优化，提高系统网络性能。



*5.* 当我们学习网络协议时，通过 tcpdump 抓包，分析协议格式，帮助我们更直观、有效、快速的学习网络协议。



上述只是简单罗列几种常见的应用场景，而 tcpdump 在网络诊断、网络优化、协议学习方面，确实是一款非常强大的网络工具，只要存在网络问题的地方，总能看到它的身影。



熟练的运用 `tcpdump`，可以帮助我们解决工作中各种网络问题，下边我们先简单学习下它的工作原理。

#### 工作原理

tcpdump 是 Linux 系统中非常有用的网络工具，运行在用户态，本质上是通过调用 `libpcap` 库的各种 `api` 来实现数据包的抓取功能。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/yVibDjicRT1VuibxMD66omEqaKPQlDrSk5MWDBicnhK8F77HJENiaxia3DUwvLsxnU3VHicnsRGqic8N4kMtzXwtxsV7BQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

通过上图，我们可以很直观的看到，数据包到达网卡后，经过数据包过滤器（BPF）筛选后，拷贝至用户态的 tcpdump 程序，以供 tcpdump 工具进行后续的处理工作，输出或保存到 pcap 文件。



数据包过滤器（BPF）主要作用，就是根据用户输入的过滤规则，只将用户关心的数据包拷贝至 tcpdump，这样能够减少不必要的数据包拷贝，降低抓包带来的性能损耗。



**思考**：这里分享一个真实的面试题

> 面试官：如果某些数据包被 iptables 封禁，是否可以通过 tcpdump 抓到包？

通过上图，我们可以很轻易的回答此问题。



因为 Linux 系统中 `netfilter` 是工作在协议栈阶段的，tcpdump 的过滤器（BPF）工作位置在协议栈之前，所以当然是可以抓到包了！



我们理解了 tcpdump 基本原理之后，下边直接进入实战！

#### 实战：基础用法

我们先通过几个简单的示例来介绍 tcpdump 基本用法。



*1.* 不加任何参数，默认情况下将抓取第一个非 lo 网卡上所有的数据包

```bash
$ tcpdump
```

*2.* 抓取 eth0 网卡上的所有数据包

```bash
$ tcpdump -i eth0
```

*3.* 抓包时指定 `-n` 选项，不解析主机和端口名。这个参数很关键，会影响抓包的性能，一般抓包时都需要指定该选项。

```bash
$ tcpdump -n -i eth0
```

*4.* 抓取指定主机  `192.168.1.100` 的所有数据包

```bash
$ tcpdump -ni eth0 host 192.168.1.100
```

*5.* 抓取指定主机 `10.1.1.2` 发送的数据包

```bash
$ tcpdump -ni eth0 src host 10.1.1.2
```

*6.* 抓取发送给 `10.1.1.2` 的所有数据包

```bash
$ tcpdump -ni eth0 dst host 10.1.1.2
```

*7.* 抓取 eth0 网卡上发往指定主机的数据包，抓到 10 个包就停止，这个参数也比较常用

```bash
$ tcpdump -ni eth0 -c 10 dst host 192.168.1.200
```

*8.* 抓取 eth0 网卡上所有 SSH 请求数据包，SSH 默认端口是 22

```bash
$ tcpdump -ni eth0 dst port 22
```

*9.* 抓取 eth0 网卡上 5 个 ping 数据包

```bash
$ tcpdump -ni eth0 -c 5 icmp
```

*10.* 抓取 eth0 网卡上所有的 arp 数据包

```bash
$ tcpdump -ni eth0 arp
```

*11.* 使用十六进制输出，当你想检查数据包内容是否有问题时，十六进制输出会很有帮助。

```bash
$ tcpdump -ni eth0 -c 1 arp -X
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
12:13:31.602995 ARP, Request who-has 172.17.92.133 tell 172.17.95.253, length 28
    0x0000:  0001 0800 0604 0001 eeff ffff ffff ac11  ................
    0x0010:  5ffd 0000 0000 0000 ac11 5c85            _.........\.
```

*12.* 只抓取 eth0 网卡上 IPv6 的流量

```bash
$ tcpdump -ni eth0 ip6
```

*13.* 抓取指定端口范围的流量

```bash
$ tcpdump -ni eth0 portrange 80-9000
```

*14.* 抓取指定网段的流量

```bash
$ tcpdump -ni eth0 net 192.168.1.0/24
```

#### 实战：高级进阶

tcpdump 强大的功能和灵活的策略，主要体现在过滤器（BPF）强大的表达式组合能力。



本节主要分享一些常见的所谓高级用法，希望读者能够举一反三，根据自己实际需求，来灵活使用它。



*1.* 抓取指定客户端访问 ssh 的数据包

```bash
$ tcpdump -ni eth0 src 192.168.1.100 and dst port 22
```

*2.* 抓取从某个网段来，到某个网段去的流量

```bash
$ tcpdump -ni eth0 src net 192.168.1.0/16 and dst net 10.0.0.0/8 or 172.16.0.0/16
```

*3.* 抓取来自某个主机，发往非 ssh 端口的流量

```bash
$ tcpdump -ni eth0 src 10.0.2.4 and not dst port 22
```

*4.* 当构建复杂查询的时候，你可能需要使用引号，单引号告诉 tcpdump 忽略特定的特殊字符，这里的 `()` 就是特殊符号，如果不用引号的话，就需要使用转义字符

```bash
$ tcpdump -ni eth0 'src 10.0.2.4 and (dst port 3389 or 22)'
```

*5.* 基于包大小进行筛选，如果你正在查看特定的包大小，可以使用这个参数

小于等于 64 字节：

```bash
$ tcpdump -ni less 64
```

大于等于 64 字节：

```bash
$ tcpdump -ni eth0 greater 64
```

等于 64 字节：

```bash
$ tcpdump -ni eth0 length == 64
```

*6.* 过滤 TCP 特殊标记的数据包

抓取某主机发送的 `RST` 数据包：

```bash
$ tcpdump -ni eth0 src host 192.168.1.100 and 'tcp[tcpflags] & (tcp-rst) != 0'
```

抓取某主机发送的 `SYN` 数据包：

```bash
$ tcpdump -ni eth0 src host 192.168.1.100 and 'tcp[tcpflags] & (tcp-syn) != 0'
```

抓取某主机发送的 `FIN` 数据包：

```bash
$ tcpdump -ni eth0 src host 192.168.1.100 and 'tcp[tcpflags] & (tcp-fin) != 0'
```

抓取 TCP 连接中的 `SYN` 或 `FIN` 包

```bash
$ tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0'
```

*7.* 抓取所有非 ping 类型的 `ICMP` 包

```bash
$ tcpdump 'icmp[icmptype] != icmp-echo and icmp[icmptype] != icmp-echoreply'
```

*8.* 抓取端口是 80，网络层协议为 IPv4， 并且含有数据，而不是 SYN、FIN 以及 ACK 等不含数据的数据包

```bash
$ tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

解释一下这个复杂的表达式，具体含义就是，整个 IP 数据包长度减去 IP 头长度，再减去 TCP 头的长度，结果不为 0，就表示数据包有 `data`，如果还不是很理解，需要自行补一下 `tcp/ip` 协议



*9.* 抓取 HTTP 报文，`0x4754` 是 `GET` 前两字符的值，`0x4854` 是 `HTTP` 前两个字符的值

```bash
$ tcpdump  -ni eth0 'tcp[20:2]=0x4745 or tcp[20:2]=0x4854'
```

#### 常用选项

通过上述的实战案例，相信大家已经掌握的 `tcpdump` 基本用法，在这里来详细总结一下常用的选项参数。



**（一）基础选项**

- `-i`：指定接口
- `-D`：列出可用于抓包的接口
- `-s`：指定数据包抓取的长度
- `-c`：指定要抓取的数据包的数量
- `-w`：将抓包数据保存在文件中
- `-r`：从文件中读取数据
- `-C`：指定文件大小，与 `-w` 配合使用
- `-F`：从文件中读取抓包的表达式
- `-n`：不解析主机和端口号，这个参数很重要，一般都需要加上
- `-P`：指定要抓取的包是流入还是流出的包，可以指定的值 `in`、`out`、`inout`



**（二）输出选项**

- `-e`：输出信息中包含数据链路层头部信息
- `-t`：显示时间戳，`tttt` 显示更详细的时间
- `-X`：显示十六进制格式
- `-v`：显示详细的报文信息，尝试 `-vvv`，`v` 越多显示越详细

#### 过滤表达式

tcpdump 强大的功能和灵活的策略，主要体现在过滤器（BPF）强大的表达式组合能力。



**（一）操作对象**

表达式中可以操作的对象有如下几种：



- `type`，表示对象的类型，比如：`host`、`net`、`port`、`portrange`，如果不指定 type 的话，默认是 host
- `dir`：表示传输的方向，可取的方式为：`src`、`dst`。
- `proto`：表示协议，可选的协议有：`ether`、`ip`、`ip6`、`arp`、`icmp`、`tcp`、`udp`。

**（二）条件组合**

表达对象之间还可以通过关键字 `and`、`or`、`not` 进行连接，组成功能更强大的表达式。



- `or`：表示或操作
- `and`：表示与操作
- `not`：表示非操作



建议看到这里后，再回头去看实战篇章的示例，相信必定会有更深的理解。如果是这样，那就达到了我预期的效果了！

#### 经验

到这里就不再加新知识点了，分享一些工作中总结的经验：

*1.* 我们要知道 `tcpdump` 不是万能药，并不能解决所有的网络问题。

*2.* 在高流量场景下，抓包可能会影响系统性能，如果是在生产环境，请谨慎使用！

*3.* 在高流量场景下，`tcpdump` 并不适合做流量统计，如果需要，可以使用交换机镜像的方式去分析统计。

*4.* 在 Linux 上使用 `tcpdump` 抓包，结合 `wireshark` 工具进行数据分析，能事半功倍。

*5.* 抓包时，尽可能不要使用 `any` 接口来抓包。

*6.* 抓包时，尽可能指定详细的数据包过滤表达式，减少无用数据包的拷贝。

*7.* 抓包时，尽量指定 `-n` 选项，减少解析主机和端口带来的性能开销。

#### 最后

通过上述内容，我们知道 tcpdump 是一款功能强大的故障诊断、网络分析工具。在我们的日常工作中，遇到的网络问题总是能够通过 tcpdump 来解决。



不过 tcpdump 相对于其它 Linux 命令来说，会复杂很多，但鉴于它强大功能的诱惑力，我们多花一些时间是值得的。要想很好地掌握 tcpdump，需要对网络报文（`TCP/IP` 协议）有一定的了解。



当然，对于简单的使用来说，只要有网络基础概念就行，掌握了 tcpdump 常用方法，就足以应付工作中大部分网络相关的疑难杂症了。



## [gdb](https://mp.weixin.qq.com/s/OLHurXiioQchhai4i9WYsw)





# git使用

## [手把手指导您使用 Git](https://mp.weixin.qq.com/s/B7rjSShEFBkuxM7wsNtlkg)





## [克隆、修改、添加和删除文件？](https://mp.weixin.qq.com/s/g_1iDs0W1ToGlmYoUbjEpw)





## [git 操作，看这一篇文章就够了！](https://mp.weixin.qq.com/s/_AJKZT8oCza8hBAxT-kU2g)

**Git 是什么？**

Git 是目前世界上最先进的分布式版本控制系统（没有之一）。



**Git 有什么特点？**

简单来说就是：**高端大气上档次！**



**那什么是版本控制系统？**

如果你用 Microsoft Word 写过长篇大论，那你一定有这样的经历：

想删除一个段落，又怕将来想恢复找不回来怎么办？有办法，先把当前文件 “另存为……” 一个新的 Word 文件，再接着改，改到一定程度，再 “另存为……” 一个新文件，这样一直改下去，最后你的 Word 文档变成了这样：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/QFzRdz9libEYJx98xK1ickW62zcHibtpaiavt1jPxIrBtjOBaBsgibLsqY9YdGk3PWG3KGL0rCMo7vxMvdZapAtxIog/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

过了一周，你想找回被删除的文字，但是已经记不清删除前保存在哪个文件里了，只好一个一个文件去找，真麻烦。

看着一堆乱七八糟的文件，想保留最新的一个，然后把其他的删掉，又怕哪天会用上，还不敢删，真郁闷。

更要命的是，有些部分需要你的财务同事帮助填写，于是你把文件 Copy 到 U 盘里给她（也可能通过 Email 发送一份给她），然后，你继续修改 Word 文件。一天后，同事再把 Word 文件传给你，此时，你必须想想，发给她之后到你收到她的文件期间，你作了哪些改动，得把你的改动和她的部分合并，真困难。

于是你想，如果有一个软件，不但能自动帮我记录每次文件的改动，还可以让同事协作编辑，这样就不用自己管理一堆类似的文件了，也不需要把文件传来传去。如果想查看某次改动，只需要在软件里瞄一眼就可以，岂不是很方便？

这个软件用起来就应该像这个样子，能记录每次文件的改动：

| 版本 | 文件名      | 用户 | 说明                    | 日期       |
| :--- | :---------- | :--- | :---------------------- | :--------- |
| 1    | service.doc | 张三 | 删除了软件服务条款 5    | 7/12 10:38 |
| 2    | service.doc | 张三 | 增加了 License 人数限制 | 7/12 18:09 |
| 3    | service.doc | 李四 | 财务部门调整了合同金额  | 7/13 9:51  |
| 4    | service.doc | 张三 | 延长了免费升级周期      | 7/14 15:17 |



![图片](https://mmbiz.qpic.cn/mmbiz_jpg/rrbZLC2ibIgtgV382cFCwmibpHFT7jndu1ibEDpFia0dzsjETHdt0HFzYlVRnHIaumpf3QyVos7giadDicqSku9zOEibw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



Git 有很多命令记不住，怎么办？

一般来说，日常使用只要记住下图 6 个命令，就可以了。但是熟练使用，恐怕要记住 60～100 个命令。

![图片](https://mmbiz.qpic.cn/mmbiz_png/QFzRdz9libEZPZW2tdOpBSZ3DvJl6ibxIzbFsPoFbL8rT7DBpcOgFJ8JoUd3VCXvGdHQ0ApqfOBGXoNrPFGo5cSg/640?tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。



```bash
Workspace：工作区Index / Stage：暂存区Repository：仓库区（或本地仓库）Remote：远程仓库
```

### **一、新建代码库**



```bash
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

#  下载一个项目和它的整个代码历史
$ git clone [url]
```

### **二、配置**

 Git 的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```bash
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

### **三、增加 / 删除文件**



```bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...
# 添加指定目录到暂存区，包括子目录
$ git add [dir]
# 添加当前目录的所有文件到暂存区
$ git add .
# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p
# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...
# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

### 四、代码提交

- 

```bash
# 提交暂存区到仓库区
$ git commit -m [message]
# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]
# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a
# 提交时显示所有diff信息
$ git commit -v
# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]
# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...

```

### 五、分支

```bash
# 列出所有本地分支
$ git branch
# 列出所有远程分支
$ git branch -r
# 列出所有本地分支和远程分支
$ git branch -a
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 新建一个分支，并切换到该分支
$ git checkout -b [branch]
# 新建一个分支，指向指定commit
$ git branch [branch] [commit]
# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]
# 切换到指定分支，并更新工作区
$ git checkout [branch-name]
# 切换到上一个分支
$ git checkout -
# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]
# 合并指定分支到当前分支
$ git merge [branch]
# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]
# 删除分支
$ git branch -d [branch-name]
# 删除远程分支
$ git push origin --delete [branch-name]$ git branch -dr [remote/branch]

```

### 六、标签

```bash
# 列出所有tag
$ git tag
# 新建一个tag在当前commit
$ git tag [tag]
# 新建一个tag在指定commit
$ git tag [tag] [commit]
# 删除本地tag
$ git tag -d [tag]
# 删除远程tag
$ git push origin :refs/tags/[tagName]
# 查看tag信息
$ git show [tag]
# 提交指定tag
$ git push [remote] [tag]
# 提交所有tag
$ git push [remote] --tags
# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]

```

### 七、查看信息

```bash
# 显示有变更的文件
$ git status
# 显示当前分支的版本历史
$ git log
# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat
# 搜索提交历史，根据关键词
$ git log -S [keyword]
# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s
# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature
# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]
# 显示指定文件相关的每一次diff
$ git log -p [file]
# 显示过去5次提交
$ git log -5 --pretty --oneline
# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
# 显示暂存区和工作区的差异
$ git diff
# 显示暂存区和上一个commit的差异
$ git diff --cached [file]
# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD
# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]
# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"
# 显示某次提交的元数据和内容变化
$ git show [commit]
# 显示某次提交发生变化的文件
$ git show --name-only [commit]
# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]
# 显示当前分支的最近几次提交
$ git reflog
```

### 八、远程同步

```bash
# 下载远程仓库的所有变动
$ git fetch [remote]
# 显示所有远程仓库
$ git remote -v
# 显示某个远程仓库的信息
$ git remote show [remote]
# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]
# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]
# 上传本地指定分支到远程仓库
$ git push [remote] [branch]
# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
# 推送所有分支到远程仓库
$ git push [remote] --all
```

## 九、撤销



```bash
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop

```

## 十、其他

```bash
# 生成一个可供发布的压缩包
$ git archive
```



## [Git 使用教程！](https://mp.weixin.qq.com/s/YQOhF7Nc94ZRLxFWpd2Wbw)

### **一、Git 是什么？**
Git 是目前世界上最先进的分布式版本控制系统。
工作原理 / 流程：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxiaCC3m4XHaxHaaqLkYlukTUALnHN74icx3VZyIM3uEXz7JA9ldicwe8BQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

### **二、SVN 与 Git 的最主要的区别？**

SVN 是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就纳闷了。

Git 是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件 A，其他人也在电脑上改了文件 A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

### **三、在 windows 上如何安装 Git？**

msysgit 是 windows 版的 Git, 如下：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxosiaDLmDfnv75UCUBV06SuFZBoyayrJxatPyPc7gUryQWLTyh0RZa1w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

需要从网上下载一个，然后进行默认安装即可。安装完成后，在开始菜单里面找到 "Git --> Git Bash", 如下：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxFjQkg9D22szLw1gZ3Ss5ZXIXaaiatM050XBzkItSWEBMVrtq90a3BAw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

会弹出一个类似的命令窗口的东西，就说明 Git 安装成功。如下：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxn02qf5YPEzHMHnCC5UfsfQUJLX0QKFFU5pguJWydNQsiaeQ6b3NtHJQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装完成后，还需要最后一步设置，在命令行输入如下：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxI8VU7mUGVtA2tvr4WLUSF0rAuP0dVicSeiab136Pr8scaKAKW2BUBXxQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因为 Git 是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识。

注意：git config --global 参数，有了这个参数，表示你这台机器上所有的 Git 仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。

### **四、如何操作？**

**1. 创建版本库。**

什么是版本库？版本库又名仓库，英文名 repository, 你可以简单的理解一个目录，这个目录里面的所有文件都可以被 Git 管理起来，每个文件的修改，删除，Git 都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件” 还原”。

所以创建一个版本库也非常简单，如下我是 D 盘 –> www 下 目录下新建一个 testgit 版本库。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx1h3uRSPw2KGCqJa6JKW2icELLWv9yDibjJcQnQQdWiakRHWCCE0ZJ1tWw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
pwd 命令是用于显示当前的目录。

通过命令 git init 把这个目录变成 git 可以管理的仓库，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxw1YVClI71BJSN2tpvKc0r9asuATe8rc0l9fg41p7X9cz4OHwB3duaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这时候你当前 testgit 目录下会多了一个.git 的目录，这个目录是 Git 来跟踪管理版本的，没事千万不要手动乱改这个目录里面的文件，否则，会把 git 仓库给破坏了。如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxaneuRvlyPJhnMgZTLL365iaTqvBZibLwPiaq0OxiblbhYRIUJdibX5PHfaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

下面先看下 demo 如下演示：

我在版本库 testgit 目录下新建一个记事本文件 readme.txt 内容如下：11111111

第一步：使用命令 git add readme.txt 添加到暂存区里面去。如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxbH4oUnicLrxsrsMf5bXot8hl4EuExIZ0w3sBM5zktDTxxYEuRLONxaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果和上面一样，没有任何提示，说明已经添加成功了。

第二步：用命令 git commit 告诉 Git，把文件提交到仓库。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx2IJicDvF99RasEj8lhSnXI4B5glBAhw57qBMuWUJdVw61sykLBbZBEA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
现在我们已经提交了一个 readme.txt 文件了，我们下面可以通过命令 git status 来查看是否还有文件未提交，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxqcB7Jia6OXyLqdBhcJ0X4WU62qNicfiblnd0rZF3yz6A7jRFvf8K29Qbg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

说明没有任何文件未提交，但是我现在继续来改下 readme.txt 内容，比如我在下面添加一行 2222222222 内容，继续使用 git status 来查看下结果，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx8SxcS22lvuD97h5jGJdMRwg5mVK2XByialZY3BoE328cfR0vPuKfCicg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上面的命令告诉我们 readme.txt 文件已被修改，但是未被提交的修改。

把文件添加到版本库中。

首先要明确下，所有的版本控制系统，只能跟踪文本文件的改动，比如 txt 文件，网页，所有程序的代码等，Git 也不列外，版本控制系统可以告诉你每次的改动，但是图片，视频这些二进制文件，虽能也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是知道图片从 1kb 变成 2kb，但是到底改了啥，版本控制也不知道。

接下来我想看下 readme.txt 文件到底改了什么内容，如何查看呢？可以使用如下命令：

git diff readme.txt 如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxUNT0TGOicgPicIicJcOhfJgos4KeqzfuQgOicg50VaNMdBIe7tkxLeXs5A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如上可以看到，readme.txt 文件内容从一行 11111111 改成 二行 添加了一行 22222222 内容。

知道了对 readme.txt 文件做了什么修改后，我们可以放心的提交到仓库了，提交修改和提交文件是一样的 2 步 (第一步是 git add 第二步是：git commit)。

如下：
![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxUn21BC9qPH8N7pfeKXpWFD5TEia04NGT9zicWqdXwxaQgJFgqGuH5Fkg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
**2. 版本回退：**
如上，我们已经学会了修改文件，现在我继续对 readme.txt 文件进行修改，再增加一行

内容为 33333333333333. 继续执行命令如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx8TZM765aCk7FVHJt6VEtKseBSBwhdFc9EwchbVjhgCJHPbFgZgxRDQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
现在我已经对 readme.txt 文件做了三次修改了，那么我现在想查看下历史记录，如何查呢？我们现在可以使用命令 git log 演示如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxXBTeneAI92ZOgvhTOzicfZKvHiaibDqm0BrHEzPrOoo5osFcqHrR28rBg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

git log 命令显示从最近到最远的显示日志，我们可以看到最近三次提交，最近的一次是，增加内容为 333333. 上一次是添加内容 222222，第一次默认是 111111. 如果嫌上面显示的信息太多的话，我们可以使用命令 git log –pretty=oneline 演示如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx0fw4yCALyund7fuuIcdRVt7XwHYUfRAAN38mzEyxl5Ss8Zuvzrg7KQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/jhtEbpg4m6Hicg8GicVuMiaKfhq6tG3JBiaCT6dIwJpMKQdpPMeYZ3tzmxrwrWHopqZaBR8BV6gFEto4aUEhr5ibhcg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我想使用版本回退操作，我想把当前的版本回退到上一个版本，要使用什么命令呢？可以使用如下 2 种命令，第一种是：git reset --hard HEAD^ 那么如果要回退到上上个版本只需把 HEAD^ 改成 HEAD^^ 以此类推。那如果要回退到前 100 个版本的话，使用上面的方法肯定不方便，我们可以使用下面的简便命令操作：git reset --hard HEAD~100 即可。未回退之前的 readme.txt 内容如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxKmtk6uNibcOJOiaia955rrSKOayqRUeAd62qibicZ3UoRibkicjMhlp2VHn4Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果想回退到上一个版本的命令如下操作：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx1eqDr0rBic0YiameTSpfDbGibnv6ibXz9I7egjrYcMy9JmdgCf2CDAeDow/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

再来查看下 readme.txt 内容如下：通过命令 cat readme.txt 查看

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxAVr16pKOlFZxEqwWNsjUMmzcdiazXp2IYemHA9upn7FH6eNKlnia57Nw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到，内容已经回退到上一个版本了。我们可以继续使用 git log 来查看下历史记录信息，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxzwR6ibLPia9HmdzM138Mxev2b6qjsWlRZPtUDI1Yw73QV1HfcCYib3DjQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们看到 增加 333333 内容我们没有看到了，但是现在我想回退到最新的版本，如：有 333333 的内容要如何恢复呢？我们可以通过版本号回退，使用命令方法如下：

git reset --hard 版本号 ，但是现在的问题假如我已经关掉过一次命令行或者 333 内容的版本号我并不知道呢？要如何知道增加 3333 内容的版本号呢？可以通过如下命令即可获取到版本号：git reflog 演示如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxpFmicd5ic3Qc1Gz4FVlTAhXodbgOCj5NBLicibDg7rrFJialJAurM7L7aNQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

通过上面的显示我们可以知道，增加内容 3333 的版本号是 6fcfc89. 我们现在可以命令

git reset --hard 6fcfc89 来恢复了。演示如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxnBYVj3lp0vH6azdLoT9UgZxS4qA3fbeNDDePpYhGWiclUl60jz9TKLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到 目前已经是最新的版本了。

**3. 理解工作区与暂存区的区别？**
工作区：就是你在电脑上看到的目录，比如目录下 testgit 里的文件 (.git 隐藏目录版本库除外)。或者以后需要再新建的目录文件等等都属于工作区范畴。
版本库 (Repository)：工作区有一个隐藏目录.git, 这个不属于工作区，这是版本库。其中版本库里面存了很多东西，其中最重要的就是 stage (暂存区)，还有 Git 为我们自动创建了第一个分支 master, 以及指向 master 的一个指针 HEAD。

我们前面说过使用 Git 提交文件到版本库有两步：

**第一步：是使用 git add 把文件添加进去，实际上就是把文件添加到暂存区。**

**第二步：使用 git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支上。**

我们继续使用 demo 来演示下：

我们在 readme.txt 再添加一行内容为 4444444，接着在目录下新建一个文件为 test.txt 内容为 test，我们先用命令 git status 来查看下状态，如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在我们先使用 git add 命令把 2 个文件都添加到暂存区中，再使用 git status 来查看下状态，如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接着我们可以使用 git commit 一次性提交到分支上，如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**4. Git 撤销修改和删除文件操作。**
\1. 撤销修改：
比如我现在在 readme.txt 文件里面增加一行 内容为 555555555555，我们先通过命令查看如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxAibZH3tONgGp9hWriaoHjdVWgduTWxYVInEXO6cEyicdZTE5rHGibSuoMQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
在我未提交之前，我发现添加 5555555555555 内容有误，所以我得马上恢复以前的版本，现在我可以有如下几种方法可以做修改：

**第一：如果我知道要删掉那些内容的话，直接手动更改去掉那些需要的文件，然后 add 添加到暂存区，最后 commit 掉。**

**第二：我可以按以前的方法直接恢复到上一个版本。使用 git reset --hard HEAD^**

但是现在我不想使用上面的 2 种方法，我想直接想使用撤销命令该如何操作呢？首先在做撤销之前，我们可以先用 git status 查看下当前的状态。如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxVk0Vl4jMqol7kZOnoPO0tEa05ZOfDicaribrib8OZ1a3INpKkicicGVSIDQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以发现，Git 会告诉你，git checkout -- file 可以丢弃工作区的修改，如下命令：
git checkout -- readme.txt, 如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

命令 git checkout --readme.txt 意思就是，把 readme.txt 文件在工作区做的修改全部撤销，这里有 2 种情况，如下：

**1.readme.txt 自动修改后，还没有放到暂存区，使用 撤销修改就回到和版本库一模一样的状态。
\2. 另外一种是 readme.txt 已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态。**
对于第二种情况，我想我们继续做 demo 来看下，假如现在我对 readme.txt 添加一行 内容为 6666666666666，我 git add 增加到暂存区后，接着添加内容 7777777，我想通过撤销命令让其回到暂存区后的状态。如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注意：命令 git checkout -- readme.txt 中的 -- 很重要，如果没有 -- 的话，那么命令变成创建分支了。

\2. 删除文件。
假如我现在版本库 testgit 目录添加一个文件 b.txt, 然后提交。如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如上：一般情况下，可以直接在文件目录中把文件删了，或者使用如上 rm 命令：rm b.txt ，如果我想彻底从版本库中删掉了此文件的话，可以再执行 commit 命令 提交掉，现在目录是这样的，

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

只要没有 commit 之前，如果我想在版本库中恢复此文件如何操作呢？

可以使用如下命令 git checkout -- b.txt，如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

再来看看我们 testgit 目录，添加了 3 个文件了。如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### **五、远程仓库**
在了解之前，先注册 github 账号，由于你的本地 Git 仓库和 github 仓库之间的传输是通过 SSH 加密的，所以需要一点设置：
第一步：创建 SSH Key。在用户主目录下，看看有没有.ssh 目录，如果有，再看看这个目录下有没有 id_rsa 和 id_rsa.pub 这两个文件，如果有的话，直接跳过此如下命令，如果没有的话，打开命令行，输入如下命令：

ssh-keygen -t rsa –C “youremail@example.com”, 由于我本地此前运行过一次，所以本地有，如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxbyG27Gkt7KQt4BeZKXVgPyyWa6gu68SjCOS7WNLmZvR6KUEDZFH0eA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

id_rsa 是私钥，不能泄露出去，id_rsa.pub 是公钥，可以放心地告诉任何人。

第二步：登录 github, 打开” settings” 中的 SSH Keys 页面，然后点击 “Add SSH Key”, 填上任意 title，在 Key 文本框里黏贴 id_rsa.pub 文件的内容。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx9HfRIwgwuTkiaggs8OS1CZYHGMpnKVx6Yl2bicM8s9NGb69hrVMziaBAQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

点击 Add Key，你就应该可以看到已经添加的 key。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

\1. 如何添加远程库？
现在的情景是：我们已经在本地创建了一个 Git 仓库后，又想在 github 创建一个 Git 仓库，并且希望这两个仓库进行远程同步，这样 github 的仓库可以作为备份，又可以其他人通过该仓库来协作。

首先，登录 github 上，然后在右上角找到 “create a new repo” 创建一个新的仓库。如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在 Repository name 填入 testgit，其他保持默认设置，点击 “Create repository” 按钮，就成功地创建了一个新的 Git 仓库：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
目前，在GitHub上的这个testgit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
```

现在，我们根据 GitHub 的提示，在本地的 testgit 仓库下运行命令：

```bash
git remote add origin https://github.com/tugenhua0707/testgit.git
```

所有的如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

把本地库的内容推送到远程，使用 git push 命令，实际上是把当前分支 master 推送到远程。

由于远程库是空的，我们第一次推送 master 分支时，加上了 –u 参数，Git 不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令。推送成功后，可以立刻在 github 页面中看到远程库的内容已经和本地一模一样了，上面的要输入 github 的用户名和密码如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

从现在起，只要本地作了提交，就可以通过如下命令：

```
git push origin master
```

把本地 master 分支的最新修改推送到 github 上了，现在你就拥有了真正的分布式版本库了。

\2. 如何从远程库克隆？

上面我们了解了先有本地库，后有远程库时候，如何关联远程库。

现在我们想，假如远程库有新的内容了，我想克隆到本地来 如何克隆呢？

首先，登录 github，创建一个新的仓库，名字叫 testgit2. 如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxyL3oLKwQwWFmw0Mg5pDn9xJCgXg27uwGMWcpEQcG0miaic8LuyekBw1A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如下，我们看到：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxtUO9rb7NUo5Iq7UCxsRkEfSvhvKASkwZ7FGgW2s548W3mRZ2fMWEOw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在，远程库已经准备好了，下一步是使用命令 git clone 克隆一个本地库了。如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接着在我本地目录下 生成 testgit2 目录了，如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### **六、创建与合并分支**

在 版本回填退里，你已经知道，每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在 Git 里，这个分支叫主分支，即 master 分支。HEAD 严格来说不是指向提交，而是指向 master，master 才是指向提交的，所以，HEAD 指向的就是当前分支。

首先，我们来创建 dev 分支，然后切换到 dev 分支上。如下操作：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

git checkout 命令加上 –b 参数表示创建并切换，相当于如下 2 条命令

```
git branch dev
git checkout dev
```

git branch 查看分支，会列出所有的分支，当前分支前面会添加一个星号。然后我们在 dev 分支上继续做 demo，比如我们现在在 readme.txt 再增加一行 7777777777777

首先我们先来查看下 readme.txt 内容，接着添加内容 77777777，如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在 dev 分支工作已完成，现在我们切换到主分支 master 上，继续查看 readme.txt 内容如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在我们可以把 dev 分支上的内容合并到分支 master 上了，可以在 master 分支上，使用如下命令 git merge dev 如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

git merge 命令用于合并指定分支到当前分支上，合并后，再查看 readme.txt 内容，可以看到，和 dev 分支最新提交的是完全一样的。

注意到上面的 Fast-forward 信息，Git 告诉我们，这次合并是 “快进模式”，也就是直接把 master 指向 dev 的当前提交，所以合并速度非常快。

合并完成后，我们可以接着删除 dev 分支了，操作如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结创建与合并分支命令如下：

查看分支：git branch

创建分支：git branch name

切换分支：git checkout name

创建 + 切换分支：git checkout –b name

合并某分支到当前分支：git merge name

删除分支：git branch –d name

**如何解决冲突？**

下面我们还是一步一步来，先新建一个新分支，比如名字叫 fenzhi1，在 readme.txt 添加一行内容 8888888，然后提交，如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxJkPIPnRTjL5HOeWHEHumhXBHBibeMMXdpNtM5hzkVv3Yv8ic7N6oRU7g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

同样，我们现在切换到 master 分支上来，也在最后一行添加内容，内容为 99999999，如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在我们需要在 master 分支上来合并 fenzhi1，如下操作：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Git 用 <<<<<<<，=======，>>>>>>> 标记出不同分支的内容，其中 <<<HEAD 是指主分支修改的内容，>>>>>fenzhi1 是指 fenzhi1 上修改的内容，我们可以修改下如下后保存：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我想查看分支合并的情况的话，需要使用命令 git log. 命令行演示如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

\3. 分支管理策略。通常合并分支时，git 一般使用”Fast forward” 模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff 来禁用”Fast forward” 模式。首先我们来做 demo 演示下：

- 创建一个 dev 分支。
- 修改 readme.txt 内容。
- 添加到暂存区。
- 切换回主分支 (master)。
- 合并 dev 分支，使用命令 git merge –no-ff -m “注释” dev
- 查看历史记录

截图如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKx7iceyn13JXfXDYiaTUDO9xs7KXIMBRB1SjFJuobHCR97HwabsoIk5jmw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

分支策略：首先 master 主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的 dev 分支上干活，干完后，比如上要发布，或者说 dev 分支代码稳定后可以合并到主分支 master 上来。

### **七、bug 分支**
在开发中，会经常碰到 bug 问题，那么有了 bug 就需要修复，在 Git 中，分支是很强大的，每个 bug 都可以通过一个临时分支来修复，修复完成后，合并分支，然后将临时的分支删除掉。

比如我在开发中接到一个 404 bug 时候，我们可以创建一个 404 分支来修复它，但是，当前的 dev 分支上的工作还没有提交。比如如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

并不是我不想提交，而是工作进行到一半时候，我们还无法提交，比如我这个分支 bug 要 2 天完成，但是我 issue-404 bug 需要 5 个小时内完成。怎么办呢？还好，Git 还提供了一个 stash 功能，可以把当前工作现场 ” 隐藏起来”，等以后恢复现场后继续工作。如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

所以现在我可以通过创建 issue-404 分支来修复 bug 了。

首先我们要确定在那个分支上修复 bug，比如我现在是在主分支 master 上来修复的，现在我要在 master 分支上创建一个临时分支，演示如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

修复完成后，切换到 master 分支上，并完成合并，最后删除 issue-404 分支。演示如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
现在，我们回到 dev 分支上干活了。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

工作区是干净的，那么我们工作现场去哪里呢？我们可以使用命令 git stash list 来查看下。如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

工作现场还在，Git 把 stash 内容存在某个地方了，但是需要恢复一下，可以使用如下 2 个方法：

**1.git stash apply 恢复，恢复后，stash 内容并不删除，你需要使用命令 git stash drop 来删除。
\2. 另一种方式是使用 git stash pop, 恢复的同时把 stash 内容也删除了。**
演示如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxUzE75xA8deXFxdwaokkEtXXWKeQw1fFlVzS1OU40ufqxBa31rjyicUg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### **八、多人协作**
当你从远程库克隆时候，实际上 Git 自动把本地的 master 分支和远程的 master 分支对应起来了，并且远程库的默认名称是 origin。

1. 要查看远程库的信息 使用 git remote
2. 要查看远程库的详细信息 使用 git remote –v



如下演示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxxFFDnmuJtDibwbTCQWS4z4E5DN2O5xnq5WLzIPy8y3fPLt3XhUlwFicA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**1. 推送分支：**

推送分支就是把该分支上所有本地提交到远程库中，推送时，要指定本地分支，这样，Git 就会把该分支推送到远程库对应的远程分支上：使用命令 git push origin master

比如我现在的 github 上的 readme.txt 代码如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxjMJarLRzRPIpRdlr0nxYV8aszZbhMbZZcclTGoU65gVElTLr5l2rBw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

本地的 readme.txt 代码如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxNZ7RssbD3x30aTSNlvhZAmeXwDQnxDMhy7rjgqouaYXatkjBYiaRRxg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我想把本地更新的 readme.txt 代码推送到远程库中，使用命令如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxlYMqb9dGqq8vJ3n6jYKt8Dm1Af0iafXh2eHSIjiajk4QtawKX5NIWkPQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们可以看到如上，推送成功，我们可以继续来截图 github 上的 readme.txt 内容 如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到 推送成功了，如果我们现在要推送到其他分支，比如 dev 分支上，我们还是那个命令 git push origin dev

那么一般情况下，那些分支要推送呢？

master 分支是主分支，因此要时刻与远程同步。
一些修复 bug 分支不需要推送到远程去，可以先合并到主分支上，然后把主分支 master 推送到远程去。
**2. 抓取分支：**

多人协作时，大家都会往 master 分支上推送各自的修改。现在我们可以模拟另外一个同事，可以在另一台电脑上（注意要把 SSH key 添加到 github 上）或者同一台电脑上另外一个目录克隆，新建一个目录名字叫 testgit2

但是我首先要把 dev 分支也要推送到远程去，如下

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接着进入 testgit2 目录，进行克隆远程的库到本地来，如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在目录下生成有如下所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
现在我们的小伙伴要在 dev 分支上做开发，就必须把远程的 origin 的 dev 分支到本地来，于是可以使用命令创建本地 dev 分支：

```
git checkout –b dev origin/dev
```

现在小伙伴们就可以在 dev 分支上做开发了，开发完成后把 dev 分支推送到远程库时。

如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxSI3BiaLtauS2CafXJ8O7uWX8mbTuBLhibMCMAjYkxeWOqwiajiaXCFwV6w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

小伙伴们已经向 origin/dev 分支上推送了提交，而我在我的目录文件下也对同样的文件同个地方作了修改，也试图推送到远程库时，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxtiavtxSHLqrCAP5fPLSP1E8icNlFicwsVc2V9MibjHCdbIR725DWV3icY8w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

由上面可知：推送失败，因为我的小伙伴最新提交的和我试图推送的有冲突，解决的办法也很简单，上面已经提示我们，先用 git pull 把最新的提交从 origin/dev 抓下来，然后在本地合并，解决冲突，再推送。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxugZ4icLTBK2QlBibjqXZRFEhFHH90VuwlHo30ib7ic2Iv1aJwF0UvKMiadA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

git pull 也失败了，原因是没有指定本地 dev 分支与远程 origin/dev 分支的链接，根据提示，设置 dev 和 origin/dev 的链接：如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxZJenbMVZgrPpic6qkdtSQLsvNpme1ltbsWrbichK6pbJJMQ0iah3kyyAg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这回 git pull 成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的 解决冲突完全一样。解决后，提交，再 push：
我们可以先来看看 readme.txt 内容了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxm3Ijz0ScjY4cSa27ibmvZUZtYFD52b3q88Hk8f6I3cNLcxmiadia0NnLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在手动已经解决完了，我接在需要再提交，再 push 到远程库里面去。如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e1jmIzRpwWiaEynpFwWSmr59icj386rKKxiao0xm3MgoHqu8yLA6pH79BVS8URqfg1aR9iacPV0sJZw5htaPdkwdvw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因此：多人协作工作模式一般是这样的：

首先，可以试图用 git push origin branch-name 推送自己的修改.
如果推送失败，则因为远程分支比你的本地更新早，需要先用 git pull 试图合并。
如果合并有冲突，则需要解决冲突，并在本地提交。再用 git push origin branch-name 推送。



## [图文详解 Git 工作原理](https://mp.weixin.qq.com/s/sRnfLAe5Aq720iwNAgzwVQ)



本文图解 Git 中的最常用命令。如果你稍微理解 Git 的工作原理，这篇文章能够让你理解的更透彻。



### 基本用法

![图片](https://mmbiz.qpic.cn/mmbiz_png/b2YlTLuGbKDsbJzupnILVFhPtMaRjmvPKYRqTMjibE9pnd8oiawLVrQbOHQe4wBXkBQkzpKCWPKBqWgOLgwccBug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdVldJGOGXuibWZGhib6OyVXMic1ZznAwYtO2eFpicV29aUVpNpwCMDia6B4w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



上面的四条命令在工作目录、暂存目录（也叫做索引）和仓库之间复制文件。



- git add files 把当前文件放入暂存区域。
- git commit 给暂存区域生成快照并提交。
- git reset – files 用来撤销最后一次 git add files，你也可以用 git reset 撤销所有暂存区域文件。
- git checkout – files 把文件从暂存区域复制到工作目录，用来丢弃本地修改。



你可以用 git reset -p，git checkout -p，or git add -p 进入交互模式。



也可以跳过暂存区域直接从仓库取出文件或者直接提交代码。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdYReXDk7rS1rOicCQ7WtiagoMiaicu2xX2XmnNtiaiariayeskukH5fu1J3UibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



- git commit -a 相当于运行 git add 把所有当前目录下的文件加入暂存区域再运行。
- git commit files 进行一次包含最后一次提交加上工作目录中文件快照的提交。并且文件被添加到暂存区域。
- git checkout HEAD – files 回滚到复制最后一次提交。



###  约定

![图片](https://mmbiz.qpic.cn/mmbiz_png/b2YlTLuGbKDsbJzupnILVFhPtMaRjmvPKYRqTMjibE9pnd8oiawLVrQbOHQe4wBXkBQkzpKCWPKBqWgOLgwccBug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



后文中以下面的形式使用图片。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdbdwYDx6TQR4KD6FWsr2B8UI1QBYzIlp7LGzmSLG1DU0Z1gUESMqGDg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



绿色的 5 位字符表示提交的 ID，分别指向父节点。分支用橘色显示，分别指向特定的提交。当前分支由附在其上的 HEAD 标识。这张图片里显示最后 5 次提交，ed489 是最新提交。master 分支指向此次提交，另一个 maint 分支指向祖父提交节点。



### 命令详解

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)



 #### **Diff**



有许多种方法查看两次提交之间的变动，下面是一些示例。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdDpAGboibbjp15iaKlk0LyveH5aibicWiaibs0icmJgohye76ojHT8gBOVQA3w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)



 #### **Commit**



提交时，Git 用暂存区域的文件创建一个新的提交，并把此时的节点设为父节点。然后把当前分支指向新的提交节点。下图中，当前分支是 master。在运行命令之前，master 指向 ed489，提交后，master 指向新的节点 f0cec 并以 ed489 作为父节点。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdVZvuXkGQXTnibv4hkRL3ALkCXGibtgPicPgmTjllf8dRg7sJ9PNozzOaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



即便当前分支是某次提交的祖父节点，git 会同样操作。下图中，在 master 分支的祖父节点 maint 分支进行一次提交，生成了 1800b。这样，maint 分支就不再是 master 分支的祖父节点。此时，合并 [1]（或者衍合 [2]）是必须的。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacduYic50KBcqN9pwAdTEJDI63zxQTx8aapgIkopvqXCDwK1UpQUf9icypg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果想更改一次提交，使用 git commit –amend。Git 会使用与当前提交相同的父节点进行一次新提交，旧的提交会被取消。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdrSBGHl2PZIfsTticrNYGQDgQqLC1Zn7rJVicpIJaJXDkiaVjrAnbfh0Bw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



另一个例子是分离 HEAD 提交 [3]，后文讲。



#### **Checkout**



Checkout 命令用于从历史提交（或者暂存区域）中拷贝文件到工作目录，也可用于切换分支。



当给定某个文件名（或者打开 - p 选项，或者文件名和 - p 选项同时打开）时，Git 会从指定的提交中拷贝文件到暂存区域和工作目录。比如，git checkout HEAD~ foo.c 会将提交节点 HEAD~（即当前提交节点的父节点）中的 foo.c 复制到工作目录并且加到暂存区域中。（如果命令中没有指定提交节点，则会从暂存区域中拷贝内容。）注意当前分支不会发生变化。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacd3nvKaac5eIaNEa0ibH7D3HGJRNHA57Vc8icte35clLq7sbOCo41Q9uKA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



当不指定文件名，而是给出一个（本地）分支时，那么 HEAD 标识会移动到那个分支（也就是说，我们 “切换” 到那个分支了），然后暂存区域和工作目录中的内容会和 HEAD 对应的提交节点一致。新提交节点（下图中的 a47c3）中的所有文件都会被复制（到暂存区域和工作目录中）；只存在于老的提交节点（ed489）中的文件会被删除；不属于上述两者的文件会被忽略，不受影响。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacd4njffFGDBaiaiawib1jv6eS5umZXNwl0jdVibDlCdZrSN0aT6JGYgu2bYQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果既没有指定文件名，也没有指定分支名，而是一个标签、远程分支、SHA-1 值或者是像 master~3 类似的东西，就得到一个匿名分支，称作 detached HEAD（被分离的 HEAD 标识）。这样可以很方便地在历史版本之间互相切换。比如说你想要编译 1.6.6.1 版本的 Git，你可以运行 git checkout v1.6.6.1（这是一个标签，而非分支名），编译，安装，然后切换回另一个分支，比如说 git checkout master。然而，当提交操作涉及到 “分离的 HEAD” 时，其行为会略有不同，详情见在下面。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdVxGHWej6PnlIiaoREvuQrnIicXPaltU9SAJ72TbePvOtA0icELZOlYcdg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



#### **HEAD 标识处于分离状态时的提交操作**



当 HEAD 处于分离状态（不依附于任一分支）时，提交操作可以正常进行，但是不会更新任何已命名的分支。（你可以认为这是在更新一个匿名分支。）



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdIknsiaJ60s2S1aYoPyn4qjkn9YepPUcpXosgNicGSKo2lEV7MmYL3bwQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



一旦此后你切换到别的分支，比如说 master，那么这个提交节点（可能）再也不会被引用到，然后就会被丢弃掉了。注意这个命令之后就不会有东西引用 2eecb。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdtUMpFj1s3iaVAHc8OdO1nQNpE1OFqibZca2gGhDib6GOgAvC1HUdJeUFg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





但是，如果你想保存这个状态，可以用命令 git checkout -b name 来创建一个新的分支。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdQ9sMJahbLwjLRUCcWBX9IX4TAPZDg4zYmCHmtKwdD1JO0K8IQ1678A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



#### **Reset**

Reset 命令把当前分支指向另一个位置，并且有选择的变动工作目录和索引。也用来在从历史仓库中复制文件到索引，而不动工作目录。



如果不给选项，那么当前分支指向到那个提交。如果用–hard 选项，那么工作目录也更新，如果用–soft 选项，那么都不变。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdg7cp5MbL5g78655RSGxzh9xLFapI79n5WGbicWSMSwA3zickCZlnslicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果没有给出提交点的版本号，那么默认用 HEAD。这样，分支指向不变，但是索引会回滚到最后一次提交，如果用–hard 选项，工作目录也同样。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdlpE3ic4w3pOznB3LDhric6FYMjPLiam2d9eytrmcKJ32f1wrYw41Q6YHw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果给了文件名（或者 - p 选项），那么工作效果和带文件名的 checkout 差不多，除了索引被更新。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdIkvt4DjfTJp02cNdxicsCPWLAxEVwWyicG0Vh0PG94prKJJMEjORdzZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)







#### **Merge**



Merge 命令把不同分支合并起来。合并前，索引必须和当前提交相同。如果另一个分支是当前提交的祖父节点，那么合并命令将什么也不做。另一种情况是如果当前提交是另一个分支的祖父节点，就导致 fast-forward 合并。指向只是简单的移动，并生成一个新的提交。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdxsnHyTf9U6NVM6iasdyop8ZFMDtINC7qYhzfpp84ECg8ArLPiauXX2iag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



否则就是一次真正的合并。默认把当前提交（ed489 如下所示）和另一个提交（33104）以及他们的共同祖父节点（b325c）进行一次三方合并 [4]。结果是先保存当前目录和索引，然后和父节点 33104 一起做一次新提交。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdDFSlUbw008ODVcP0qlj9FF1kpMV1ZsQSzX5BspvfkiajVaE2q428oibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



#### **Cherry Pick**



cherry-pick 命令 “复制” 一个提交节点并在当前分支做一次完全一样的新提交。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdlGk2GCEeqNdG4opRvmPcxglggpuYSJ2OibqKtrk6k3CHeia1Uc9ewOVw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



#### **Rebase**

衍合是合并命令的另一种选择。合并把两个父分支合并进行一次提交，提交历史不是线性的。衍合在当前分支上重演另一个分支的历史，提交历史是线性的。本质上，这是线性化的自动的 cherry-pick。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdNpPX70icDA8vR1DKM7B0HATDmibTGZoLVvtKzH6RVwabwddKRfa3wefQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



上面的命令都在 topic 分支中进行，而不是 master 分支，在 master 分支上重演，并且把分支指向新的节点。注意旧提交没有被引用，将被回收。



要限制回滚范围，使用–onto 选项。下面的命令在 master 分支上重演当前分支从 169a6 以来的最近几个提交，即 2c33a。



![图片](https://mmbiz.qpic.cn/mmbiz_png/A1HKVXsfHNnZfxjv4QgIc8lho4spoiacdEy3uLQL1VCFeOsGeC1uaVM6UPwiafbiaycrzpnWBujErcic7sH1SIYmcQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



同样有 git rebase –interactive 让你更方便的完成一些复杂操作，比如丢弃、重排、修改、合并提交。没有图片体现这些，细节看这里：git-rebase (1)[5]。



### 技术说明

文件内容并没有真正存储在索引（.git/index）或者提交对象中，而是以 blob 的形式分别存储在数据库中（.git/objects），并用 SHA-1 值来校验。索引文件用识别码列出相关的 blob 文件以及别的数据。对于提交来说，以树（tree）的形式存储，同样用对于的哈希值识别。树对应着工作目录中的文件夹，树中包含的 树或者 blob 对象对应着相应的子目录和文件。每次提交都存储下它的上一级树的识别码。



如果用 detached HEAD 提交，那么最后一次提交会被 the reflog for HEAD 引用。但是过一段时间就失效，最终被回收，与 git commit –amend 或者 git rebase 很像。



>  相关链接：
>
> 1. http://marklodato.github.io/visual-git-guide/index-zh-cn.html#merge
> 2. http://marklodato.github.io/visual-git-guide/index-zh-cn.html#rebase
> 3. http://marklodato.github.io/visual-git-guide/index-zh-cn.html#detached
> 4. http://en.wikipedia.org/wiki/Three-way_merge
> 5. http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html#_interactive_mode
>
> 
>
> 原文链接：http://marklodato.github.io/visual-git-guide/index-zh-cn.html







## [Git 从入门到精通，这一篇就够了](https://mp.weixin.qq.com/s/b8bQW2N5VC-qmGD4dTiwKQ)



### 简介

#### Git 是什么

Git 是一个开源的分布式版本控制系统。

#### 什么是版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

#### 什么是分布式版本控制系统

介绍分布式版本控制系统前，有必要先了解一下传统的集中式版本控制系统。

**集中化的版本控制系统**，诸如 CVS，Subversion 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

这么做最显而易见的缺点是中央服务器的单点故障。如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。要是中央服务器的磁盘发生故障，碰巧没做备份，或者备份不够及时，就会有丢失数据的风险。最坏的情况是彻底丢失整个项目的所有历史更改记录。

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbufvk6Wu4gUFIbsGamZ1UmHO0xpU821gkUDY10vY6Pcp6632R14LQSI4u1BI7rxAxQApMaic0kMEEMw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**分布式版本控制系统**的客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。因为每一次的提取操作，实际上都是一次对代码仓库的完整备份。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 为什么使用 Git

Git 是分布式的。这是 Git 和其它非分布式的版本控制系统，例如 svn，cvs 等，最核心的区别。分布式带来以下好处：

**工作时不需要联网**

首先，分布式版本控制系统根本没有 “中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件 A，你的同事也在他的电脑上改了文件 A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

**更加安全**

集中式版本控制系统，一旦中央服务器出了问题，所有人都无法工作。

分布式版本控制系统，每个人电脑中都有完整的版本库，所以某人的机器挂了，并不影响其它人。

### 安装

**Debian/Ubuntu 环境安装**

如果你使用的系统是 Debian/Ubuntu ， 安装命令为：

```bash
$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
> libz-dev libssl-dev
$ apt-get install git-core
$ git --version
git version 1.8.1.2
```

**Centos/RedHat 环境安装**

如果你使用的系统是 Centos/RedHat ，安装命令为：

```bash
$ yum install curl-devel expat-devel gettext-devel \
> openssl-devel zlib-devel
$ yum -y install git-core
$ git --version
git version 1.7.1
```

**Windows 环境安装**

在 *Git 官方下载地址*下载 exe 安装包。按照安装向导安装即可。

建议安装 Git Bash 这个 git 的命令行工具。

**Mac 环境安装**

在 *Git 官方下载地址*下载 mac 安装包。按照安装向导安装即可。

> https://git-scm.com/downloads

### 配置

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件：包含系统上每一个用户及他们仓库的通用配置。如果使用带有 `--system` 选项的 `git config` 时，它会从此文件读写配置变量。
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。可以传递 `--global` 选项让 Git 读写此文件。
3. 当前使用仓库的 Git 目录中的 `config` 文件（就是 `.git/config`）：针对该仓库。

每一个级别覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。

在 Windows 系统中，Git 会查找 `$HOME` 目录下（一般情况下是 `C:\Users\$USER`）的 `.gitconfig` 文件。Git 同样也会寻找 `/etc/gitconfig` 文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。

#### 用户信息

当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

很多 GUI 工具都会在第一次运行时帮助你配置这些信息。

#### .gitignore

`.gitignore` 文件可能从字面含义也不难猜出：这个文件里配置的文件或目录，会自动被 git 所忽略，不纳入版本控制。

在日常开发中，我们的项目经常会产生一些临时文件，如编译 Java 产生的 `*.class` 文件，又或是 IDE 自动生成的隐藏目录（Intellij 的 `.idea` 目录、Eclipse 的 `.settings` 目录等）等等。这些文件或目录实在没必要纳入版本管理。在这种场景下，你就需要用到 `.gitignore` 配置来过滤这些文件或目录。

配置的规则很简单，也没什么可说的，看几个例子，自然就明白了。

这里推荐一下 Github 的开源项目：https://github.com/github/gitignore

在这里，你可以找到很多常用的模板，如：Java、Nodejs、C++ 的 `.gitignore` 模板等等。

### 原理

个人认为，对于 Git 这个版本工具，再不了解原理的情况下，直接去学习命令行，可能会一头雾水。所以，本文特意将原理放在命令使用章节之前讲解。可以参考：[Git 原理入门解析](http://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247489338&idx=2&sn=6fdd8968003b8f59d43c09b72894e877&chksm=ebd62816dca1a1003dcefb6dae8296dd08924426fa6f12d09b8695922007fcb9bfa342c35bd1&scene=21#wechat_redirect)

#### 版本库

当你一个项目到本地或创建一个 git 项目，项目目录下会有一个隐藏的 `.git` 子目录。这个目录是 git 用来跟踪管理版本库的，千万不要手动修改。

#### 哈希值

Git 中所有数据在存储前都计算校验和，然后以校验和来引用。这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。这个功能建构在 Git 底层，是构成 Git 哲学不可或缺的部分。若你在传送过程中丢失信息或损坏文件，Git 就能发现。

Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。这是一个由 40 个十六进制字符（0-9 和 a-f）组成字符串，基于 Git 中文件的内容或目录结构计算出来。SHA-1 哈希看起来是这样：

```bash
24b9da6552252987aa493b52f8696cd6d3b00373
```

Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

#### 文件状态

在 GIt 中，你的文件可能会处于三种状态之一：

- **已修改（modified）** - 已修改表示修改了文件，但还没保存到数据库中。
- **已暂存（staged）** - 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- **已提交（committed）** - 已提交表示数据已经安全的保存在本地数据库中。

#### 工作区域

与文件状态对应的，不同状态的文件在 Git 中处于不同的工作区域。

- **工作区（working）** - 当你 `git clone` 一个项目到本地，相当于在本地克隆了项目的一个副本。工作区是对项目的某个版本独立提取出来的内容。这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

- **暂存区（staging）** - 暂存区是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。有时候也被称作`索引`，不过一般说法还是叫暂存区。

- **本地仓库（local）** - 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 本地仓库。

- **远程仓库（remote）** - 以上几个工作区都是在本地。为了让别人可以看到你的修改，你需要将你的更新推送到远程仓库。同理，如果你想同步别人的修改，你需要从远程仓库拉取更新。

  

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 命令

国外网友制作了一张 Git Cheat Sheet，总结很精炼，各位不妨收藏一下。

本节选择性介绍 git 中比较常用的命令行场景。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 创建仓库

克隆一个已创建的仓库：

```bash
# 通过 SSH
$ git clone ssh://user@domain.com/repo.git

#通过 HTTP
$ git clone http://domain.com/user/repo.git
```

创建一个新的本地仓库：

```bash
$ git init
```

#### 添加修改

添加修改到暂存区：

```bash
# 把指定文件添加到暂存区
$ git add xxx

# 把当前所有修改添加到暂存区
$ git add .

# 把所有修改添加到暂存区
$ git add -A
```

提交修改到本地仓库：

```bash
# 提交本地的所有修改
$ git commit -a

# 提交之前已标记的变化
$ git commit

# 附加消息提交
$ git commit -m 'commit message'
```

##### 储藏

有时，我们需要在同一个项目的不同分支上工作。当需要切换分支时，偏偏本地的工作还没有完成，此时，提交修改显得不严谨，但是不提交代码又无法切换分支。这时，你可以使用 `git stash` 将本地的修改内容作为草稿储藏起来。

官方称之为储藏，但我个人更喜欢称之为存草稿。

```bash
# 1. 将修改作为当前分支的草稿保存
$ git stash

# 2. 查看草稿列表
$ git stash list
stash@{0}: WIP on master: 6fae349 :memo: Writing docs.

# 3.1 删除草稿
$ git stash drop stash@{0}

# 3.2 读取草稿
$ git stash apply stash@{0}
```

#### 撤销修改

撤销本地修改：

```bash
# 移除缓存区的所有文件（i.e. 撤销上次git add）
$ git reset HEAD

# 将HEAD重置到上一次提交的版本，并将之后的修改标记为未添加到缓存区的修改
$ git reset <commit>

# 将HEAD重置到上一次提交的版本，并保留未提交的本地修改
$ git reset --keep <commit>

# 放弃工作目录下的所有修改
$ git reset --hard HEAD

# 将HEAD重置到指定的版本，并抛弃该版本之后的所有修改
$ git reset --hard <commit-hash>

# 用远端分支强制覆盖本地分支
$ git reset --hard <remote/branch> e.g., upstream/master, origin/my-feature

# 放弃某个文件的所有本地修改
$ git checkout HEAD <file>
```

删除添加`.gitignore` 文件前错误提交的文件：

```bash
$ git rm -r --cached .
$ git add .
$ git commit -m "remove xyz file"
```

撤销远程修改（创建一个新的提交，并回滚到指定版本）：

```bash
$ git revert <commit-hash>
```

彻底删除指定版本：

```bash
# 执行下面命令后，commit-hash 提交后的记录都会被彻底删除，使用需谨慎
$ git reset --hard <commit-hash>
$ git push -f
```

#### 更新与推送

更新：

```bash
# 下载远程端版本，但不合并到HEAD中
$ git fetch <remote>

# 将远程端版本合并到本地版本中
$ git pull origin master

# 以rebase方式将远端分支与本地合并
$ git pull --rebase <remote> <branch>
```

推送：

```bash
# 将本地版本推送到远程端
$ git push remote <remote> <branch>

# 删除远程端分支
$ git push <remote> :<branch> (since Git v1.5.0)
$ git push <remote> --delete <branch> (since Git v1.7.0)

# 发布标签
$ git push --tags
```

#### 查看信息

显示工作路径下已修改的文件：

```
$ git status
```

显示与上次提交版本文件的不同：

```
$ git diff
```

显示提交历史：

```
# 从最新提交开始，显示所有的提交记录（显示hash， 作者信息，提交的标题和时间）
$ git log

# 显示某个用户的所有提交
$ git log --author="username"

# 显示某个文件的所有修改
$ git log -p <file>
```

显示搜索内容：

```
# 从当前目录的所有文件中查找文本内容
$ git grep "Hello"

# 在某一版本中搜索文本
$ git grep "Hello" v2.5
```

#### 分支

增删查分支：

```
# 列出所有的分支
$ git branch

# 列出所有的远端分支
$ git branch -r

# 基于当前分支创建新分支
$ git branch <new-branch>

# 基于远程分支创建新的可追溯的分支
$ git branch --track <new-branch> <remote-branch>

# 删除本地分支
$ git branch -d <branch>

# 强制删除本地分支，将会丢失未合并的修改
$ git branch -D <branch>
```

切换分支：

```
# 切换分支
$ git checkout <branch>

# 创建并切换到新分支
$ git checkout -b <branch>
```

#### 标签

```
# 给当前版本打标签
$ git tag <tag-name>

# 给当前版本打标签并附加消息
$ git tag -a <tag-name>
```

#### 合并与重置

> merge 与 rebase 虽然是 git 常用功能，但是强烈建议不要使用 git 命令来完成这项工作。
>
> 因为如果出现代码冲突，在没有代码比对工具的情况下，实在太艰难了。
>
> 你可以考虑使用各种 Git GUI 工具。

合并：

```
# 将分支合并到当前HEAD中
$ git merge <branch>
```

重置：

```
# 将当前HEAD版本重置到分支中，请勿重置已发布的提交
$ git rebase <branch>
```

#### Github

Github 作为最著名的代码开源协作社区，在程序员圈想必无人不知，无人不晓。[关于 Git 和 Github 你不知道的十件事](http://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247489949&idx=2&sn=99b2d900eed1e313ba165c75d07348bd&chksm=ebd626b1dca1afa7affee39ed37d094a62a24303eb08d364fa060fa3f29dc1780409fc33f022&scene=21#wechat_redirect)

这里不赘述 Github 的用法，确实有不会用的新手同学，可以参考官方教程：

> https://guides.github.com/

#### clone 方式

Git 支持三种协议：HTTPS / SSH / GIT

而 Github 上支持 HTTPS 和 SSH。

HTTPS 这种方式要求你每次 push 时都要输入用户名、密码，有些繁琐。

而 SSH 要求你本地生成证书，然后在你的 Github 账户中注册。第一次配置麻烦是麻烦了点，但是以后就免去了每次 push 需要输入用户名、密码的繁琐。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以下介绍一下，如何生成证书，以及在 Github 中注册。

##### 生成 SSH 公钥

如前所述，许多 Git 服务器都使用 SSH 公钥进行认证。为了向 Git 服务器提供 SSH 公钥，如果某系统用户尚未拥有密钥，必须事先为其生成一份。这个过程在所有操作系统上都是相似的。首先，你需要确认自己是否已经拥有密钥。默认情况下，用户的 SSH 密钥存储在其 `\~/.ssh` 目录下。进入该目录并列出其中内容，你便可以快速确认自己是否已拥有密钥：

```
$ cd ~/.ssh
$ ls
authorized_keys2  id_dsa       known_hosts
config            id_dsa.pub
```

我们需要寻找一对以 `id_dsa` 或 `id_rsa` 命名的文件，其中一个带有 `.pub` 扩展名。 `.pub` 文件是你的公钥，另一个则是私钥。如果找不到这样的文件（或者根本没有 `.ssh` 目录），你可以通过运行 `ssh-keygen` 程序来创建它们。在 Linux/Mac 系统中，`ssh-keygen` 随 SSH 软件包提供；在 Windows 上，该程序包含于 MSysGit 软件包中。

```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/schacon/.ssh/id_rsa):
Created directory '/home/schacon/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/schacon/.ssh/id_rsa.
Your public key has been saved in /home/schacon/.ssh/id_rsa.pub.
The key fingerprint is:
d0:82:24:8e:d7:f1:bb:9b:33:53:96:93:49:da:9b:e3 schacon@mylaptop.local
```

首先 `ssh-keygen` 会确认密钥的存储位置（默认是 `.ssh/id_rsa`），然后它会要求你输入两次密钥口令。如果你不想在使用密钥时输入口令，将其留空即可。

现在，进行了上述操作的用户需要将各自的公钥发送给任意一个 Git 服务器管理员（假设服务器正在使用基于公钥的 SSH 验证设置）。他们所要做的就是复制各自的 `.pub` 文件内容，并将其通过邮件发送。公钥看起来是这样的：

```
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+M7Q== schacon@mylaptop.local
```

在你的 Github 账户中，依次点击 **Settings** > **SSH and GPG keys** > **New SSH key**

然后，将上面生成的公钥内容粘贴到 `Key` 编辑框并保存。至此大功告成。

后面，你在克隆你的 Github 项目时使用 SSH 方式即可。

如果觉得我的讲解还不够细致，可以参考：https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

### 最佳实践 Git Flow

> 详细内容，可以参考这篇文章：Git 在团队中的最佳实践 -- 如何正确使用 Git Flow

> https://www.cnblogs.com/cnblogsfans/p/5075073.html

Git 在实际开发中的最佳实践策略 Git Flow 可以归纳为以下：

- **`master` 分支** - 也就是我们经常使用的主线分支，这个分支是最近发布到生产环境的代码，这个分支只能从其他分支合并，不能在这个分支直接修改。
- **`develop` 分支** - 这个分支是我们的主开发分支，包含所有要发布到下一个 release 的代码，这个分支主要是从其他分支合并代码过来，比如 feature 分支。
- **`feature` 分支** - 这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回 develop 分支进入下一个 release。
- **`release` 分支** - 当你需要一个发布一个新 release 的时候，我们基于 Develop 分支创建一个 release 分支，完成 release 后，我们合并到 master 和 develop 分支。
- **`hotfix` 分支** - 当我们在 master 发现新的 Bug 时候，我们需要创建一个 hotfix, 完成 hotfix 后，我们合并回 master 和 develop 分支，所以 hotfix 的改动会进入下一个 release

### 常见问题

#### 编辑提交 (editting commits)

##### 我刚才提交了什么

如果你用 `git commit -a` 提交了一次变化 (changes)，而你又不确定到底这次提交了哪些内容。你就可以用下面的命令显示当前 `HEAD` 上的最近一次的提交 (commit):

```
(master)$ git show
```

或者

```
$ git log -n1 -p
```

##### 我的提交信息 (commit message) 写错了

如果你的提交信息 (commit message) 写错了且这次提交 (commit) 还没有推 (push), 你可以通过下面的方法来修改提交信息 (commit message):

```
$ git commit --amend
```

这会打开你的默认编辑器，在这里你可以编辑信息。另一方面，你也可以用一条命令一次完成:

```
$ git commit --amend -m 'xxxxxxx'
```

如果你已经推 (push) 了这次提交 (commit), 你可以修改这次提交 (commit) 然后强推 (force push), 但是不推荐这么做。

##### 我提交 (commit) 里的用户名和邮箱不对

如果这只是单个提交 (commit)，修改它：

```
$ git commit --amend --author "New Authorname <authoremail@mydomain.com>"
```

如果你需要修改所有历史，参考 'git filter-branch' 的指南页.

##### 我想从一个提交 (commit) 里移除一个文件

通过下面的方法，从一个提交 (commit) 里移除一个文件:

```
$ git checkout HEAD^ myfile
$ git add -A
$ git commit --amend
```

这将非常有用，当你有一个开放的补丁 (open patch)，你往上面提交了一个不必要的文件，你需要强推 (force push) 去更新这个远程补丁。

##### 我想删除我的的最后一次提交 (commit)

如果你需要删除推了的提交 (pushed commits)，你可以使用下面的方法。可是，这会不可逆的改变你的历史，也会搞乱那些已经从该仓库拉取 (pulled) 了的人的历史。简而言之，如果你不是很确定，千万不要这么做。

```
$ git reset HEAD^ --hard
$ git push -f [remote] [branch]
```

如果你还没有推到远程，把 Git 重置 (reset) 到你最后一次提交前的状态就可以了 (同时保存暂存的变化):

```
(my-branch*)$ git reset --soft HEAD@{1}
```

这只能在没有推送之前有用。如果你已经推了，唯一安全能做的是 `git revert SHAofBadCommit`， 那会创建一个新的提交 (commit) 用于撤消前一个提交的所有变化 (changes)；或者，如果你推的这个分支是 rebase-safe 的 (例如：其它开发者不会从这个分支拉), 只需要使用 `git push -f`；更多，请参考 *the above section*。

> https://juejin.im/post/5c8296f85188257e3941b2d4

##### 删除任意提交 (commit)

同样的警告：不到万不得已的时候不要这么做.

```
$ git rebase --onto SHA1_OF_BAD_COMMIT^ SHA1_OF_BAD_COMMIT
$ git push -f [remote] [branch]
```

或者做一个 交互式 rebase 删除那些你想要删除的提交 (commit) 里所对应的行。

##### 我尝试推一个修正后的提交 (amended commit) 到远程，但是报错：

```
To https://github.com/yourusername/repo.git
! [rejected]        mybranch -> mybranch (non-fast-forward)
error: failed to push some refs to 'https://github.com/tanay1337/webmaker.org.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

注意，rebasing (见下面) 和修正 (amending) 会用一个**新的提交 (commit) 代替旧的** , 所以如果之前你已经往远程仓库上推过一次修正前的提交 (commit)，那你现在就必须强推 (force push) (`-f`)。注意 – *总是* 确保你指明一个分支！

```
(my-branch)$ git push origin mybranch -f
```

一般来说， **要避免强推**. 最好是创建和推 (push) 一个新的提交 (commit)，而不是强推一个修正后的提交。后者会使那些与该分支或该分支的子分支工作的开发者，在源历史中产生冲突。

##### 我意外的做了一次硬重置 (hard reset)，我想找回我的内容

如果你意外的做了 `git reset --hard`, 你通常能找回你的提交 (commit), 因为 Git 对每件事都会有日志，且都会保存几天。

```
(master)$ git reflog
```

你将会看到一个你过去提交 (commit) 的列表，和一个重置的提交。选择你想要回到的提交 (commit) 的 SHA，再重置一次:

```
(master)$ git reset --hard SHA1234
```

这样就完成了。

#### 暂存 (Staging)

##### 我需要把暂存的内容添加到上一次的提交 (commit)

```
(my-branch*)$ git commit --amend
```

##### 我想要暂存一个新文件的一部分，而不是这个文件的全部

一般来说，如果你想暂存一个文件的一部分，你可这样做:

```
$ git add --patch filename.x
```

`-p` 简写。这会打开交互模式， 你将能够用 `s` 选项来分隔提交 (commit)；然而，如果这个文件是新的，会没有这个选择， 添加一个新文件时，这样做:

```
$ git add -N filename.x
```

然后，你需要用 `e` 选项来手动选择需要添加的行，执行 `git diff --cached` 将会显示哪些行暂存了哪些行只是保存在本地了。

##### 我想把在一个文件里的变化 (changes) 加到两个提交 (commit) 里

`git add` 会把整个文件加入到一个提交. `git add -p` 允许交互式的选择你想要提交的部分.

##### 我想把暂存的内容变成未暂存，把未暂存的内容暂存起来

这个有点困难， 我能想到的最好的方法是先 stash 未暂存的内容， 然后重置 (reset)，再 pop 第一步 stashed 的内容，最后再 add 它们。

```
$ git stash -k
$ git reset --hard
$ git stash pop
$ git add -A
```

#### 未暂存 (Unstaged) 的内容

##### 我想把未暂存的内容移动到一个新分支

```
$ git checkout -b my-branch
```

##### 我想把未暂存的内容移动到另一个已存在的分支

```
$ git stash
$ git checkout my-branch
$ git stash pop
```

##### 我想丢弃本地未提交的变化 (uncommitted changes)

如果你只是想重置源 (origin) 和你本地 (local) 之间的一些提交 (commit)，你可以：

```
## one commit
(my-branch)$ git reset --hard HEAD^
## two commits
(my-branch)$ git reset --hard HEAD^^
## four commits
(my-branch)$ git reset --hard HEAD~4
## or
(master)$ git checkout -f
```

重置某个特殊的文件，你可以用文件名做为参数:

```
$ git reset filename
```

##### 我想丢弃某些未暂存的内容

如果你想丢弃工作拷贝中的一部分内容，而不是全部。

签出 (checkout) 不需要的内容，保留需要的。

```
$ git checkout -p
## Answer y to all of the snippets you want to drop
```

另外一个方法是使用 `stash`， Stash 所有要保留下的内容，重置工作拷贝，重新应用保留的部分。

```
$ git stash -p
## Select all of the snippets you want to save
$ git reset --hard
$ git stash pop
```

或者，stash 你不需要的部分，然后 stash drop。

```
$ git stash -p
## Select all of the snippets you don't want to save
$ git stash drop
```

#### 分支 (Branches)

##### 我从错误的分支拉取了内容，或把内容拉取到了错误的分支

这是另外一种使用 `git reflog` 情况，找到在这次错误拉 (pull) 之前 HEAD 的指向。

```
(master)$ git reflog
ab7555f HEAD@{0}: pull origin wrong-branch: Fast-forward
c5bc55a HEAD@{1}: checkout: checkout message goes here
```

重置分支到你所需的提交 (desired commit):

```
$ git reset --hard c5bc55a
```

完成。

##### 我想扔掉本地的提交 (commit)，以便我的分支与远程的保持一致

先确认你没有推 (push) 你的内容到远程。

`git status` 会显示你领先 (ahead) 源 (origin) 多少个提交:

```
(my-branch)$ git status
## On branch my-branch
## Your branch is ahead of 'origin/my-branch' by 2 commits.
##   (use "git push" to publish your local commits)
#
```

一种方法是:

```
(master)$ git reset --hard origin/my-branch
```

##### 我需要提交到一个新分支，但错误的提交到了 master

在 master 下创建一个新分支，不切换到新分支，仍在 master 下:

```
(master)$ git branch my-branch
```

把 master 分支重置到前一个提交:

```
(master)$ git reset --hard HEAD^
```

`HEAD^` 是 `HEAD^1` 的简写，你可以通过指定要设置的 `HEAD` 来进一步重置。

或者，如果你不想使用 `HEAD^`, 找到你想重置到的提交 (commit) 的 hash (`git log` 能够完成)， 然后重置到这个 hash。使用 `git push` 同步内容到远程。

例如，master 分支想重置到的提交的 hash 为 `a13b85e`:

```
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

签出 (checkout) 刚才新建的分支继续工作:

```
(master)$ git checkout my-branch
```

##### 我想保留来自另外一个 ref-ish 的整个文件

假设你正在做一个原型方案 (原文为 working spike (see note)), 有成百的内容，每个都工作得很好。现在，你提交到了一个分支，保存工作内容:

```
(solution)$ git add -A && git commit -m "Adding all changes from this spike into one big commit."
```

当你想要把它放到一个分支里 (可能是 `feature`, 或者 `develop`), 你关心是保持整个文件的完整，你想要一个大的提交分隔成比较小。

假设你有:

- 分支 `solution`, 拥有原型方案， 领先 `develop` 分支。
- 分支 `develop`, 在这里你应用原型方案的一些内容。

我去可以通过把内容拿到你的分支里，来解决这个问题:

```bash
(develop)$ git checkout solution -- file1.txt
```

这会把这个文件内容从分支 `solution` 拿到分支 `develop` 里来:

```bash
## On branch develop
## Your branch is up-to-date with 'origin/develop'.
## Changes to be committed:
##  (use "git reset HEAD <file>..." to unstage)
#
##        modified:   file1.txt
```

然后，正常提交。

Note: Spike solutions are made to analyze or solve the problem. These solutions are used for estimation and discarded once everyone gets clear visualization of the problem. ~ Wikipedia.

##### 我把几个提交 (commit) 提交到了同一个分支，而这些提交应该分布在不同的分支里

假设你有一个 `master` 分支， 执行 `git log`, 你看到你做过两次提交:

```bash
(master)$ git log

commit e3851e817c451cc36f2e6f3049db528415e3c114
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:27 2014 -0400

    Bug #21 - Added CSRF protection

commit 5ea51731d150f7ddc4a365437931cd8be3bf3131
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:12 2014 -0400

    Bug #14 - Fixed spacing on title

commit a13b85e984171c6e2a1729bb061994525f626d14
Author: Aki Rose <akirose@example.com>
Date:   Tue Jul 21 01:12:48 2014 -0400

    First commit
```

让我们用提交 hash (commit hash) 标记 bug (`e3851e8` for #21, `5ea5173` for #14).

首先，我们把 `master` 分支重置到正确的提交 (`a13b85e`):

```bash
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

现在，我们对 bug #21 创建一个新的分支:

```bash
(master)$ git checkout -b 21
(21)$
```

接着，我们用 *cherry-pick* 把对 bug #21 的提交放入当前分支。这意味着我们将应用 (apply) 这个提交 (commit)，仅仅这一个提交 (commit)，直接在 HEAD 上面。

```bash
(21)$ git cherry-pick e3851e8
```

这时候，这里可能会产生冲突， 参见交互式 rebasing 章 **冲突节** 解决冲突.

再者， 我们为 bug #14 创建一个新的分支，也基于 `master` 分支

```bash
(21)$ git checkout master
(master)$ git checkout -b 14
(14)$
```

最后，为 bug #14 执行 `cherry-pick`:

```bash
(14)$ git cherry-pick 5ea5173
```

##### 我想删除上游 (upstream) 分支被删除了的本地分支

一旦你在 github 上面合并 (merge) 了一个 pull request, 你就可以删除你 fork 里被合并的分支。如果你不准备继续在这个分支里工作，删除这个分支的本地拷贝会更干净，使你不会陷入工作分支和一堆陈旧分支的混乱之中。

```bash
$ git fetch -p
```

##### 我不小心删除了我的分支

如果你定期推送到远程，多数情况下应该是安全的，但有些时候还是可能删除了还没有推到远程的分支。让我们先创建一个分支和一个新的文件:

```bash
(master)$ git checkout -b my-branch
(my-branch)$ git branch
(my-branch)$ touch foo.txt
(my-branch)$ ls
README.md foo.txt
```

添加文件并做一次提交

```bash
(my-branch)$ git add .
(my-branch)$ git commit -m 'foo.txt added'
(my-branch)$ foo.txt added
 1 files changed, 1 insertions(+)
 create mode 100644 foo.txt
(my-branch)$ git log

commit 4e3cd85a670ced7cc17a2b5d8d3d809ac88d5012
Author: siemiatj <siemiatj@example.com>
Date:   Wed Jul 30 00:34:10 2014 +0200

    foo.txt added

commit 69204cdf0acbab201619d95ad8295928e7f411d5
Author: Kate Hudson <katehudson@example.com>
Date:   Tue Jul 29 13:14:46 2014 -0400

    Fixes #6: Force pushing after amending commits
```

现在我们切回到主 (master) 分支，‘不小心的’删除 `my-branch` 分支

```bash
(my-branch)$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
(master)$ git branch -D my-branch
Deleted branch my-branch (was 4e3cd85).
(master)$ echo oh noes, deleted my branch!
oh noes, deleted my branch!
```

在这时候你应该想起了 `reflog`, 一个升级版的日志，它存储了仓库 (repo) 里面所有动作的历史。

```bash
(master)$ git reflog
69204cd HEAD@{0}: checkout: moving from my-branch to master
4e3cd85 HEAD@{1}: commit: foo.txt added
69204cd HEAD@{2}: checkout: moving from master to my-branch
```

正如你所见，我们有一个来自删除分支的提交 hash (commit hash)，接下来看看是否能恢复删除了的分支。

```bash
(master)$ git checkout -b my-branch-help
Switched to a new branch 'my-branch-help'
(my-branch-help)$ git reset --hard 4e3cd85
HEAD is now at 4e3cd85 foo.txt added
(my-branch-help)$ ls
README.md foo.txt
```

看！我们把删除的文件找回来了。Git 的 `reflog` 在 rebasing 出错的时候也是同样有用的。

##### 我想删除一个分支

删除一个远程分支:

```bash
(master)$ git push origin --delete my-branch
```

你也可以:

```bash
(master)$ git push origin :my-branch
```

删除一个本地分支:

```bash
(master)$ git branch -D my-branch
```

##### 我想从别人正在工作的远程分支签出 (checkout) 一个分支

首先，从远程拉取 (fetch) 所有分支:

```bash
(master)$ git fetch --all
```

假设你想要从远程的 `daves` 分支签出到本地的 `daves`

```bash
(master)$ git checkout --track origin/daves
Branch daves set up to track remote branch daves from origin.
Switched to a new branch 'daves'
```

(`--track` 是 `git checkout -b [branch] [remotename]/[branch]` 的简写)

这样就得到了一个 `daves` 分支的本地拷贝，任何推过 (pushed) 的更新，远程都能看到.

#### Rebasing 和合并 (Merging)

##### 我想撤销 rebase/merge

你可以合并 (merge) 或 rebase 了一个错误的分支，或者完成不了一个进行中的 rebase/merge。Git 在进行危险操作的时候会把原始的 HEAD 保存在一个叫 ORIG_HEAD 的变量里，所以要把分支恢复到 rebase/merge 前的状态是很容易的。

```bash
(my-branch)$ git reset --hard ORIG_HEAD
```

##### 我已经 rebase 过，但是我不想强推 (force push)

不幸的是，如果你想把这些变化 (changes) 反应到远程分支上，你就必须得强推 (force push)。是因你快进 (Fast forward) 了提交，改变了 Git 历史，远程分支不会接受变化 (changes)，除非强推 (force push)。这就是许多人使用 merge 工作流，而不是 rebasing 工作流的主要原因之一， 开发者的强推 (force push) 会使大的团队陷入麻烦。使用时需要注意，一种安全使用 rebase 的方法是，不要把你的变化 (changes) 反映到远程分支上，而是按下面的做:

```bash
(master)$ git checkout my-branch
(my-branch)$ git rebase -i master
(my-branch)$ git checkout master
(master)$ git merge --ff-only my-branch
```

更多，参见 *this SO thread.*

> http://stackoverflow.com/questions/11058312/how-can-i-use-git-rebase-without-requiring-a-forced-push

##### 我需要组合 (combine) 几个提交 (commit)

假设你的工作分支将会做对于 `master` 的 pull-request。一般情况下你不关心提交 (commit) 的时间戳，只想组合 *所有* 提交 (commit) 到一个单独的里面，然后重置 (reset) 重提交 (recommit)。确保主 (master) 分支是最新的和你的变化都已经提交了，然后:

```bash
(my-branch)$ git reset --soft master
(my-branch)$ git commit -am "New awesome feature"
```

如果你想要更多的控制，想要保留时间戳，你需要做交互式 rebase (interactive rebase):

```bash
(my-branch)$ git rebase -i master
```

如果没有相对的其它分支， 你将不得不相对自己的 `HEAD` 进行 rebase。例如：你想组合最近的两次提交 (commit), 你将相对于 `HEAD\~2` 进行 rebase， 组合最近 3 次提交 (commit), 相对于 `HEAD\~3`, 等等。

```bash
(master)$ git rebase -i HEAD~2
```

在你执行了交互式 rebase 的命令 (interactive rebase command) 后，你将在你的编辑器里看到类似下面的内容:

```bash
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
pick b729ad5 fixup
pick e3851e8 another fix

## Rebase 8074d12..b729ad5 onto 8074d12
#
## Commands:
##  p, pick = use commit
##  r, reword = use commit, but edit the commit message
##  e, edit = use commit, but stop for amending
##  s, squash = use commit, but meld into previous commit
##  f, fixup = like "squash", but discard this commit's log message
##  x, exec = run command (the rest of the line) using shell
#
## These lines can be re-ordered; they are executed from top to bottom.
#
## If you remove a line here THAT COMMIT WILL BE LOST.
#
## However, if you remove everything, the rebase will be aborted.
#
## Note that empty commits are commented out
```

所有以 `#` 开头的行都是注释，不会影响 rebase.

然后，你可以用任何上面命令列表的命令替换 `pick`, 你也可以通过删除对应的行来删除一个提交 (commit)。

例如，如果你想 **单独保留最旧 (first) 的提交 (commit), 组合所有剩下的到第二个里面** , 你就应该编辑第二个提交 (commit) 后面的每个提交 (commit) 前的单词为 `f`:

```bash
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
f b729ad5 fixup
f e3851e8 another fix
```

如果你想组合这些提交 (commit) **并重命名这个提交 (commit)**, 你应该在第二个提交 (commit) 旁边添加一个 `r`，或者更简单的用 `s` 替代 `f`:

```bash
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
s b729ad5 fixup
s e3851e8 another fix
```

你可以在接下来弹出的文本提示框里重命名提交 (commit)。

```
Newer, awesomer features

## Please enter the commit message for your changes. Lines starting
## with '#' will be ignored, and an empty message aborts the commit.
## rebase in progress; onto 8074d12
## You are currently editing a commit while rebasing branch 'master' on '8074d12'.
#
## Changes to be committed:
#    modified:   README.md
#
```

如果成功了，你应该看到类似下面的内容:

```
(master)$ Successfully rebased and updated refs/heads/master.
```

##### 安全合并 (merging) 策略

`--no-commit` 执行合并 (merge) 但不自动提交，给用户在做提交前检查和修改的机会。 `no-ff` 会为特性分支 (feature branch) 的存在过留下证据，保持项目历史一致。

```
(master)$ git merge --no-ff --no-commit my-branch
```

##### 我需要将一个分支合并成一个提交 (commit)

```
(master)$ git merge --squash my-branch
```

##### 我只想组合 (combine) 未推的提交 (unpushed commit)

有时候，在将数据推向上游之前，你有几个正在进行的工作提交 (commit)。这时候不希望把已经推 (push) 过的组合进来，因为其他人可能已经有提交 (commit) 引用它们了。

```
(master)$ git rebase -i @{u}
```

这会产生一次交互式的 rebase (interactive rebase), 只会列出没有推 (push) 的提交 (commit)， 在这个列表时进行 reorder/fix/squash 都是安全的。

##### 检查是否分支上的所有提交 (commit) 都合并 (merge) 过了

检查一个分支上的所有提交 (commit) 是否都已经合并 (merge) 到了其它分支，你应该在这些分支的 head (或任何 commits) 之间做一次 diff:

```
(master)$ git log --graph --left-right --cherry-pick --oneline HEAD...feature/120-on-scroll
```

这会告诉你在一个分支里有而另一个分支没有的所有提交 (commit), 和分支之间不共享的提交 (commit) 的列表。另一个做法可以是:

```
(master)$ git log master ^feature/120-on-scroll --no-merges
```

##### 交互式 rebase (interactive rebase) 可能出现的问题

###### 这个 rebase 编辑屏幕出现 'noop'

如果你看到的是这样:

```
noop
```

这意味着你 rebase 的分支和当前分支在同一个提交 (commit) 上，或者 *领先 (ahead)* 当前分支。你可以尝试:

- 检查确保主 (master) 分支没有问题
- rebase `HEAD~2` 或者更早

###### 有冲突的情况

如果你不能成功的完成 rebase, 你可能必须要解决冲突。

首先执行 `git status` 找出哪些文件有冲突:

```
(my-branch)$ git status
On branch my-branch
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   README.md
```

在这个例子里面， `README.md` 有冲突。打开这个文件找到类似下面的内容:

```
   <<<<<<< HEAD
   some code
   =========
   some code
   >>>>>>> new-commit
```

你需要解决新提交的代码 (示例里，从中间 `==` 线到 `new-commit` 的地方) 与 `HEAD` 之间不一样的地方.

有时候这些合并非常复杂，你应该使用可视化的差异编辑器 (visual diff editor):

```
(master*)$ git mergetool -t opendiff
```

在你解决完所有冲突和测试过后， `git add` 变化了的 (changed) 文件，然后用 `git rebase --continue` 继续 rebase。

```
(my-branch)$ git add README.md
(my-branch)$ git rebase --continue
```

如果在解决完所有的冲突过后，得到了与提交前一样的结果，可以执行 `git rebase --skip`。

任何时候你想结束整个 rebase 过程，回来 rebase 前的分支状态，你可以做:

```
(my-branch)$ git rebase --abort
```

#### 杂项 (Miscellaneous Objects)

##### 克隆所有子模块

```
$ git clone --recursive git://github.com/foo/bar.git
```

如果已经克隆了:

```
$ git submodule update --init --recursive
```

##### 删除标签 (tag)

```
$ git tag -d <tag_name>
$ git push <remote> :refs/tags/<tag_name>
```

##### 恢复已删除标签 (tag)

如果你想恢复一个已删除标签 (tag), 可以按照下面的步骤：首先，需要找到无法访问的标签 (unreachable tag):

```
$ git fsck --unreachable | grep tag
```

记下这个标签 (tag) 的 hash，然后用 Git 的 update-ref:

```
$ git update-ref refs/tags/<tag_name> <hash>
```

这时你的标签 (tag) 应该已经恢复了。

##### 已删除补丁 (patch)

如果某人在 GitHub 上给你发了一个 pull request, 但是然后他删除了他自己的原始 fork, 你将没法克隆他们的提交 (commit) 或使用 `git am`。在这种情况下，最好手动的查看他们的提交 (commit)，并把它们拷贝到一个本地新分支，然后做提交。

做完提交后，再修改作者，参见变更作者。然后，应用变化，再发起一个新的 pull request。

#### 跟踪文件 (Tracking Files)

##### 我只想改变一个文件名字的大小写，而不修改内容

```
(master)$ git mv --force myfile MyFile
```

##### 我想从 Git 删除一个文件，但保留该文件

```
(master)$ git rm --cached log.txt
```

#### 配置 (Configuration)

##### 我想给一些 Git 命令添加别名 (alias)

在 OS X 和 Linux 下，你的 Git 的配置文件储存在 `~/.gitconfig`。我在 `[alias]` 部分添加了一些快捷别名 (和一些我容易拼写错误的)，如下:

```
[alias]
    a = add
    amend = commit --amend
    c = commit
    ca = commit --amend
    ci = commit -a
    co = checkout
    d = diff
    dc = diff --changed
    ds = diff --staged
    f = fetch
    loll = log --graph --decorate --pretty=oneline --abbrev-commit
    m = merge
    one = log --pretty=oneline
    outstanding = rebase -i @{u}
    s = status
    unpushed = log @{u}
    wc = whatchanged
    wip = rebase -i @{u}
    zap = fetch -p
```

##### 我想缓存一个仓库 (repository) 的用户名和密码

你可能有一个仓库需要授权，这时你可以缓存用户名和密码，而不用每次推 / 拉 (push/pull) 的时候都输入，Credential helper 能帮你。

```
$ git config --global credential.helper cache
## Set git to use the credential memory cache
$ git config --global credential.helper 'cache --timeout=3600'
## Set the cache to timeout after 1 hour (setting is in seconds)
```

#### 我不知道我做错了些什么

你把事情搞砸了：你 `重置(reset)` 了一些东西，或者你合并了错误的分支，亦或你强推了后找不到你自己的提交 (commit) 了。有些时候，你一直都做得很好，但你想回到以前的某个状态。

这就是 `git reflog` 的目的， `reflog` 记录对分支顶端 (the tip of a branch) 的任何改变，即使那个顶端没有被任何分支或标签引用。基本上，每次 HEAD 的改变，一条新的记录就会增加到 `reflog`。遗憾的是，这只对本地分支起作用，且它只跟踪动作 (例如，不会跟踪一个没有被记录的文件的任何改变)。

```
(master)$ git reflog
0a2e358 HEAD@{0}: reset: moving to HEAD\~2
0254ea7 HEAD@{1}: checkout: moving from 2.2 to master
c10f740 HEAD@{2}: checkout: moving from master to 2.2
```

上面的 reflog 展示了从 master 分支签出 (checkout) 到 2.2 分支，然后再签回。那里，还有一个硬重置 (hard reset) 到一个较旧的提交。最新的动作出现在最上面以 `HEAD@{0}` 标识.

如果事实证明你不小心回移 (move back) 了提交 (commit), reflog 会包含你不小心回移前 master 上指向的提交 (0254ea7)。

```
$ git reset --hard 0254ea7
```

然后使用 git reset 就可以把 master 改回到之前的 commit，这提供了一个在历史被意外更改情况下的安全网。

### 小结

最后，放一张总结的脑图总结一下以上的知识点。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbufvk6Wu4gUFIbsGamZ1UmHOO3hhdZbTvgn48niaC2xQGjwyacjUkJ8j1YkvrjibF1nnEPhWtoiahiapicQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 参考资料

- **官方资源**

- - Git 官网：https://git-scm.com/
  - Git Github：https://github.com/git/git

- **模板**

- - gitignore 模板 - .gitignore 文件模板
  - gitattributes 模板 - .gitattributes 文件模板
  - github-cheat-sheet - git 命令简略图表

- **Git 书**

- Git 官方推荐教程 - Scott Chacon 的 Git 书。

- **Git 工具**

- - guis - Git 官网展示的客户端工具列表。
  - gogs - 极易搭建的自助 Git 服务。
  - gitflow - 应用 fit-flow 模型的工具。
  - firstaidgit.io 一个可搜索的最常被问到的 Git 的问题
  - git-extra-commands - 一堆有用的额外的 Git 脚本
  - git-extras - GIT 工具集 -- repo summary, repl, changelog population, author commit percentages and more
  - git-fire - git-fire 是一个 Git 插件，用于帮助在紧急情况下添加所有当前文件，做提交 (committing), 和推 (push) 到一个新分支 (阻止合并冲突)。
  - git-tips - Git 小提示
  - git-town - 通用，高级 Git 工作流支持！http://www.git-town.com

- **GUI 客户端 (GUI Clients)**

- - GitKraken - 豪华的 Git 客户端 Windows, Mac & Linux
  - git-cola - 另外一个 Git 客户端 Windows & OS X
  - GitUp - 一个新的 Git 客户端，在处理 Git 的复杂性上有自己的特点
  - gitx-dev - 图形化的 Git 客户端 OS X
  - Source Tree - 免费的图形化 Git 客户端 Windows & OS X
  - Tower - 图形化 Git 客户端 OS X (付费)

- **git cheat sheet**

- github-git-cheat-sheet：https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf











# 数据结构与算法

## 排序算法

### 冒泡







## 选择排序







## 数据结构









# C语言

## errno全局变量

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



## 与内存相关函数



### memcpy函数



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
>    const char src[50] = "http://www.runoob.com";
>    char dest[50];
>  
>    memcpy(dest, src, strlen(src)+1);
>    printf("dest = %s\n", dest);
>    
>    return(0);
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
>   char *s="http://www.runoob.com";
>   char d[20];
>   memcpy(d, s+11, 6);// 从第 11 个字符 (r) 开始复制，连续复制 6 个字符 (runoob)
>   // 或者 memcpy (d, s+11*sizeof (char), 6*sizeof (char));
>   d[6]='\0';
>   printf("%s", d);
>   return 0;
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
>   char src[] = "***";
>   char ddest[] = "abcdefg";
>   printf(" 使用 memcpy 前: % s\n", dest);
>   memcpy(dest, src, strlen(src));
>   printf(" 使用 memcpy 后: % s\n", dest);
>   return 0;
> }
> //让我们编译并运行上面的程序，这将产生以下结果：
> 使用 memcpy 前: abcdefg
> 使用 memcpy 后: ***defg
> ```
>
> 



### bzero函数

```c
//bzero () 会将内存块（字符串）的前 n 个字节清零，其原型为：
void bzero(void *s, int n);
```

【参数】s 为内存（字符串）指针，n 为需要清零的字节数。

  bzero () 会将参数 s 所指的内存区域前 n 个字节，全部设为零值。

  实际上，bzero (void *s, int n) 等价于 memset ((void*) s, 0,size_tn)，用来将内存块的前 n 个字节清零，但是 s 参数为指针，又很奇怪的位于 string.h 文件  中，也可以用来清零字符串。

   注意：bzero () 不是标准函数，没有在 ANSI 中定义，笔者在 VC6.0 和 MinGW5 下编译没通过；据称 Linux 下的 GCC 支持，不过笔者没有亲测。鉴于此，还是使用 memset () 替代吧。



#  CPU并行程序设计

## MPI并行





## OpenMP并行





## Pthread并行



```c
pthread_mutex_init(&m_mutexNetRecv, NULL);
pthread_create(&m_UDPReceive_Thread_ID, NULL, vUDPReceiveManager, (void*)&err);

```







# GPU并行程序设计













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
>     unsigned char buf[sizeof(struct in6_addr)];
>     int domain, s;
>     char str[INET6_ADDRSTRLEN];
>
>     if (argc != 3)
>     {
>         fprintf(stderr, "Usage: %s {i4|i6|<num>} string\n", argv[0]);
>         exit(EXIT_FAILURE);
>     }
>
>     domain = (strcmp(argv[1], "i4") == 0) ? AF_INET : (strcmp(argv[1], "i6") == 0) ? AF_INET6: atoi(argv[1]);
>
>     s = inet_pton(domain, argv[2], buf);
>     if (s <= 0)
>     {
>         if (s == 0)
>             fprintf(stderr, "Not in presentation format");
>         else
>             perror("inet_pton");
>         exit(EXIT_FAILURE);
>     }
>
>     if (inet_ntop(domain, buf, str, INET6_ADDRSTRLEN) == NULL)
>     {
>         perror("inet_ntop");
>         exit(EXIT_FAILURE);
>     }
>
>     printf("%s\n", str);
>
>     exit(EXIT_SUCCESS);
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
> 实例1：
>
> ```c
> //server.c
> #include <sys/types.h>
> #include <sys/socket.h>
> #include <stdio.h>
> #include <netinet/in.h>
> #include <arpa/inet.h>
> #include <unistd.h>
> #include <string.h>
> #include <stdlib.h>
> #include <fcntl.h>
> #include <sys/shm.h>
>
> #define MYPORT 8887
> #define QUEUE 20
> #define BUFFER_SIZE 1024
>
> int main()
> {
>     ///定义sockfd
>     int server_sockfd = socket(AF_INET, SOCK_STREAM, 0);
>
>     ///定义sockaddr_in
>     struct sockaddr_in server_sockaddr;
>     server_sockaddr.sin_family = AF_INET;
>     server_sockaddr.sin_port = htons(MYPORT);
>     server_sockaddr.sin_addr.s_addr = htonl(INADDR_ANY);
>
>     ///bind，成功返回0，出错返回-1
>     if (bind(server_sockfd, (struct sockaddr *)&server_sockaddr, sizeof(server_sockaddr)) == -1)
>     {
>         perror("bind");
>         exit(1);
>     }
>
>     ///listen，成功返回0，出错返回-1
>     if (listen(server_sockfd, QUEUE) == -1)
>     {
>         perror("listen");
>         exit(1);
>     }
>
>     ///客户端套接字
>     char buffer[BUFFER_SIZE];
>     struct sockaddr_in client_addr;
>     socklen_t length = sizeof(client_addr);
>
>     ///成功返回非负描述字，出错返回-1
>     int conn = accept(server_sockfd, (struct sockaddr *)&client_addr, &length);
>     if (conn < 0)
>     {
>         perror("connect");
>         exit(1);
>     }
>
>     while (1)
>     {
>         memset(buffer, 0, sizeof(buffer));
>         int len = recv(conn, buffer, sizeof(buffer), 0);
>         if (strcmp(buffer, "exit\n") == 0)
>             break;
>         fputs(buffer, stdout);
>         send(conn, buffer, len, 0);
>     }
>     close(conn);
>     close(server_sockfd);
>     return 0;
> }
> ```
>
> ```c
> //client.c
> #include <sys/types.h>
> #include <sys/socket.h>
> #include <stdio.h>
> #include <netinet/in.h>
> #include <arpa/inet.h>
> #include <unistd.h>
> #include <string.h>
> #include <stdlib.h>
> #include <fcntl.h>
> #include <sys/shm.h>
>
> #define MYPORT 8887
> #define BUFFER_SIZE 1024
>
> int main()
> {
>     ///定义sockfd
>     int sock_cli = socket(AF_INET, SOCK_STREAM, 0);
>
>     ///定义sockaddr_in
>     struct sockaddr_in servaddr;
>     memset(&servaddr, 0, sizeof(servaddr));
>     servaddr.sin_family = AF_INET;
>     servaddr.sin_port = htons(MYPORT);                 ///服务器端口
>     servaddr.sin_addr.s_addr = inet_addr("127.0.0.1"); ///服务器ip
>
>     ///连接服务器，成功返回0，错误返回-1
>     if (connect(sock_cli, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
>     {
>         perror("connect");
>         exit(1);
>     }
>
>     char sendbuf[BUFFER_SIZE];
>     char recvbuf[BUFFER_SIZE];
>     while (fgets(sendbuf, sizeof(sendbuf), stdin) != NULL)
>     {
>         send(sock_cli, sendbuf, strlen(sendbuf), 0); ///发送
>         if (strcmp(sendbuf, "exit\n") == 0)
>             break;
>         recv(sock_cli, recvbuf, sizeof(recvbuf), 0); ///接收
>         fputs(recvbuf, stdout);
>
>         memset(sendbuf, 0, sizeof(sendbuf));
>         memset(recvbuf, 0, sizeof(recvbuf));
>     }
>
>     close(sock_cli);
>     return 0;
> }
> ```
>
> 实例2
>
> ```c
> //echo_server.c
> #include <stdio.h>
> #include <sys/socket.h>
> #include <arpa/inet.h>
>
> #define BUF_SIZE 5
>
> int main(int argc, char *argv[])
> {
>     char message[BUF_SIZE];
>     int str_len, i;
>
>     struct sockaddr_in serv_addr, clnt_addr;
>
>     int serv_sock = socket(PF_INET, SOCK_STREAM, 0);
>     if (serv_sock == -1)
>     {
>         printf("socket() error");
>         exit(1);
>     }
>
>     memset(&serv_addr, 0, sizeof(serv_addr));
>     serv_addr.sin_family = AF_INET;
>     serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
>     serv_addr.sin_port = htons(9600);
>
>     if (bind(serv_sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == -1)
>     {
>         printf("bind() error");
>         exit(1);
>     }
>
>     if (listen(serv_sock, 5) == 1)
>     {
>         printf("listen() error");
>         exit(1);
>     }
>
>     int clnt_addr_sz = sizeof(clnt_addr);
>     for (i = 0; i < 5; i++)
>     {
>         int clnt_sock = accept(serv_sock, (struct sockaddr *)&clnt_addr, &clnt_addr_sz);
>         if (clnt_sock == -1)
>         {
>             printf("accept() error");
>             exit(1);
>         }
>
>         while (str_len = read(clnt_sock, message, BUF_SIZE) > 0)
>         {
>             write(clnt_sock, message, str_len);
>         }
>
>         close(clnt_sock);
>     }
>
>     close(serv_sock);
>     return 0;
> }
> ```
>
> ```c
> //echo_client.c
> #include <stdio.h>
> #include <string.h>
> #include <sys/socket.h>
> #include <arpa/inet.h>
>
> #define BUF_SIZE 5
>
> int main(int argc, char *argv[])
> {
>     char message[BUF_SIZE];
>     int str_len, i;
>
>     struct sockaddr_in serv_addr, clnt_addr;
>
>     int serv_sock = socket(PF_INET, SOCK_STREAM, 0);
>     if (serv_sock == -1)
>     {
>         printf("socket() error");
>         exit(1);
>     }
>
>     memset(&serv_addr, 0, sizeof(serv_addr));
>     serv_addr.sin_family = AF_INET;
>     serv_addr.sin_addr.s_addr = inet_addr("127.0.0.1");
>     serv_addr.sin_port = htons(9600);
>
>     if (connect(serv_sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == -1)
>     {
>         printf("connect() error");
>         exit(1);
>     }
>
>     while (1)
>     {
>         fputs("请输入您的信息,按Q键退出\n", stdout);
>         fgets(message, 1024, stdin);
>
>         //因为fgets会保留输入中换行符,故判断加\n
>         if (!strcmp(message, "q\n") || !strcmp(message, "Q\n"))
>         {
>             break;
>         }
>
>         write(serv_sock, message, sizeof(message));
>         read(serv_sock, message, BUF_SIZE - 1);
>         printf("Message from server: %s\n", message);
>     }
>
>     close(serv_sock);
>     return 0;
> }
> ```
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

​    <font color=blue>返回值：</font>

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

​    <font color=blue>返回值：</font>

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
>     int sockfd,len;  
>     struct sockaddr_in addr;  
>     int addr_len = sizeof(struct sockaddr_in);  
>     char buffer[256];  
>     /* 建立 socket*/  
>     if((sockfd=socket(AF_INET,SOCK_DGRAM,0))<0){  
>         perror ("socket");  
>         exit(1);  
>     }  
>     /* 填写 sockaddr_in 结构 */  
>     bzero ( &addr, sizeof(addr) );  
>     addr.sin_family=AF_INET;  
>     addr.sin_port=htons(PORT);  
>     addr.sin_addr.s_addr=htonl(INADDR_ANY) ;  
>     if (bind(sockfd, (struct sockaddr *)&addr, sizeof(addr))<0){  
>         perror("connect");  
>         exit(1);  
>     }  
>     while(1){  
>         bzero(buffer,sizeof(buffer));  
>         len = recvfrom(sockfd, buffer,sizeof(buffer), 0 , (struct sockaddr *)&addr ,&addr_len);  
>         /* 显示 client 端的网络地址 */  
>         printf("receive from %s\n" , inet_ntoa( addr.sin_addr));  
>         /* 将字串返回给 client 端 */  
>         sendto(sockfd,buffer,len,0,(struct sockaddr *)&addr,addr_len);  
>     }  
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
>     int s,len;  
>     struct sockaddr_in addr;  
>     int addr_len =sizeof(struct sockaddr_in);  
>     char buffer[256];  
>     /* 建立 socket*/  
>     if((s = socket(AF_INET,SOCK_DGRAM,0))<0){  
>         perror("socket");  
>         exit(1);  
>     }  
>     /* 填写 sockaddr_in*/  
>     bzero(&addr,sizeof(addr));  
>     addr.sin_family = AF_INET;  
>     addr.sin_port = htons(PORT);  
>     addr.sin_addr.s_addr = inet_addr(SERVER_IP);  
>     while(1){  
>         bzero(buffer,sizeof(buffer));  
>         /* 从标准输入设备取得字符串 */  
>         len =read(STDIN_FILENO,buffer,sizeof(buffer));  
>         /* 将字符串传送给 server 端 */  
>         sendto(s,buffer,len,0,(struct sockaddr *)&addr,addr_len);  
>         /* 接收 server 端返回的字符串 */  
>         len = recvfrom(s, buffer, sizeof(buffer), 0, (struct sockaddr *)&addr, &addr_len);  
>         printf("receive: %s",buffer);  
>     }  
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
>     int   len;
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
>     char buffer[128];
>     int result, nread;
>     fd_set inputs, testfds;
>     struct timeval timeout;
>     FD_ZERO(&inputs);   //用select函数之前先把集合清零
>     FD_SET(0, &inputs); //把要检测的句柄——标准输入（0），加入到集合里。
>     while (1)
>     {
>         testfds = inputs;
>         timeout.tv_sec = 2;
>         timeout.tv_usec = 500000;
>         result = select(FD_SETSIZE, &testfds, (fd_set *)0, (fd_set *)0, &timeout);
>         switch (result)
>         {
>         case 0:
>             printf("timeout/n");
>             break;
>         case -1:
>             perror("select");
>             exit(1);
>         default:
>             if (FD_ISSET(0, &testfds))
>             {
>                 ioctl(0, FIONREAD, &nread); //取得从键盘输入字符的个数，包括回车。
>                 if (nread == 0)
>                 {
>                     printf("keyboard done/n");
>                     exit(0);
>                 }
>                 nread = read(0, buffer, nread);
>                 buffer[nread] = 0;
>                 printf("read %d from keyboard: %s", nread, buffer);
>             }
>             break;
>         }
>     }
>     return 0;
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

​      time () 提供了秒级的精确度，用 time () 函数结合其他函数（如：localtime、gmtime、asctime、ctime）可以获得当前系统时间或是标准时间。如果需要更高的时间精确度，就需要 `struct timespec` 和 `struct timeval `来处理。

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
struct  timeb{
    time_t   time;                      /* 为 1970-01-01 至今的秒数 */
    unsigned   short   millitm;        /* 千分之一秒即毫秒 */
    short   timezonel;               /* 为目前时区和 Greenwich 相差的时间，单位为分钟 */
    short   dstflag;                   /* 为日光节约时间的修正状态，如果为非 0 代表启用日光节约时间修正 */
};

// struct timespec 有两个成员，一个是秒，一个是纳秒，所以最高精确度是纳秒。
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
struct  timeb{
    time_t   time;                      /* 为 1970-01-01 至今的秒数 */
    unsigned   short   millitm;        /* 千分之一秒即毫秒 */
    short   timezonel;               /* 为目前时区和 Greenwich 相差的时间，单位为分钟 */
    short   dstflag;                   /* 为日光节约时间的修正状态，如果为非 0 代表启用日光节约时间修正 */
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
    int  sival_int;         /* Integer value */
    void   *sival_ptr;      /* Pointer value */
};

struct sigevent {
    int sigev_notify; /* Notification method */
    int sigev_signo;  /* Notification signal */
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
      int sigev_notify; /* Notification method */
      int sigev_signo;  /* Notification signal */
      union sigval sigev_value;  /* Data passed with notification */
      void (*sigev_notify_function) (union sigval);/* Function used for thread notification (SIGEV_THREAD) */
      void *sigev_notify_attributes;/* Attributes for notification thread (SIGEV_THREAD) */
      pid_t sigev_notify_thread_id; /* ID of thread to signal (SIGEV_THREAD_ID) */
  };
  ```


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
  	SIGEV_NONE , // 设置该值防止定时器到期时产生信号,空的提醒，事件发生时不做任何事情;
  	SIGEV_THREAD, //通过线程创建传递信号，通知进程在一个新的线程中启动 sigev_notify_function 函数，函数的实参是 sigev_value，系统 API 自动启动一个线程，我们不用显式启动;
  	SIGEV_THREAD_ID //表示信号会发送到指定的线程;
  }
  ```

  sigev_signo 表示定时器到期时将会发出的信号。这些信号在 signum.h 中有定义。常用的信号由如下：

  ```c
  #define SIGALRM 		14 // 时钟信号
  #define SIGUSR1			10 //用户自定义信号1
  #define SIGUSR2 		12  //用户自定义信号2
  ...
  ```

  还有最后一个成员是 sigev_value，它则是来绑定定时器的。表示这些设置的环境将会作用到哪个定时器上。

+ 该函数最后一个参数是 timerid，表示定时器的 id，定时器标识符，结构体timer_t变量的地址，定时器创建成功，将会产生一个 id，而该 id 就会被赋值给 timerid。





####   timer_settime函数

​		经过上述函数创建了一个定时器之后，还需要设置定时器的定时周期以及启动时钟周期（即过了多久开始启动时钟）。这项工作交由 timer_settime 接口来完成。

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



####   signal 函数

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
> >  在信号发生跳转到自定的 handler 处理函数执行后，系统会自动将此处理函数换回原来系统预设的处理方式，如果要改变此操作请改用 sigaction ().



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
>    signal(SIGINT, sighandler);
>
>    while(1)
>    {
>       printf("开始休眠一秒钟...\n");
>       sleep(1);
>    }
>
>    return(0);
> }
>
> void sighandler(int signum)
> {
>    printf("捕获信号 %d，跳出...\n", signum);
>    exit(1);
> }
> ```
>
> 让我们编译并运行上面的程序，这将产生以下结果，且程序会进入无限循环，需使用 CTRL + C 键跳出程序。
>
> ```bash
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 开始休眠一秒钟...
> 捕获信号 2，跳出...
> ```
>
>







####  sigaction函数

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
    默认会导致进程流产的信号有：SIGABRT,SIGBUS,SIGFPE,SIGILL,SIGIOT,SIGQUIT,SIGSEGV,SIGTRAP,SIGXCPU,SIGXFSZ
    默认会导致进程退出的信号有：SIGALRM,SIGHUP,SIGINT,SIGKILL,SIGPIPE,SIGPOLL,SIGPROF,SIGSYS,SIGTERM,SIGUSR1,SIGUSR2,SIGVTALRM
    默认会导致进程停止的信号有：SIGSTOP,SIGTSTP,SIGTTIN,SIGTTOU
    默认进程忽略的信号有：SIGCHLD,SIGPWR,SIGURG,SIGWINCH

+ act：要设置的对信号的新处理方式。

+ oldact：原来对信号的处理方式。

+ 返回值：0 表示成功，-1 表示有错误发生。

  错误代码：
  1、EINVAL 参数 signum 不合法，或是企图拦截 SIGKILL/SIGSTOPSIGKILL 信号。
  2、EFAULT 参数 act, oldact 指针地址无法存取。
  3、EINTR 此调用被中断。

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





# 函数与内存

## [申请内存时底层发生了什么？](https://mp.weixin.qq.com/s/DN-ckM1YrPMeicN7P9FvXg)

识不到的东西却无比重要，申请过这么多内存，**你知道申请内存时底层都发生什么了吗**？

我们就从神话故事开始吧。



> **三界**

中国古代的神话故事通常有“三界”之说，一般指的是天、地、人三界，天界是神仙所在的地方，凡人无法企及；人界说的是就是人间；地界说的是阎罗王所在的地方，孙悟空上天入地无所不能就是说可以在这三界自由出入。

有的同学可能会问，这和计算机有什么关系呢？

原来，我们的代码也是分三六九等的，程序运行起来后也是有“三界”之说的，程序运行起来的“三界”就是这样的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUzGCKPsIQwtHU6atfP23MvxZmrbIjjntNGxAZ3IXOfmBMibnzLDJghkA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

x86 CPU提供了“四界”：0,1,2,3，**这几个数字其实就是指CPU的几种工作状态**，数字越小表示CPU的特权越大，0号状态下CPU特权最大，可以执行任何指令，数字越大表示CPU特权越小，3号状态下CPU特权最小，不能执行一些特权指令。

一般情况下系统只使用0和3，因此确切的说是“两界”，这两界可不是说天、地，这两界指的是“用户态(3)”以及“内核态(0)”，接下来我们看看什么是内核态、什么是用户态。



> **内核态**

什么是内核态？当CPU执行操作系统代码时就处于内核态，**在内核态下CPU可以执行任何机器指令、访问所有地址空间、不受限制的访问任何硬件**，可以简单的认为内核态就是“天界”，在这里的代码(操作系统代码)无所不能。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUBWWX8bkrGllWY1YGO0z0D0bHjiaicMQ3icO8RZbvl58f7bQUFpg3YMblA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



> **用户态**

什么是用户态？当CPU执行我们写的“普通”代码(非操作系统、驱动程序员)时就处于用户态，粗糙的划分方法就是除了操作系统之外的代码，就像我们写的HelloWorld程序。

用户态就好比“人界”，在用户态我们的代码处处受限，不能直接访问硬件、不能访问特定地址空间，否则神仙(操作系统)直接将你kill掉，这就是著名的Segmentation fault、不能执行特权指令，等等。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUkKk9tz3fgSF9ejzVWnxicNrRcviaZYjtqWicwJ3GuMpSBM2oWniacxlueQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

关于这一部分的详细讲解，请参见《[深入理解操作系统](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU2NTYyOTQ4OQ==&action=getalbum&album_id=1433368223499796481#wechat_redirect)》系列文章。



> **跨界**

孙悟空神通广大，一个跟斗就能从人间跑到天上去骂玉帝老儿，程序员就没有这个本领了。普通程序永远也去不了内核态，只能以通信的方式从用户态往内核态传递信息。

操作系统为普通程序员留了一些特定的暗号，这些暗号就和普通函数一样，程序员通过调用这些暗号就能向操作系统请求服务了，这些像普通函数一样的暗号就被称为[**系统调用**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483880&idx=1&sn=26ab417ffdd46b2956e5dc07516477af&chksm=fcb986b6cbce0fa0e0959341ec9c7a0c2db0acd9f5a1250e5cbe33306da2f10f1f3cd08152aa&scene=21#wechat_redirect)，System Call，通过系统调用我们可以让操作系统代替我们完成一些事情，像打开文件、网络通信等等。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU6nDiazC6U7A9gLAwKOYTscricaKKicESIbiaAnXia3tqCs0upAjsictJXpiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

你可能有些疑惑，什么，还有系统调用这种东西，为什么我没调用过也可以打开文件、进行网络通信？



> **标准库**

虽然我们可以通过系统让操作系统替我们完成一些特定任务，但这些系统调用都是和操作系统强相关的，Linux和Windows的系统调用就完全不同。

如果你直接使用系统调用的话，那么Linux版本的程序就没有办法在Windows上运行，因此我们需要某种标准，该标准对程序员屏蔽底层差异，这样程序员写的程序就无需修改的在不同操作系统上运行了。

在C语言中，这就是所谓的**标准库**。

注意，标准库代码也是运行在用户态的，并不是神仙(操作系统)，一般来说，我们调用标准库去打开文件、网络通信等等，标准库再根据操作系统选择对应的系统调用。

从分层的角度看，我们的程序一般都是这样的汉堡包类型：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU6CicgEJETyCvCkQiaF9yZ6wickG6bmKqbOv5Rq9CXlGoExibic9Aedcniaiag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最上层是应用程序，应用程序一般只和标准库打交道(当然，我们也可以绕过标准库)，标准库通过系统调用和操作系统交互，操作系统管理底层硬件。

**这就是为什么在C语言下同样的open函数既能在Linux下打开文件也能在Windows下打开文件的原因**。

说了这么多，这和malloc又有什么关系呢？



> **主角登场**

原来，我们分配内存时使用的malloc函数其实不是实现在操作系统里的，而是在标准库中实现的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUCaWAXfdCrP8Q9LqJQTJtxplt150VY6eKvQR2MO3iaUefbcQPmMS7Bjw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们知道了，malloc是标准库的一部分，当我们调用malloc时实际上是标准库在为我们申请内存。

这里值得注意的是，我们平时在C语言中使用malloc只是内存分配器的一种，实际上有很多内存分配器，像tcmalloc，jemalloc等等，它们都有各自适用的场景，对于高性能程序来说使用满足特定要求的内存分配器是至关重要的。

那么接下来的问题就是malloc又是怎么工作的呢？



> **malloc是如何工作的**

实际上你可以把malloc的工作理解为去停车场找停车位，停车场就是一片malloc持有的内存，可用的停车位就是可供malloc支配的空闲内存，停在停车场占用的车位就是已经分配出去的内存，特殊点在于停在该停车场的车宽度大小不一，malloc需要回答这样一个问题：当有一辆车来到停车场后该停到哪里？

通过上面的类比你应该能大体理解工作原理了，具体分析详见《[自己动手实现一个malloc内存分配器](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247485171&idx=1&sn=d93f2f5e9d61b00515c043776d2f7330&chksm=fcb981adcbce08bb39d120d7bfd097308371fb4b4e4369ba9502ae4e4243028b450bd0fe3110&scene=21#wechat_redirect)》。

但是，请注意，**上面这****篇文章并不是故事的全部**，在这篇文章中有一个问题我们故意忽略了，这个问题就是**如果内存分配器中的空闲内存块不够用了该怎么办呢**？

在上面这篇文章中我们总是假定自己实现的malloc总能找到一块空闲内存，但实际上并不是这样的。



> **内存不够该怎么办？**

让我们再来看一下程序在内存中是什么样的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU7M3vQH0jVRDWx2bZPgic6ATaHiapb3iaK7bh4YicEQGhNXvavQToeVHb6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们已经知道了，malloc管理的是堆区，注意，在堆区和栈区之间有一片空白区域，这片空白区域的目的是什么呢？

原来，栈区其实是可以增长的，随着调用深度的增加，相应的栈区占用的内存也会增加，关于栈区这一主题，你可以参考《[函数运行时在内存中是什么样子](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484963&idx=1&sn=542d3bec57c6a9dfc17c83005fd2c030&chksm=fcb9817dcbce086b10cb44cad7c9777b0088fb8d9d6baf71ae36a9b03e1f8ef5bec62b79d6f7&scene=21#wechat_redirect)》这篇文章。

栈区的增长就需要占用原来的空白区域。

相应的，堆区也可以增长：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUibCxiaHf5fONfge0DMCHZgP3XXncRoV2Dwhiarrw3uJERoZEgTJicdRkow/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

堆区增长后占用的内存就会变多，这就解决了内存分配器空闲内存不足的问题，那么很自然的，malloc该怎样让堆区增长呢？

原来malloc内存不足时要向操作系统申请内存，**操作系统才是真大佬**，malloc不过是小弟，对每个进程，操作系统(类Unix系统)都维护了一个叫做brk的变量，brk发音break，这个brk指向了堆区的顶部。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUBaN7diaLZy5Z7kAkYHvU8pV4BcaRFMV8NmcbNRgrW040KEWVDKtolbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

将brk上移后堆区增大，那么我们该怎么样让堆区增大呢？

这就涉及到我们刚提到的系统调用了。



> **向操作系统申请内存**

操作系统专门提供了一个叫做brk的系统调用，还记得刚提到堆的顶部吧，这个brk()系统调用就是用来增加或者减小堆区的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUuibpNKII4KyVk4FDs8I5mQglzJDanibSZfbibxn9cbJ3gY7zG2vs9rD9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

实际上不只brk系统调用，sbr、mmap系统调用也可以实现同样的目的，mmap也更为灵活，但该函数并不是本文重点，就不在这里详细讨论了。

现在我们知道了，如果malloc自己维护的内存空间不足将通过brk系统调用向操作系统申请内存。这样malloc就可以把这些从操作系统申请到的内存当做新的空闲内存块分配出去。



> **看起来已经讲完的故事**

现在我就可以简单总结一下了，当我们申请内存时，经历这样几个步骤：

1. 程序调用malloc申请内存，注意malloc实现在标准库中
2. malloc开始搜索空闲内存块，如果能找到一块大小合适的就分配出去，前两个步骤都是发生在用户态
3. 如果malloc没有找到空闲内存块那么就像操作系统发出请求来增大堆区，这是通过系统调用brk(sbrk、mmap也可以)实现的，注意，brk是操作系统的一部分，因此当brk开始执行时，此时就进入内核态了。brk增大进程的堆区后返回，malloc的空闲内存块增加，此时malloc又一次能找到合适的空闲内存块然后分配出去。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUO3f3ztpm8WRXfblEAvHOhydv8Zr1o58usmryw9GrhbI6YeIT5MQz5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

故事就到这里了吗？



> **冰山之下**

实际上到目前为止，我们接触到的仅仅是冰山一角。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUOXGKA1bG5ojicgS3cEI0MMVp54qOH46KQHb3BgApHXpQmlrKwjYKnWg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们看到的冰山是这样的：我们向malloc申请内存，malloc内存不够时向操作系统申请内存，之后malloc找到一块空闲内存返回给调用者。

但是，你知道吗，**上述过程根本就没有涉及到哪怕一丁点物理内存**！！！

我们确实向malloc申请到内存了，malloc不够也确实从操作系统申请到内存了，但这些内存都不是真的物理内存，**NOT REAL**。

实际上，进程看到的内存都是假的，是操作系统给进程的一个幻象，这个幻象就是由著名的**虚拟内存**系统来维护的，我们经常说的这张图就是进程的虚拟内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU7M3vQH0jVRDWx2bZPgic6ATaHiapb3iaK7bh4YicEQGhNXvavQToeVHb6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

所谓虚拟内存就是假的、不是真正的物理内存，虚拟内存是给进程用的，操作系统维护了虚拟内存到物理内存的映射，当malloc返回后，程序员申请到的内存就是虚拟内存。

注意，**此时操作系统根本就没有真正的分配物理内存，程序员从malloc拿到的内存目前还只是一张空头支票**。

那么这张空头支票什么时候才能兑现呢？也就是什么时候操作系统才会真正的分配物理内存呢？

答案是当我们真正使用这段内存时，当我们真正使用这段内存时，这时会产生一个缺页错误，操作系统捕捉到该错误后开始真正的分配物理内存，操作系统处理完该错误后我们的程序才能真正的读写这块内存。

这里只是简略的提到了虚拟内存，实际上虚拟内存是当前操作系统内部极其重要的一部分，关于虚拟内存的工作原理将在《[深入理解操作系统](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU2NTYyOTQ4OQ==&action=getalbum&album_id=1433368223499796481#wechat_redirect)》系列文章中详细讨论。



> **完整的故事**

现在，这个故事就可以完整讲出来了，当我们调用malloc申请内存时：

1. malloc开始搜索空闲内存块，如果能找到一块大小合适的就分配出去
2. 如果malloc找不到一块合适的空闲内存，那么调用brk等系统调用扩大堆区从而获得更多的空闲内存
3. malloc调用brk后开始转入内核态，此时操作系统中的虚拟内存系统开始工作，扩大进程的堆区，注意额外扩大的这一部分内存仅仅是虚拟内存，操作系统并没有为此分配真正的物理内存
4. brk执行结束后返回到malloc，从内核态切换到用户态，malloc找到一块合适的空闲内存后返回
5. 程序员拿到新申请的内存，程序继续
6. 当有代码读写新申请的内存时系统内部出现缺页中断，此时再次由用户态切换到内核态，操作系统此时真正的分配物理内存，之后再次由内核态切换回用户态，程序继续。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUSVyLUeFf3bAv8kJ6IBKpERibQZqqiaJSGx6jdKllImYIC9NnlpY3ic2Ww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

以上就是一次内存申请的完整过程，可以看到一次内存申请过程是非常复杂的。



> **总结**

怎么样，程序员申请内存使用的malloc虽然表面看上去非常简单，简单到就一行代码，但这行代码背后是非常复杂的。

有的同学可能会问，为什么我们要理解这背后的原理呢？理解了原理后我才能知道内存申请的复杂性，对于高性能程序来讲频繁的调用malloc对系统性能是有影响的，那么很自然的一个问题就是我们能否避免malloc？

##   [函数与内存](https://mp.weixin.qq.com/s/fyrnqiK8ucGjmUxuHeakNQ)



在开始本篇的内容前，我们先来思考几个问题。

1. 我们先来看一段简单的代码：

   ```c
   void func(int a) {
     if (a > 100000000) return;

     int arr[100] = {0};
     func(a + 1);
   }
   ```

2. 我们在上一篇文章《[**高性能高并发服务器是如何实现的**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484933&idx=1&sn=c4112a54f5751f38e841baf3e3cc35bd&chksm=fcb9815bcbce084de2823467d3ba9d3e835663a6bb69df1fc7f71677aef099584f93b0e01809&scene=21#wechat_redirect)》中提到了一项关键技术——协程，你知道协程的本质是什么吗？有的同学可能会说是用户态线程，那么什么是用户态线程，这是怎么实现的？

3. 函数运行起来后在内存中是什么样子？

这几个问题看似没什么关联，但这背后都指向一样东西，这就是所谓的函数**运行时栈**，**run time stack**。

接下来我们就好好看看到底什么是函数运行时栈，为什么彻底理解函数运行时栈对程序员来说非常重要。

> **从进程、线程到函数调用**

汽车在高速上行驶时有很多信息，像速度、位置等等，通过这些信息我们可以直观的感受汽车的运行时状态。

里等等，通过这些信息我们可以直观的感受系统中程序运行的状态。

其中，我们创造了进程、线程这样的概念来记录有哪些程序正在运行，关于进程和线程的概念请参见《[**看完这篇还不懂进程、线程与线程池你来打我**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484768&idx=1&sn=049db350af9e5eea5cf3523ceb83f447&chksm=fcb9823ecbce0b28ca28d021e68c78138cde4a1b86ea7209c0c667d3d544d223d8b2aecbccec&scene=21#wechat_redirect)》。

**进程和线程的运行体现在函数执行上**，函数的执行除了函数内部执行的顺序执行还有子函数调用的控制转移以及子函数执行完毕的返回。其中函数内部的顺序执行乏善可陈，重点是函数的调用。

因此接下来我们的视角将从宏观的进程和线程拉近到微观下的函数调用，重点来讨论一下函数调用是怎样实现的。



>  **函数执行的活动轨迹：栈**

玩过游戏的同学应该知道，有时你为了完成一项主线任务不得不去打一些支线的任务，支线任务中可能还有支线任务，当一个支线任务完成后退回到前一个支线任务，这是什么意思呢，举个例子你就明白了。

假设主线任务西天取经A依赖支线任务收服孙悟空B和收服猪八戒C，也就是说收服孙悟空B和收服猪八戒C完成后才能继续主线任务西天取经A；

支线任务收服孙悟空B依赖任务拿到紧箍咒D，只有当任务D完成后才能回到任务B；

整个任务的依赖关系如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UM6nPtWzlRuA1EU8Inc4cp30xQCyiaGTAvatAOSrdKYM9UnrkChzVHKibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们来模拟一下任务完成过程。

首先我们来到任务A，执行主线任务：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMWEicBfDADgsdzibPLyJoFiaGANL5OBYmMGoMmOys7ic209SbiazzSXguhlA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务A的过程中我们发现任务A依赖任务B，这时我们暂停任务A去执行任务B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMlaT1C1nZ4Nib1AlhibdOTia1lrYSGb054xd20IFjWKCdXcX0HwicTicdLOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务B的时候，我们又发现依赖任务D：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UM2WGO1bReDXjbu4ts90MNoFY9s5pgUThvA7EATPjianYdIliaBlQGXaoQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务D的时候我们发现该任务不再依赖任何其它任务，因此C完成后我们可以会退到前一个任务，也就是B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMlaT1C1nZ4Nib1AlhibdOTia1lrYSGb054xd20IFjWKCdXcX0HwicTicdLOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

任务B除了依赖任务C外不再依赖其它任务，这样任务B完成后就可以回到任务A：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMWEicBfDADgsdzibPLyJoFiaGANL5OBYmMGoMmOys7ic209SbiazzSXguhlA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们回到了主线任务A，依赖的任务B执行完成，接下来是任务C：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMPZ0rRnEdnBsDPD4RTY08To0AicbHRoK7WhzFvu6XvzPjMo0O0JKZl7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

和任务D一样，C不依赖任何其它其它任务，任务C完成后就可以再次回到任务A，再之后任务A执行完毕，整个任务执行完成。

让我们来看一下整个任务的活动轨迹：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMibA8lEicYT8Lor9AylgiaHXwaSC2wPpOZL0Tib4o1vk6ib9MeMTn0wzdchg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

仔细观察，实际上你会发现这是一个First In Last Out 的顺序，天然适用于栈这种数据结构来处理。

再仔细看一下栈顶的轨迹，也就是A、B、D、B、A、C、A，**实际上你会发现这里的轨迹就是任务依赖树的遍历过程**，是不是很神奇，这也是为什么树这种数据结构的遍历除了可以用递归也可以用栈来实现的原因。



>  **A Box**

函数调用也是同样的道理，你把上面的ABCD换成函数ABCD，本质不变。

因此，现在我们知道了，使用栈这种结构就可以用来保存函数调用信息。

和游戏中的每个任务一样，当函数在运行时每个函数也要有自己的一个“小盒子”，**这个小盒子中保存了函数运行时的各种信息**，这些小盒子通过栈这种结构组织起来，这个小盒子就被称为栈帧，stack frames，也有的称之为call stack，不管用什么命名方式，总之，就是这里所说的小盒子，这个小盒子就是函数运行起来后占用的内存，**这些小盒子构成了我们通常所说的栈区**。关于栈区详细的讲解你可以参考《[**深入理解操作系统：程序员应如何理解内存**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483839&idx=1&sn=20276b1966b76530890e9f09c6f41892&chksm=fcb986e1cbce0ff73c50f1a3f943bf182d8db666b27b4e55d20bd7cbbdea3fadbfbcb7c68f26&scene=21#wechat_redirect)》一文。

那么函数调用时都有哪些信息呢？



>  **控制转移**

我们知道当函数A调用函数B的时候，控制从A转移到了B，所谓控制其实就是指CPU执行属于哪个函数的机器指令，CPU从开始执行属于函数A的指令切换到执行属于函数B的指令，我们就说控制从函数A转移到了函数B。

控制从函数A转移到函数B，那么我们需要有这样两个信息：

- 我从哪里来 (返回)
- 要到去哪里 (跳转)

是不是很简单，就好比你出去旅游，你需要知道去哪里，还需要记住回家的路。

函数调用也是同样的道理。

当函数A调用函数B时，我们只要知道：

- 函数A对于的机器指令执行到了哪里 (我从哪里来，返回)
- 函数B第一条机器指令所在的地址 (要到哪里去，跳转)

有这两条信息就足以让CPU开始执行函数B对应的机器指令，当函数B执行完毕后跳转回函数A。

那么这些信息是怎么获取并保持的呢？

现在我们就可以打开这个小盒子，看看是怎么使用的了。

假设函数A调用函数B，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMVjgxLV2L5Nn0WhGZAhge7RzdD01u5CzR7RpbictDlDrb3719cLbKfeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当前，CPU执行函数A的机器指令，该指令的地址为0x400564，接下来CPU将执行下一条机器指令也就是:

-

```c
call 0x400540
```



这条机器指令是什么意思呢？

这条机器指令对应的就是我们在代码中所写的函数调用，注意call后有一条机器指令地址，注意观察上图你会看到，**该地址就是函数B的第一条机器指令**，从这条机器指令后CPU将跳转到函数B。

现在我们已经解决了控制跳转的“要到哪里去”问题，当函数B执行完毕后怎么跳转回来呢？

原来，call指令除了给出跳转地址之外还有这样一个作用，也就是**把call指令的下一条指令的地址，也就是0x40056a push到函数A的栈帧中**，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMQsNg63AfUVOusvQkTa19wTBdmIK2l2h2AggIiceYQ1zf8pTPlomRSrQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 =100x80)

现在，函数A的小盒子变大了一些，因为装入了返回地址：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMLHu7EPtcboFPDiciaZ2ZJFtGjdDRIYY5icialsYTjAnu2uUMI396iaNRQ6Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在CPU开始执行函数B对应的机器指令，注意观察，函数B也有一个属于自己的小盒子(栈帧)，可以往里面扔一些必要的信息。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMEFHVVgibvBicAtmibj1s06Vr5M7ibWk4rJoxcoiclYeFXcfXsXibQNkf1E4w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果函数B中又调用了其它函数呢？

道理和函数A调用函数B是一样的。

让我们来看一下函数B最后一条机器指令ret，这条机器指令的作用是告诉CPU跳转到函数A保存在栈帧上的返回地址，这样当函数B执行完毕后就可以跳转到函数A继续执行了。

至此，我们解决了控制转移中“我从哪里来”的问题。



>  **传递参数与获取返回值**

函数调用与返回使得我们可以编写函数，进行函数调用。但调用函数除了提供函数名称之外还需要传递参数以及获取返回值，那么这又是怎样实现的呢？

在x86-64中，多数情况下参数的传递与获取返回值是通过**寄存器**来实现的。

假设函数A调用了函数B，函数A将一些参数写入相应的寄存器，当CPU执行函数B时就可以从这些寄存器中获取参数了。

同样的，函数B也可以将返回值写入寄存器，当函数B执行结束后函数A从该寄存器中就可以读取到返回值了。

我们知道寄存器的数量是有限的，当传递的参数个数多于寄存器的数量该怎么办呢？

这时那个属于函数的小盒子也就是栈帧又能发挥作用了。

原来，当参数个数多于寄存器数量时剩下的参数直接放到栈帧中，这样被调函数就可以**从前一个函数的栈帧中获取到参数了**。

现在栈帧的样子又可以进一步丰富了，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMzLPnnNzHXLsYF3VAXoAsBCpibA17YRdaXiamLnfibpv0vURiaIQicLCTZdg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从图中我们可以看到，调用函数B时有部分参数放到了函数A的栈帧中，同时函数A栈帧的顶部依然保存的是返回地址。



> **局部变量**

我们知道在函数内部定义的变量被称为局部变量，这些变量在函数运行时被放在了哪里呢？

原来，这些变量同样可以放在寄存器中，但是当局部变量的数量超过寄存器的时候这些变量就必须放到栈帧中了。

因此，我们的栈帧内容又一步丰富了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMM3Q3LQdcQLJdtGktOKxoxqZeRxAgMsqFHuthohdnBBtN2hWDmIicL9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

细心的同学可能会有这样的疑问，我们知道寄存器是共享资源可以被所有函数使用，既然可以将函数A的局部变量写入寄存器，那么当函数A调用函数B时，函数B的局部变量也可以写到寄存器，这样的话当函数B执行完毕回到函数A时寄存器的值已经被函数B修改过了，这样会有问题吧。

这样的确会有问题，因此我们在向寄存器中写入局部变量之前，**一定要先将寄存器中开始的值保存起来**，当寄存器使用完毕后再恢复原值就可以了。

那么我们要将寄存器中的原始值保存在哪里呢？

有的同学可能已经猜到了，没错，依然是函数的栈帧中。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMib8974tib4sQXTWY9kywBtvHjFxTanphsQsUb6IprbevNBTGJuySplYQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最终，我们的小盒子就变成了如图所示的样子，当寄存器使用完毕后根据栈帧中保存的初始值恢复其内容就可以了。

现在你应该知道函数在运行时到底是什么样子了吧，以上就是问题3的答案。



>  **Big Picture**

需要再次强调的一点就是，上述讨论的栈帧就位于我们常说的栈区。

栈区，属于进程地址空间的一部分，如图所示，我们将栈区放大就是图左边的样子。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMkbNXU3lnWgRWhJMmtkFnTFxvicAXCuQTAibfGMkM0h57bdOe3E9YzQ9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 =80x80)

关于栈区详细的讲解你可以参考《[**深入理解操作系统：程序员应如何理解内存**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483829&idx=1&sn=b9c32f56e95bdc229315e2b5ffd365cd&chksm=fcb986ebcbce0ffdc6c087775b895f6a0c0b83c72a91ee5614e253bea005509de8abfab6fc0e&scene=21#wechat_redirect)》这篇。

最后，让我们回到文章开始的这段简单代码：

```c
void func(int a) {
    if (a > 100000000) return;

    int arr[100] = {0};
    func(a + 1);
}

void main(){
    func(0);
}
```



想一想这段代码会有什么问题？

原来，**栈区是有大小限制的**，当超过限制后就会出现著名的**栈溢出**问题，显然上述代码会导致这一问题的出现。

因此：

1. 不要创建过大的局部变量
2. 函数栈帧，也就是调用层次不能太多



**总结**

本章我们从几个看似没什么关联的问题出发，详细讲解了函数运行时栈是怎么一回事，为什么我们不能创建过多的局部变量。细心的同学会发现第2个问题我们没有解答，这个问题的讲解放到下一篇，也就是协程中讲解。





##   [线程间共享了哪些进程资源？](https://mp.weixin.qq.com/s/5Xq-uzbjCExWfws-czVYGQ)

进程和线程这两个话题是程序员绕不开的，操作系统提供的这两个抽象概念实在是太重要了。

关于进程和线程有一个**极其经典**的问题，那就是进程和线程的区别是什么？相信很多同学对答案似懂非懂。



>  **记住了不一定真懂**

关于这个问题有的同学可能已经“背得”滚瓜烂熟了：“进程是操作系统分配资源的单位，线程是调度的基本单位，**线程之间共享进程资源**”。

可是你真的理解了上面最后一句话吗？**到底线程之间共享了哪些进程资源，共享资源意味着什么？共享资源这种机制是如何实现的？**对此如果你没有答案的话，那么这意味着**你几乎很难写出能正确工作的多线程程序**，同时也意味着这篇文章就是为你准备的。



>  **逆向思考**

查理芒格经常说这样一句话：“反过来想，总是反过来想”，如果你对线程之间共享了哪些进程资源这个问题想不清楚的话那么也可以反过来思考，那就是**有哪些资源是线程私有的**。



> **线程私有资源**

线程运行的本质其实就是函数的执行，函数的执行总会有一个源头，这个源头就是所谓的入口函数，CPU从入口函数开始执行从而形成一个执行流，只不过我们人为的给执行流起一个名字，这个名字就叫线程。

既然线程运行的本质就是函数的执行，那么函数执行都有哪些信息呢？

在《[函数运行时在内存中是什么样子](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484963&idx=1&sn=542d3bec57c6a9dfc17c83005fd2c030&chksm=fcb9817dcbce086b10cb44cad7c9777b0088fb8d9d6baf71ae36a9b03e1f8ef5bec62b79d6f7&scene=21#wechat_redirect)》这篇文章中我们说过，函数运行时的信息保存在栈帧中，栈帧中保存了函数的返回值、调用其它函数的参数、该函数使用的局部变量以及该函数使用的寄存器信息，如图所示，假设函数A调用函数B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UNsZhzaGdjEAX7UrhQ9Zm6c0bC3tHYkahgiatFD7M8EYibTQDiaJx5LtJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此外，CPU执行指令的信息保存在一个叫做程序计数器的寄存器中，通过这个寄存器我们就知道接下来要执行哪一条指令。由于操作系统随时可以暂停线程的运行，因此我们保存以及恢复程序计数器中的值就能知道线程是从哪里暂停的以及该从哪里继续运行了。

由于线程运行的本质就是函数运行，函数运行时信息是保存在栈帧中的，因此每个线程都有自己独立的、私有的栈区。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UKECTocZz5RBaFsSicibSeKLsmkKcalTic01yJicf9Ig1JpVH7JEYicTaic8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

同时函数运行时需要额外的寄存器来保存一些信息，像部分局部变量之类，这些寄存器也是线程私有的，**一个线程不可能访问到另一个线程的这类寄存器信息**。

从上面的讨论中我们知道，到目前为止，所属线程的栈区、程序计数器、栈指针以及函数运行使用的寄存器是线程私有的。

以上这些信息有一个统一的名字，就是**线程上下文**，thread context。

我们也说过操作系统调度线程需要随时中断线程的运行并且需要线程被暂停后可以继续运行，操作系统之所以能实现这一点，依靠的就是线程上下文信息。

现在你应该知道哪些是线程私有的了吧。

除此之外，剩下的都是线程间共享资源。

那么剩下的还有什么呢？还有图中的这些。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U76WYIzjJfrQiciaicFja35ywtqpX8AduKvLrFBQPR51lepO5ZAvoYcDmw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这其实就是进程地址空间的样子，也就是说线程共享进程地址空间中除线程上下文信息中的所有内容，意思就是说线程可以**直接读取**这些内容。

接下来我们分别来看一下这些区域。

> **代码区**

进程地址空间中的代码区，这里保存的是什么呢？从名字中有的同学可能已经猜到了，没错，这里保存的就是我们写的代码，**更准确的是编译后的可执行机器指令**。

那么这些机器指令又是从哪里来的呢？答案是从可执行文件中加载到内存的，可执行程序中的代码区就是用来初始化进程地址空间中的代码区的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UtCicgiaqiaFU5qrMibe2XPcGXM9A7x3J69Cl2ncgf6afCn3iaYZCM2Y2FYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

线程之间共享代码区，**这就意味着程序中的任何一个函数都可以放到线程中去执行，不存在某个函数只能被特定线程执行的情况**。

> **数据区**

进程地址空间中的数据区，这里存放的就是所谓的全局变量。

什么是全局变量？所谓全局变量就是那些你定义在函数之外的变量，在C语言中就像这样：



```c
char c; // 全局变量

void func() {

}
```

其中字符c就是全局变量，存放在进程地址空间中的数据区。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UEtibNTe7gC9X9ett0gA8EcDU9TTg93CqGQKoqc4e81RG9p8KDYZwgSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在程序员运行期间，也就是run time，**数据区中的全局变量有且仅有一个实例，所有的线程都可以访问到该全局变量**。

值得注意的是，在C语言中还有一类特殊的“全局变量”，那就是用static关键词修饰过的变量，就像这样：

```c
void func(){
    static int a = 10;
}
```

注意到，**虽然变量a定义在函数内部，但变量a依然具有全局变量的特性**，也就是说变量a放在了进程地址空间的数据区域，**即使函数执行完后该变量依然存在**，而普通的局部变量随着函数调用结束和函数栈帧一起被回收掉了，但这里的变量a不会被回收，因为其被放到了数据区。

这样的变量对每个线程来说也是可见的，也就是说每个线程都可以访问到该变量。

> **堆区**

堆区是程序员比较熟悉的，我们在C/C++中用malloc或者new出来的数据就存放在这个区域，很显然，**只要知道变量的地址，也就是指针，任何一个线程都可以访问指针指向的数据**，因此堆区也是线程共享的属于进程的资源。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UY1zP3O4nm60RaE1iaRj5mHaWAPLDYyRVaxuflc3QFzSl8RYlh9eK5hg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **栈区**

唉，等等！刚不是说栈区是线程私有资源吗，怎么这会儿又说起栈区了？

确实，从线程这个抽象的概念上来说，栈区是线程私有的，然而从实际的实现上看，**栈区属于线程私有这一规则并没有严格遵守**，这句话是什么意思？

通常来说，注意这里的用词是**通常**，通常来说栈区是线程私有，既然有通常就有不通常的时候。

不通常是因为不像进程地址空间之间的严格隔离，线程的栈区没有严格的隔离机制来保护，因此如果一个线程能拿到来自另一个线程栈帧上的指针，**那么该线程就可以改变另一个线程的栈区**，也就是说这些线程可以任意修改本属于另一个线程栈区中的变量。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U6DwtO4rCcxRGcu5opoTjgUjHbE0SplTJ5lDPiaghICz30vwkuy89qZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这从某种程度上给了程序员极大的便利，但同时，这也会导致极其难以排查到的bug。

试想一下你的程序运行的好好的，结果某个时刻突然出问题，定位到出问题代码行后根本就排查不到原因，你当然是排查不到问题原因的，因为你的程序本来就没有任何问题，是别人的问题导致你的函数栈帧数据被写坏从而产生bug，这样的问题通常很难排查到原因，需要对整体的项目代码非常熟悉，常用的一些debug工具这时可能已经没有多大作用了。

说了这么多，那么同学可能会问，一个线程是怎样修改本属于其它线程的数据呢？

接下来我们用一个代码示例讲解一下。

> **修改线程私有数据**

不要担心，以下代码足够简单：

```c
void thread(void* var) {
    int* p = (int*)var;
    *p = 2;
}

int main() {
    int a = 1;
    pthread_t tid;

    pthread_create(&tid, NULL, thread, (void*)&a);
    return 0;
}
```

这段代码是什么意思呢？

首先我们在主线程的栈区定义了一个局部变量，也就是 int a= 1这行代码，现在我们已经知道了，局部变量a属于主线程私有数据，但是，接下来我们创建了另外一个线程。

在新创建的这个线程中，我们将变量a的地址以参数的形式传给了新创建的线程，然后我来看一下thread函数。

在新创建的线程中，我们获取到了变量a的指针，然后将其修改为了2，也就是这行代码，我们在新创建的线程中修改了本属于主线程的私有数据。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U7JUibLhQnz6ULEKEPrgjHbhzAicXCiaPKboRRMvxNtIAkFPoSWtXYnicNg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在你应该看明白了吧，尽管栈区是线程的私有数据，但由于栈区没有添加任何保护机制，一个线程的栈区对其它线程是可以见的，也就是说我们可以修改属于任何一个线程的栈区。

就像我们上文说得到的，这给程序员带来了极大便利的同时也带来了无尽的麻烦，试想上面这段代码，如果确实是项目需要那么这样写代码无可厚非，但如果上述新创建线程是因bug修改了属于其它线程的私有数据的话，那么产生问题就很难定位了，**因为bug可能距离问题暴露的这行代码已经很远了**，这样的问题通常难以排查。

> **动态链接库**

进程地址空间中除了以上讨论的这些实际上还有其它内容，还有什么呢？

这就要从可执行程序说起了。

什么是可执行程序呢？在Windows中就是我们熟悉的exe文件，在Linux世界中就是ELF文件，这些可以被操作系统直接运行的程序就是我们所说的可执行程序。

那么可执行程序是怎么来的呢？

有的同学可能会说，废话，不就是编译器生成的吗？

实际上这个答案只答对了一半。

假设我们的项目比较简单只有几个源码文件，编译器是怎么把这几个源代码文件转换为最终的一个可执行程序呢？

原来，编译器在将可执行程序翻译成机器指令后，接下来还有一个重要的步骤，这就是链接，链接完成后生成的才是可执行程序。

完成链接这一过程的就是链接器。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UwvIibKUkp3PExcAMtf1mugIcwGYR0BLzkT3RHxFCLMon1SHteXyM3Lg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中链接器可以有两种链接方式，这就是**静态链接**和**动态链接**。

静态链接的意思是说把所有的机器指令一股脑全部打包到可执行程序中，动态链接的意思是我们不把动态链接的部分打包到可执行程序，而是在可执行程序运行起来后去内存中找动态链接的那部分代码，这就是所谓的静态链接和动态链接。

动态链接一个显而易见的好处就是可执行程序的大小会很小，就像我们在Windows下看一个exe文件可能很小，**那么该exe很可能是动态链接的方式生成的**。

而动态链接的部分生成的库就是我们熟悉的动态链接库，在Windows下是以DLL结尾的文件，在Linux下是以so结尾的文件。

说了这么多，这和线程共享资源有什么关系呢？

原来如果一个程序是动态链接生成的，**那么其地址空间中有一部分包含的就是动态链接库**，否则程序就运行不起来了，这一部分的地址空间也是被所有线程所共享的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UJtnywWE8NIFjwsGwH6UV0EldGXnZjQ7p2fKVDAv3YKCicGP2MpBZsXg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

也就是说进程中的所有线程都可以使用动态链接库中的代码。

以上其实是关于链接这一主题的极简介绍，关于链接这一话题的详细讨论可以参考《[彻底理解链接器](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483677&idx=1&sn=09212e8e7ecf7d58bfee53fa04e74911&chksm=fcb98643cbce0f5503916804ffe95bdd30917c9829429bb7fca7191b84f8171409db83e6e28c&scene=21#wechat_redirect)》系列文章。

> **文件**

最后，如果程序在运行过程中打开了一些文件，那么进程地址空间中还保存有打开的文件信息，进程打开的文件也可以被所有的线程使用，这也属于线程间的共享资源。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UuuPsC5SXGR6nAEHyIsHiajCBxqicGP8bvTgwC1UBBTXFe4T0p6uYQPbg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **One More Thing：TLS**

本文就这些了吗？

实际上关于线程私有数据还有一项没有详细讲解，因为再讲下去本篇就撑爆了，而且本篇已经讲解的部分足够用了，剩下的这一点仅仅作为补充，也就是选学部分，如果你对此不感兴趣的话完全可以跳过，没有问题![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya3FQ7uPTlj2CkqH7qdNaDEsEn3N6lhOGzAE3PSVvkoJHcxDF6bXFEKfwjqdN33YsLliaGLKC9oAgxw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)。

关于线程私有数据还有一项技术，那就是线程局部存储，Thread Local Storage，TLS。

这是什么意思呢？

其实从名字上也可以看出，所谓线程局部存储，是指存放在该区域中的变量有两个含义：

- 存放在该区域中的变量是全局变量，所有线程都可以访问
- 虽然看上去所有线程访问的都是同一个变量，但该全局变量独属于一个线程，一个线程对此变量的修改对其他线程不可见。

说了这么多还是没懂有没有？没关系，接下来看完这两段代码还不懂你来打我。

我们先来看第一段代码，不用担心，这段代码非常非常的简单：

```c
int a = 1; // 全局变量

void print_a() {
    cout<<a<<endl;
}

void run() {
    ++a;
    print_a();
}

void main() {
    thread t1(run);
    t1.join();

    thread t2(run);
    t2.join();
}
```

怎么样，这段代码足够简单吧，上述代码是用C++11写的，我来讲解下这段代码是什么意思。

- 首先我们创建了一个全局变量a，初始值为1
- 其次我们创建了两个线程，每个线程对变量a加1
- 线程的join函数表示该线程运行完毕后才继续运行接下来的代码

那么这段代码的运行起来会打印什么呢？

全局变量a的初始值为1，第一个线程加1后a变为2，因此会打印2；第二个线程再次加1后a变为3，因此会打印3，让我们来看一下运行结果：

```c
2
3
```

看来我们分析的没错，全局变量在两个线程分别加1后最终变为3。

接下来我们对变量a的定义稍作修改，其它代码不做改动：

```
__thread int a = 1; // 线程局部存储
```

我们看到全局变量a前面加了一个__thread关键词用来修饰，也就是说我们告诉编译器把变量a放在线程局部存储中，那这会对程序带来哪些改变呢？

简单运行一下就知道了：



```c
2
2
```

和你想的一样吗？有的同学可能会大吃一惊，为什么我们明明对变量a加了两次，但第二次运行为什么还是打印2而不是3呢？

想一想这是为什么。

原来，这就是线程局部存储的作用所在，线程t1对变量a的修改不会影响到线程t2，线程t1在将变量a加到1后变为2，但对于线程t2来说此时变量a依然是1，因此加1后依然是2。

因此，**线程局部存储可以让你使用一个独属于线程的全局变量**。也就是说，虽然该变量可以被所有线程访问，但该变量在每个线程中都有一个副本，一个线程对改变量的修改不会影响到其它线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UvpTAlrZbo2MKIiaZhuMJWOJIcE7N1Ahy8HgMARSwBTGzAzDm9LtW1ibg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **总结**

怎么样，没想到教科书上一句简单的“线程共享进程资源”背后竟然会有这么多的知识点吧，**教科书上的知识看似容易，但，并不简单**。



#   Linux/C进程与线程



## 进程

全文脉络思维导图如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9x0wgXBMLYGsibCWVjiaCOn0fHO06Cfj4jYuOEPWAUrKHJDJID222B328A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###   进程与线程的简单解释

进程（Process）和线程（Thread）是操作系统的基本概念，但是它们比较抽象，不容易掌握。以下这个解释出自阮一峰老师的博客（http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html），虽然**「不是非常严谨，但是足够形象」**，看完之后能对进程和线程有个非常直观的印象，这样也方便理解后文。

① 计算机的核心是 CPU，它承担了所有的计算任务。它就像一座工厂，时刻在运行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xicmG6b8LWelyBbHPCexMQAjyrxlmbicQQBPM9k85s9P6mgnMGB9TvXYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

② **「假定工厂的电力有限，一次只能供给一个车间使用」**。也就是说，一个车间开工的时候，其他车间都必须停工。背后的含义就是，单个 CPU 一次只能运行一个任务。

③ 进程就好比工厂的车间，它代表 CPU 所能处理的单个任务。任一时刻，CPU 总是运行一个进程，其他进程处于非运行状态。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xb5m6WEticW2ryEoqicHCFrrUVHH0APZiblkhJ6Hd6x1EFROjBYCj7o3wg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

④ 一个车间里，可以有很多工人。他们协同完成一个任务。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xl0QhD4F48lsq1mVdK9DaAauozRLMg7cGUlu5RF4VrLDdB56YpPibCqA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑤ 线程就好比车间里的工人。一个进程可以包括多个线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xDCI3aZDibXyldQrAKJggsUF70KXKHHwfTjNdYRwrUd7LHJ63Hz8qTeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑥ 车间的空间是工人们共享的，比如许多房间是每个工人都可以进出的。这象征一个进程的内存空间是共享的，每个线程都可以使用这些共享内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xSutVmJvVke0rq1H5Flb4CgdGtFMbnyAaYby26xRkyagAKjMdrcQibCA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑦ 可是，每间房间的大小不同，有些房间最多只能容纳一个人，比如厕所。里面有人的时候，其他人就不能进去了。这代表一个线程使用某些共享内存时，其他线程必须等它结束，才能使用这一块内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xQY2mZxz5UJXfBvtB8xznKa3d2SKM6S4WxthT1ZDlRotffdrHFkjdOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑧ 一个防止他人进入的简单方法，就是门口加一把锁。先到的人锁上门，后到的人看到上锁，就在门口排队，等锁打开再进去。这就叫"互斥锁"（Mutual exclusion，缩写 Mutex），防止多个线程同时读写某一块内存区域。

⑨ 还有些房间，可以同时容纳 n 个人，比如厨房。也就是说，如果人数大于 n，多出来的人只能在外面等着。这好比某些内存区域，只能供给固定数目的线程使用。

⑩ 这时的解决方法，就是在门口挂 n 把钥匙。进去的人就取一把钥匙，出来时再把钥匙挂回原处。后到的人发现钥匙架空了，就知道必须在门口排队等着了。这种做法叫做 "信号量"（Semaphore），用来保证多个线程不会互相冲突。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xO1CTXibJdxQF8R90O0aSQXib71MS1xemPZedtB0lej1bgCN2CiaX6aNZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

不难看出，互斥锁 Mutex 是信号量 semaphore 的一种特殊情况（n = 1时）。也就是说，完全可以用后者替代前者。但是，因为 Mutex 较为简单，且效率高，所以在必须保证资源独占的情况下，还是采用这种设计。

###  什么是进程

结合上文的简单解释，下面给出进程的科学定义：**「进程是程序在某个数据集合上的一次运行活动，也是操作系统进行资源分配和保护的基本单位」**。

通俗来说，**「进程就是程序的一次执行过程」**，程序是静态的，它作为系统中的一种资源是永远存在的。而进程是动态的，它是动态的产生，变化和消亡的，拥有其自己的生命周期。

举个例子：同时挂三个 QQ 号，它们就对应三个 QQ 进程，退出一个就会杀死一个对应的进程。但是，就算你把这三个 QQ 全都退出了，QQ 这个程序死亡了吗？显然没有。

进程不仅包含正在运行的程序实体，并且包括这个运行的程序中占据的所有系统资源，比如说 CPU、内存、网络资源等。很多小伙伴在回答进程的概念的时候，往往只会说它是一个运行的实体，而会忽略掉进程所占据的资源。比如说，同样一个程序，同一时刻被两次运行了，那么他们就是两个独立的进程。

###  进程的组成

进程主要由三个部分组成：

1）**「进程控制块 PCB」**。包含如下几个部分：

- 进程描述信息
- 进程控制和管理信息
- 资源分配清单
- CPU 相关信息

2）**「数据段」**。即进程运行过程中各种数据（比如程序中定义的变量）

3）**「程序段」**。就是程序的代码（指令序列）

举个例子：同时挂三个 QQ 号，会对应三个 QQ 进程，它们的 PCB、数据段各不相同，但程序段的内容都是相同的 （都是运行着相同的 QQ 程序）

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xTwjPiaGDH9nXbyQpAhU3tD0ZPVgmsVUbwIFOBxLOibwIv366rhS4yhDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

PCB 是提供给操作系统用的，而程序段、数据段是给进程自己用的。

> 进程控制块 PCB

每个进程有且仅有一个进程控制块（Process Control Block，PCB），或称进程描述符，它是**「进程存在的唯一标识」**，是**「操作系统用来记录和刻画进程状态及环境信息的数据结构」**，也是操作系统掌握进程的唯一资料结构和管理进程的主要依据。所以说 PCB 是提供给操作系统使用的。

通俗的解释：操作系统需要对各个进程进行管理，但凡管理时所需要的信息，都会被放在 PCB 中，**「PCB 是进程存在的唯一标志」**。创建进程和撤销进程等都是指对 PCB 的操作，当进程被创建时，操作系统为其创建 PCB，当进程结束时，会回收其 PCB。

一般来说，PCB 会包含如下四类信息：

1）**「进程描述信息」**：用来让操作系统区分各个进程

- 当进程被创建时，操作系统会为该进程分配一个唯一的、不重复的 “身份证号”— **「PID」**（ProcessID，进程 ID）
- 另外，进程描述信息还包含进程所属的用户 ID（**「UID」**）

2）**「进程控制和管理信息」**：记录进程的运行情况。比如 CPU 的使用时间、磁盘使用情况、网络流量使用情况等。

3）**「资源分配清单」**：记录给进程分配了哪些资源。比如分配了多少内存、正在使用哪些 I/O 设备、正在使用哪些文件等。

4）**「CPU 相关信息」**：进程在让出 CPU 时，必须保存该进程在 CPU 中的各种信息，比如各种寄存器的值。用于实现进程切换，确保这个进程再次运行的时候恢复 CPU 现场，从断点处继续执行。这就是所谓的**「保存现场信息」**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xBfLA3GdgUB9j2Xxh1WQkYXgqTZ14EtGwavicMrfgU42NyRcUv1nEq5A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###  进程的状态

尽管每一个进程都是独立的实体，有其自己的 PCB 和内部状态，但是进程之间经常需要相互作用。一个进程的输出结果可能是另一个进程的输入。假设进程 A 的输入依赖进程 B 的输出，那么在进程 B 的输出结果没有出来之前，进程 A 就无法执行，它就会被阻塞。这就是进程的阻塞态。

经典的进程三态模型如下：

- **「运行态」**（running）：进程占有 CPU 正在运行。
- **「就绪态」**（ready）：进程具备运行条件，等待系统分配 CPU 以便运行。
- **「阻塞态」** / 等待态（wait）：进程不具备运行条件，正在等待某个事件的完成。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xiaoKuicdsjCCD5Mibu4Rqp4JWewDyiaAqJ4Hf5nKBbPIib71LVqia0tHbgMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上图中的时间片用完，可以这样理解：

进程是并发执行的嘛，宏观上在一段时间内能同时运行多个程序，但其实微观上是交替发生的。也就是说 CPU 一般不会让一个进程一次性执行完，为了保证所有进程可以得到公平调度，CPU 时间被划分为一段段的时间片，这些时间片再被轮流分配给各个进程。某个进程的时间片用完后这个进程就会进入就绪态，而其他被分配到时间片的进程就会进入运行态。这个处于就绪态的进程就需要等待进程调度程序的下一次调度，为其分配 CPU 时间片后才能再次恢复运行。

需要注意的是：**「阻塞态是由于缺少需要的资源从而由运行态转换而来，但是该资源不包括 CPU 时间片，缺少 CPU 时间片会从运行态转换为就绪态」**。

很多系统中都增加了新建态（new）和终止态（exit），形成**「五态模型」**：

- **「新建态」**（new）：进程正在被创建时的状态
- **「终止态」**（exit）：进程正在从系统中消失时的状态

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xM83HUYMBulpXicIj1JFVuueKv5pA1gDQFOZ4gAzbBLhdpujNaic8QGlQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从上图可以发现，**「只有就绪态和运行态可以相互转换，其它的都是单向转换」**。

这些不同状态的进程操作系统是如何进行管理的呢？上文说过，PCB 是提供给操作系统使用的，是操作系统管理进程的主要依据。没错，操作就是通过 PCB 来管理这些拥有不同状态的进程的。

进程的 PCB 会通过某种方式组织起来，一般来说，操作系统会把处于同一状态的所有进程的 PCB 链接在一起，这种数据结构就称为**「进程队列」**（Process Queue）。

### 进程控制

所谓进程控制就是对系统中的所有进程实施有效的管理，**「实现进程状态转换」**功能。包括创建进程、阻塞进程、唤醒进程、终止进程等，这些功能均由**「原语」**来实现，操作系统通过原语来完成进程原理，包括进程的同步和互斥、进程的通信和管理。

**「什么是原语」**？原语是一种特殊的程序，它的执行具有**「原子性」**。也就是说，这段程序的运行必须一气呵成，不可中断。原语是操作系统内核里的一段程序：

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9x3zWqAWibnicaXaj5btBEG4NUUs7RZB6o8fdrlgaq9fbUxmqR67YDADpA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

思考一下：为什么进程控制（进程状态转换）的过程要一气呵成，不可中断？

答：如果进程状态转换的过程不能一气呵成，就有可能导致操作系统中的某些关键数据结构信息不统一，这会影响操作系统进行别的管理工作。

#### 进程的创建

操作系统初始启动时会创建承担系统资源分配和控制管理的一些系统进程，同时还会创建一个所有用户进程的祖先，其他用户进程是在应用程序运行时创建的。

操作系统允许一个进程创建另一个进程，而且允许子进程继承父进程所拥有的资源，当子进程被终止时，其在父进程处继承的资源应当还给父进程。同时，终止父进程时同时也会终止其所有的子进程。

创建进程的过程，也就是**「创建原语」**包含的内容如下：

- 在进程列表中增加一项，从 PCB 池中申请一个空闲的 PCB（PCB 是有限的，若申请失败则创建失败），为新进程分配一个唯一的进程标识符；
- 为新进程分配地址空间，由进程管理程序确定加载至进程地址空间中的程序；
- 为新进程分配各种资源；
- 初始化 PCB，如进程标识符、CPU 初始状态等；
- 把新进程的状态设置为就绪态，并将其移入就绪队列，等待被调度运行。

**「什么事件会触发进程的创建呢」**？有如下四种情况：

- 用户登录：分时系统中，用户登录成功，系统会为其建立一个新的进程
- 作业调度：多道批处理系统中，有新的作业放入内存中，会为其建立一个新的进程
- 提供服务：用户向操作系统提出某些请求时，会新建一个进程处理该请求
- 应用请求：由用户进程主动请求创建一个子进程

#### 进程的终止

进程的终止也称为撤销，进程完成特定工作或出现严重错误后必须被终止。引起进程终止的事件有三种：

- 正常结束：进程自己请求终止（exit 系统调用）
- 异常结束：比如整数除 0，非法使用特权指令，然后被操作系统强行终止
- 外界干预：Ctrl + Alt + delete 打开进程管理器，用户手动杀死进程

终止（撤销）进程的过程，也就是**「撤销原语」**包含的内容如下：

- 从 PCB 集合中找到终止进程的 PCB；
- 若进程处于运行态，则立即剥夺其 CPU，终止该进程的执行，然后将 CPU 资源分配给其他进程；
- 如果其还有子进程，则应将其所有子进程终止；
- 将该进程所拥有的全部资源都归还给父进程或操作系统；
- 回收 PCB 并将其归还至 PCB 池。

#### 进程的阻塞和唤醒

进程阻塞是指进程让出 CPU 资源转而等待一个事件，如等待资源、等待 I/O 操作完成等。进程通常使用阻塞原语来阻塞自己，所以阻塞是进程的自主行为，是一个同步事件。当等待事件完成时会产生一个中断，激活操作系统，在系统的控制下将被阻塞的进程唤醒，也就是唤醒原语。

进程的阻塞和唤醒显然是由进程切换来完成的。

进程的阻塞步骤，也就是**「阻塞原语」**的内容为：

- 找到将要被阻塞的进程对应的 PCB；
- 保护进程运行现场，将 PCB 状态信息设置为阻塞态，暂时停止进程运行；
- 将该 PCB 插入相应事件的阻塞队列（等待队列）。

进程的唤醒步骤，也就是**「唤醒原语」**的内容为：

- 在该事件的阻塞队列中找到相应进程的 PCB；
- 将该 PCB 从阻塞队列中移出，并将进程的状态设置为就绪态；
- 把该 PCB 插入到就绪队列中，等待被调度程序调度。

阻塞原语和唤醒原语的作用正好相反，**「阻塞原语使得进程从运行态转为阻塞态，而唤醒原语使得进程从阻塞态转为就绪态」**。如果某个进程使用阻塞原语来阻塞自己，那么他就必须使用唤醒原语来唤醒自己，因何事阻塞，就由何事唤醒，否则被阻塞的进程将永远处于阻塞态。因此，**「阻塞原语和唤醒原语是成对出现的」**。

###   进程上下文切换

所谓进程的上下文切换，就是说各个进程之间是共享 CPU 资源的，不可能一个进程永远占用着 CPU 资源，不同的时候进程之间需要切换，使得不同的进程被分配 CPU 资源，这个过程就是进程的上下文切换，**「一个进程切换到另一个进程运行」**。

因为进程是由内核进行管理和调度的，所以**「进程的上下文切换一定发生在内核态」**。

进程上下文的切换也是一个原语操作，称为**「切换原语」**，其内容如下：

- 首先，将进程 A 的运行环境信息存入 PCB，这个运行环境信息就是进程的上下文（Context）
- 然后，将 PCB 移入相应的进程队列；
- 选择另一个进程 B 进行执行，并更新其 PCB 中的状态为运行态
- 当进程 A 被恢复运行的时候，根据它的 PCB 恢复进程 A 所需的运行环境

引起进程上下文切换的事件，也就是某个占用 CPU 资源运行的当前进程被赶出 CPU 的原因有如下：

- 当前进程的时间片到
- 有更高优先级的进程到达
- 当前进程主动阻塞
- 当前进程终止





###  [Linux 中进程管理系统调用](https://mp.weixin.qq.com/s/Yb2AhzXVt-waLtQL9hVXEg)

现在关注一下 Linux 系统中与进程管理相关的系统调用。在了解之前你需要先知道一下什么是系统调用。

操作系统为我们屏蔽了硬件和软件的差异，它的最主要功能就是为用户提供一种抽象，隐藏内部实现，让用户只关心在 GUI 图形界面下如何使用即可。操作系统可以分为两种模式

- 内核态：操作系统内核使用的模式
- 用户态：用户应用程序所使用的模式

我们常说的`上下文切换` 指的就是内核态模式和用户态模式的频繁切换。而`系统调用`指的就是引起内核态和用户态切换的一种方式，系统调用通常在后台静默运行，表示计算机程序向其操作系统内核请求服务。

系统调用指令有很多，下面是一些与进程管理相关的最主要的系统调用。

> fork

fork 调用用于创建一个与父进程相同的子进程，创建完进程后的子进程拥有和父进程一样的程序计数器、相同的 CPU 寄存器、相同的打开文件。



> exec

exec 系统调用用于执行驻留在活动进程中的文件，调用 exec 后，新的可执行文件会替换先前的可执行文件并获得执行。也就是说，调用 exec 后，会将旧文件或程序替换为新文件或执行，然后执行文件或程序。新的执行程序被加载到相同的执行空间中，因此进程的 `PID`不会修改，因为我们**没有创建新进程，只是替换旧进程**。但是进程的数据、代码、堆栈都已经被修改。如果当前要被替换的进程包含多个线程，那么所有的线程将被终止，新的进程映像被加载执行。

这里需要解释一下`进程映像(Process image)` 的概念

**什么是进程映像呢**？进程映像是执行程序时所需要的可执行文件，通常会包括下面这些东西

- **代码段（codesegment/textsegment）**

又称文本段，用来存放指令，运行代码的一块内存空间

此空间大小在代码运行前就已经确定

内存空间一般属于只读，某些架构的代码也允许可写

在代码段中，也有可能包含一些只读的常数变量，例如字符串常量等。

- **数据段（datasegment）**

可读可写

存储初始化的全局变量和初始化的 static 变量

数据段中数据的生存期是随程序持续性（随进程持续性） 随进程持续性：进程创建就存在，进程死亡就消失

- **bss 段（bsssegment）：**

可读可写

存储未初始化的全局变量和未初始化的 static 变量

bss 段中的数据一般默认为 0

- **Data 段**

是可读写的，因为变量的值可以在运行时更改。此段的大小也固定。

- **栈（stack）：**

可读可写

存储的是函数或代码中的局部变量(非 static 变量)

栈的生存期随代码块持续性，代码块运行就给你分配空间，代码块结束，就自动回收空间

- **堆（heap）：**

可读可写

存储的是程序运行期间动态分配的 malloc/realloc 的空间

堆的生存期随进程持续性，从 malloc/realloc 到 free 一直存在

下面是这些区域的构成图

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtSDG2dd5po5BOWDpFCIL9qSsvY2P7ZjBppD97UWKy6ic1eFzjKBcfwZw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

exec 系统调用是一些函数的集合，这些函数是

- execl
- execle
- execlp
- execv
- execve
- execvp

下面来看一下 exec 的工作原理

1. 当前进程映像被替换为新的进程映像
2. 新的进程映像是你做为 exec 传递的参数
3. 结束当前正在运行的进程
4. 新的进程映像有 PID，相同的环境和一些文件描述符(因为未替换进程，只是替换了进程映像)
5. CPU 状态和虚拟内存受到影响，当前进程映像的虚拟内存映射被新进程映像的虚拟内存代替。

> waitpid

等待子进程结束或终止

> exit

在许多计算机操作系统上，计算机进程的终止是通过执行 `exit` 系统调用命令执行的。0 表示进程能够正常结束，其他值表示进程以非正常的行为结束。

其他一些常见的系统调用如下

| 系统调用指令 | 描述                             |
| :----------- | :------------------------------- |
| pause        | 挂起信号                         |
| nice         | 改变分时进程的优先级             |
| ptrace       | 进程跟踪                         |
| kill         | 向进程发送信号                   |
| pipe         | 创建管道                         |
| mkfifo       | 创建 fifo 的特殊文件（命名管道） |
| sigaction    | 设置对指定信号的处理方法         |
| msgctl       | 消息控制操作                     |
| semctl       | 信号量控制                       |





## 线程

现在我们来讨论一下 Linux 中的线程，线程是轻量级的进程，想必这句话你已经听过很多次了，`轻量级`体现在所有的进程切换都需要清除所有的表、进程间的共享信息也比较麻烦，一般来说通过管道或者共享内存，如果是 fork 函数后的父子进程则使用共享文件，然而线程切换不需要像进程一样具有昂贵的开销，而且线程通信起来也更方便。线程分为两种：用户级线程和内核级线程

### 用户级线程

用户级线程避免使用内核，通常，每个线程会显示调用开关，发送信号或者执行某种切换操作来放弃 CPU，同样，计时器可以强制进行开关，用户线程的切换速度通常比内核线程快很多。在用户级别实现线程会有一个问题，即单个线程可能会垄断 CPU 时间片，导致其他线程无法执行从而 `饿死`。如果执行一个 I/O 操作，那么 I/O 会阻塞，其他线程也无法运行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtKyhzS8jyhliakaUUuNxeFg6grxWyNkgBZsOss4WhgcvhhACdtjzhiaiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

一种解决方案是，一些用户级的线程包解决了这个问题。可以使用时钟周期的监视器来控制第一时间时间片独占。然后，一些库通过特殊的包装来解决系统调用的 I/O 阻塞问题，或者可以为非阻塞 I/O 编写任务。

### 内核级线程

内核级线程通常使用几个进程表在内核中实现，每个任务都会对应一个进程表。在这种情况下，内核会在每个进程的时间片内调度每个线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOt5BCuQ5qDL0nic412MZLJXr7y6j8MkG15Wictw6PEmMPOoMK5A2t345dQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

所有能够阻塞的调用都会通过系统调用的方式来实现，当一个线程阻塞时，内核可以进行选择，是运行在同一个进程中的另一个线程（如果有就绪线程的话）还是运行一个另一个进程中的线程。

从用户空间 -> 内核空间 -> 用户空间的开销比较大，但是线程初始化的时间损耗可以忽略不计。这种实现的好处是由时钟决定线程切换时间，因此不太可能将时间片与任务中的其他线程占用时间绑定到一起。同样，I/O 阻塞也不是问题。

### 混合实现

结合用户空间和内核空间的优点，设计人员采用了一种`内核级线程`的方式，然后将用户级线程与某些或者全部内核线程多路复用起来

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtibHUBALDR6SFTzX0AXrPmTzzyOjjBVRclyMNuO2IIooUQGAjjWN32jQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在这种模型中，编程人员可以自由控制用户线程和内核线程的数量，具有很大的灵活度。采用这种方法，内核只识别内核级线程，并对其进行调度。其中一些内核级线程会被多个用户级线程多路复用。







## Linux 进程间通信

Linux 进程间的通信机制通常被称为 `Internel-Process communication,IPC`下面我们来说一说 Linux 进程间通信的机制，大致来说，Linux 进程间的通信机制可以分为 6 种

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtP5IhMLrNop6iaPNJrKWw6zF8Cia97FdkkpXicVibwVWQYGkNXN6ibwnAPgA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

下面我们分别对其进行概述

### <span id="信号2">信号 signal</span>

信号是 UNIX 系统最先开始使用的进程间通信机制，因为 Linux 是继承于 UNIX 的，所以 Linux 也支持信号机制，通过向一个或多个进程发送`异步事件信号`来实现，信号可以从键盘或者访问不存在的位置等地方产生；信号通过 shell 将任务发送给子进程。

你可以在 Linux 系统上输入 `kill -l` 来列出系统使用的信号，下面是我提供的一些信号

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOt3hAicRftdqkewydkbQhb4ZA8xZTTUZVS3dHp4EpYsg3Pia2gSvkvzWSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

进程可以选择忽略发送过来的信号，但是有两个是不能忽略的：`SIGSTOP` 和 `SIGKILL` 信号。SIGSTOP 信号会通知当前正在运行的进程执行关闭操作，SIGKILL 信号会通知当前进程应该被杀死。除此之外，进程可以选择它想要处理的信号，进程也可以选择阻止信号，如果不阻止，可以选择自行处理，也可以选择进行内核处理。如果选择交给内核进行处理，那么就执行默认处理。

操作系统会中断目标程序的进程来向其发送信号、在任何非原子指令中，执行都可以中断，如果进程已经注册了新号处理程序，那么就执行进程，如果没有注册，将采用默认处理的方式。

例如：当进程收到 `SIGFPE` 浮点异常的信号后，默认操作是对其进行 `dump(转储)`和退出。信号没有优先级的说法。如果同时为某个进程产生了两个信号，则可以将它们呈现给进程或者以任意的顺序进行处理。

下面我们就来看一下这些信号是干什么用的

- SIGABRT 和 SIGIOT

SIGABRT 和 SIGIOT 信号发送给进程，告诉其进行终止，这个 信号通常在调用 C标准库的`abort()`函数时由进程本身启动

- SIGALRM 、 SIGVTALRM、SIGPROF

当设置的时钟功能超时时会将 SIGALRM 、 SIGVTALRM、SIGPROF 发送给进程。当实际时间或时钟时间超时时，发送 SIGALRM。当进程使用的 CPU 时间超时时，将发送 SIGVTALRM。当进程和系统代表进程使用的CPU 时间超时时，将发送 SIGPROF。

- SIGBUS

SIGBUS 将造成`总线中断`错误时发送给进程

- SIGCHLD

当子进程终止、被中断或者被中断恢复，将 SIGCHLD 发送给进程。此信号的一种常见用法是指示操作系统在子进程终止后清除其使用的资源。

- SIGCONT

SIGCONT 信号指示操作系统继续执行先前由 SIGSTOP 或 SIGTSTP 信号暂停的进程。该信号的一个重要用途是在 Unix shell 中的作业控制中。

- SIGFPE

SIGFPE 信号在执行错误的算术运算（例如除以零）时将被发送到进程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtrvAYhGPeUVoHxmrxKAIhhcfc3uXX3hfXqRLBozEjyh01qdkkOqjySw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- SIGUP

当 SIGUP 信号控制的终端关闭时，会发送给进程。许多守护程序将重新加载其配置文件并重新打开其日志文件，而不是在收到此信号时退出。

- SIGILL

SIGILL 信号在尝试执行非法、格式错误、未知或者特权指令时发出

- SIGINT

当用户希望中断进程时，操作系统会向进程发送 SIGINT 信号。用户输入 ctrl - c 就是希望中断进程。

- SIGKILL

SIGKILL 信号发送到进程以使其马上进行终止。与 SIGTERM 和 SIGINT 相比，这个信号无法捕获和忽略执行，并且进程在接收到此信号后无法执行任何清理操作，下面是一些例外情况

僵尸进程无法杀死，因为僵尸进程已经死了，它在等待父进程对其进行捕获

处于阻塞状态的进程只有再次唤醒后才会被 kill 掉

`init` 进程是 Linux 的初始化进程，这个进程会忽略任何信号。

SIGKILL 通常是作为最后杀死进程的信号、它通常作用于 SIGTERM 没有响应时发送给进程。

- SIGPIPE

SIGPIPE 尝试写入进程管道时发现管道未连接无法写入时发送到进程

- SIGPOLL

当在明确监视的文件描述符上发生事件时，将发送 SIGPOLL 信号。

- SIGRTMIN 至 SIGRTMAX

SIGRTMIN 至 SIGRTMAX 是`实时信号`

- SIGQUIT

当用户请求退出进程并执行核心转储时，SIGQUIT 信号将由其控制终端发送给进程。

- SIGSEGV

当 SIGSEGV 信号做出无效的虚拟内存引用或分段错误时，即在执行分段违规时，将其发送到进程。

- SIGSTOP

SIGSTOP 指示操作系统终止以便以后进行恢复时

- SIGSYS

当 SIGSYS 信号将错误参数传递给系统调用时，该信号将发送到进程。

- SIGTERM

我们上面简单提到过了 SIGTERM 这个名词，这个信号发送给进程以请求终止。与 SIGKILL 信号不同，该信号可以被过程捕获或忽略。这允许进程执行良好的终止，从而释放资源并在适当时保存状态。SIGINT 与SIGTERM 几乎相同。

- SIGTSIP

SIGTSTP 信号由其控制终端发送到进程，以请求终端停止。

- SIGTTIN 和 SIGTTOU

当 SIGTTIN 和SIGTTOU 信号分别在后台尝试从 tty 读取或写入时，信号将发送到该进程。

- SIGTRAP

在发生异常或者 trap 时，将 SIGTRAP 信号发送到进程

- SIGURG

当套接字具有可读取的紧急或带外数据时，将 SIGURG 信号发送到进程。

- SIGUSR1 和 SIGUSR2

SIGUSR1 和 SIGUSR2 信号被发送到进程以指示用户定义的条件。

- SIGXCPU

当 SIGXCPU 信号耗尽 CPU 的时间超过某个用户可设置的预定值时，将其发送到进程

- SIGXFSZ

当 SIGXFSZ 信号增长超过最大允许大小的文件时，该信号将发送到该进程。

- SIGWINCH

SIGWINCH 信号在其控制终端更改其大小（窗口更改）时发送给进程。

### 管道 pipe

Linux 系统中的进程可以通过建立管道 pipe 进行通信

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtrHFf4S6ibmfyq2N85zFxdW8IgnkMxyF2cvblHC3Kbbe3a8BMpH7pyqQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在两个进程之间，可以建立一个通道，一个进程向这个通道里写入字节流，另一个进程从这个管道中读取字节流。管道是同步的，当进程尝试从空管道读取数据时，该进程会被阻塞，直到有可用数据为止。shell 中的`管线 pipelines` 就是用管道实现的，当 shell 发现输出

```
sort <f | head
```

它会创建两个进程，一个是 sort，一个是 head，sort，会在这两个应用程序之间建立一个管道使得 sort 进程的标准输出作为 head 程序的标准输入。sort 进程产生的输出就不用写到文件中了，如果管道满了系统会停止 sort 以等待 head 读出数据

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtaXHx7mIsrAmdKzIBwv3vUHbiaXDwoo0L0yuGwehiaficonL2aWTDz3nibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

管道实际上就是 `|`，两个应用程序不知道有管道的存在，一切都是由 shell 管理和控制的。

### 共享内存 shared memory

两个进程之间还可以通过共享内存进行进程间通信，其中两个或者多个进程可以访问公共内存空间。两个进程的共享工作是通过共享内存完成的，一个进程所作的修改可以对另一个进程可见(很像线程间的通信)。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtzOrlYsXSJnzIld0U6fYckSY3YJXF3OY5LFdwx6S81iaficb7WfFcxN4A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在使用共享内存前，需要经过一系列的调用流程，流程如下

- 创建共享内存段或者使用已创建的共享内存段`(shmget())`
- 将进程附加到已经创建的内存段中`(shmat())`
- 从已连接的共享内存段分离进程`(shmdt())`
- 对共享内存段执行控制操作`(shmctl())`

### 先入先出队列 FIFO

先入先出队列 FIFO 通常被称为 `命名管道(Named Pipes)`，命名管道的工作方式与常规管道非常相似，但是确实有一些明显的区别。未命名的管道没有备份文件：操作系统负责维护内存中的缓冲区，用来将字节从写入器传输到读取器。一旦写入或者输出终止的话，缓冲区将被回收，传输的数据会丢失。相比之下，命名管道具有支持文件和独特 API ，命名管道在文件系统中作为设备的专用文件存在。当所有的进程通信完成后，命名管道将保留在文件系统中以备后用。命名管道具有严格的 FIFO 行为

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtqgBofiaKl3ic9yxKX5EQOZibbqpIiaddu5sicGCShAxRJ9xJLyR6CGHbrQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

写入的第一个字节是读取的第一个字节，写入的第二个字节是读取的第二个字节，依此类推。

### 消息队列 Message Queue

一听到消息队列这个名词你可能不知道是什么意思，消息队列是用来描述内核寻址空间内的内部链接列表。可以按几种不同的方式将消息按顺序发送到队列并从队列中检索消息。每个消息队列由 IPC 标识符唯一标识。消息队列有两种模式，一种是`严格模式`， 严格模式就像是 FIFO 先入先出队列似的，消息顺序发送，顺序读取。还有一种模式是 `非严格模式`，消息的顺序性不是非常重要。

### 套接字 Socket

还有一种管理两个进程间通信的是使用 `socket`，socket 提供端到端的双相通信。一个套接字可以与一个或多个进程关联。就像管道有命令管道和未命名管道一样，套接字也有两种模式，套接字一般用于两个进程之间的网络通信，网络套接字需要来自诸如`TCP（传输控制协议）`或较低级别`UDP（用户数据报协议）`等基础协议的支持。

套接字有以下几种分类

- `顺序包套接字(Sequential Packet Socket)`：此类套接字为最大长度固定的数据报提供可靠的连接。此连接是双向的并且是顺序的。
- `数据报套接字(Datagram Socket)`：数据包套接字支持双向数据流。数据包套接字接受消息的顺序与发送者可能不同。
- `流式套接字(Stream Socket)`：流套接字的工作方式类似于电话对话，提供双向可靠的数据流。
- `原始套接字(Raw Socket)`：可以使用原始套接字访问基础通信协议。



##   进程间同步方式







##   线程通信方式







##   线程间同步方式





#  附录



## 附录1

### vim 主题颜色

> stellarized.vim， solarized8.vim， solarized8_low.vim， solarized8_higt.vim， solarized8_flat.vim, ayu.vim, cake16.vim, github1.vim, cosmic_latte.vim, onehalflight.vim, stellarized.vim, carbonized_dark.vim carbonized_light.vim,  pencil.vim , snow.vim vimspectr300-light.vim petrel.vim greygull.vim  seagull.vim stormpetrel.vim





> " cake16.vim , one.vim github.vim papaercolor_dark.vim carbonized_dark.vim carbonized_light.vim pencil.vim ayu.vim ayu_light.vim ayu_mirage.vim solarized8.vim solarized8_flat.vim solarized8_low.vim solarized8_higt.vim, snow.vim vimspectr300-light.vim petrel.vim greygull.vim  seagull.vim stormpetrel.vim,  c16gui, molokai, lilydjwg_dark_modified, lilydjwg_dark, nightshade_print_modified, colorful256,colorful, SolarizedDark_modified,SolarizedLight, nightshade_print, rainbow_autumn,vividchalk, flattened_light,flattened_dark, thegoodluck, 
>
> 
>
> " desert,blacklight,adrian,darkblack,darkzen,gor,habLight,neverness,putty,redstring,relaxedgreen,satori,tcsoft,cleanphp,autumn,bayQua,bmichaelsen, camo,candycode,carrot ,earth,fine_blue,fruity,gobo,inkpot,navajo,nicotine,phpx,professional,sf,umber_green,white,winter,zellner,dante_modified,rcg_gui_modified,gruvbox,











## 附录2



