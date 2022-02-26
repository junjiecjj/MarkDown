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

方法一：

```bash
sudo apt install fonts-firacode
```

方法二：

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



> 6 Powerline字体

sudo apt-get install fonts-powerline



###  修改终端提示符

+ 主要是将.bashrc中的PS1改为[github.bashrc](https://github.com/junjiecjj/configure_file/blob/master/bashrc)中此PS1的形式；

###  安装谷歌浏览器

+ 去[谷歌官网](https://www.google.cn/chrome/)下载.deb安装包

+ sudo dpkg -i google-chrome-xxx.deb

+ 打开谷歌浏览器，安装谷歌上网助手；

+ 登陆谷歌账号，自动同步；

+ 完成;

  

###  安装alacritty终端

```bash
sudo add-apt-repository ppa:aslatter/ppa

sudo add-apt-repository ppa:mmstick76/alacritty

sudo apt install alacritty

```



### 安装Kitty终端

```bash
sudo apt install kitty -y
```



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

### Markdown编辑器 notable

在 Ubuntu 下有 Notable 的 deb 和 appimage 包文件可以使用。尽管 Ubuntu 软件中提供了 Notable Snap 软件包，但建议从以下链接下载`.deb`：https://github.com/notable/notable/releases

+ sudo dpkg -i notable_1.8.4_amd64.deb
+ sudo apt-get install -f

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
git clone --recursive git://github.com/Valloric/YouCompleteMe
cd /.vim/bundle/YouCompleteMe/
git submodule update –init –recursive # 获取 YCM 的依赖包
```

此时如果检测完整性, 即输入:

```bash
git submodule update --init --recursive
```

不会出现任何返回, 因为一开始的 git 加了 recursive 参数。接下来:

```bash
sudo ./install.py --clang-completer --system-libclang
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



最后修改.ycm_extra_conf.py 文件
`Vim .vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py`,加入:

```python
'-isystem'
'/usr/include'
'isystem'
'/usr/include/c++/5.4.0'
'-isystem'
'/usr/include'
'/usr/include/x86_64_linux-gnu/c++'
```







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

#### 简介

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

#### 安装使用

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

```bash
wget https://github.com/muesli/duf/releases/download/v0.5.0/checksums.txt
wget https://github.com/muesli/duf/releases/download/v0.5.0/duf_0.5.0_linux_amd64.deb
sha256sum --ignore-missing -c checksums.txt
sudo dpkg -i duf_0.5.0_linux_amd64.deb
```



###  plots

+ `sudo add-apt-repository ppa:apandada1/plots`
+ `sudo apt update`
+ `sudo apt install plots`







### WSL







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

## 安装slock

```bash
git clone https://github.com/junjiecjj/slock.git

cd slock

sudo make clean install 
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



再来说说快捷键，和 i3wm 相同，dwm 的各种快捷键的起手操作都是 “MOD”，这个 MOD 键可以自己定义，默认是 MOD4, 即 Windows 键， MOD1= 即 Alt 键。



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
call vundle#begin('~/.vim/bundle')  
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
call vundle#begin('~/.nvim/bundle')
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

2. 安装nodejs

   ```bash
   mkdir -p /usr/local/nodejs
   去 http://nodejs.cn/download/ 下载64位的包node-v16.4.0-linux-x64.tar.xz至~/下载
   
   tar -xvf node-v16.4.0-linux-x64.tar.xz
   
   sudo mv ~/下载/node-v16.4.0-linux-x64  /usr/local/node
   
   sudo ln -s /usr/local/nodejs/bin/node /usr/bin/node
   
   sudo ln -s /usr/local/nodejs/bin/npm /usr/bin/npm
   ```

   

   

3. 安装yarn:

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
$: sudo apt install llvm -y
$: sudo apt install clang -y
$: git clone --depth=1 --recursive https://github.com/MaskRay/ccls
$: cd ccls
$: cmake -H. -BRelease -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_PREFIX_PATH=/usr/bin
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



#  窗口管理器i3









#  窗口管理器awesome









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





在 Linux 的世界中，有着一个文本三剑客的称呼，它们分别代表 grep (文本过滤）,sed（流编辑器）,awk (gawk)（报告生成器）。

## grep (文本过滤）

1.  **命令格式**

> grep [-acinv...] [--color=auto] [-A n] [-B n] '字符串' 文件名

2. **命令参数**

+ (1) -a  #以文本文件方式搜索。  

+ (2) -A<显示行数>  #除了显示符合范本样式的那一行之外，并显示该行之后的内容。  

+ (3) -b   #在显示符合样式的那一行之前，标示出该行第一个字符的编号。  

+ (4) -B<显示行数>  #除了显示符合样式的那一行之外，并显示该行之前的内容。  

+ (5) -c   #计算找到的符合行的次数。 

+ (6) -C<显示行数>  #除了显示符合样式的那一行之外，并显示该行之前后的内容。  

+ (7) -d <动作>  #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。  

+ (8) -e<范本样式>  #指定字符串做为查找文件内容的样式。  

+ (9) -E   #将样式为延伸的普通表示法来使用。  

+ (10) -f<规则文件> #指定规则文件，内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。  

+ (11) -F   #将样式视为固定字符串的列表。  

+ (12) -G  #将样式视为普通的表示法来使用。  

+ (13) -h  #在显示符合样式的那一行之前，不标示该行所属的文件名称。  

+ (14) -H  #在显示符合样式的那一行之前，表示该行所属的文件名称。  

+ (15) -i  #忽略字符大小写。  

+ (16) -l  #列出文件内容符合指定样式的文件名称。  

+ (17) -L  #列出文件内容不符合指定样式的文件名称。  

+ (18)  --line-number  #在显示符合样式的那一行之前，标示出该行的行数编号。  

+ (19) -n  # 顺便输出行号

```bash
grep -n the ex.txt
grep -n 'the' ex.txt
grep -n "the" ex.txt
#以上等价
```

+ (20) --quiet 或 --silent  #不显示任何信息。  

+ (21) -r  #此参数的效果和指定“-d recurse”参数相同。  

+ (22) -s  #不显示错误信息。  

+ (23) -v  #反向选择，即找不含搜索字符串的行  

```bash
grep -nv 'the' ex.txtgrep -v '^$' ex.txt | grep -v '^#' 
去除空白行和行首为#的行
```

+ (24) -V  #显示版本信息。  

+ (25) -w  #只显示全字符合的行。  

+ (26) -x  #只显示全行符合的行。  

+ (27) -y   #此参数的效果和指定“-i”参数相同。



假设test.txt文件内容如下：

```text
chen yin jie
chen he xian
chen yan xian

chen zhi hao
chen zhi wen

wang yin
wangyin

wang ting
wangting

wangyuzhong
jiangxian xiu 
lin jin shun
lin yin shun
lin hui fang

abc def hij
abcdef hij
abcdefhij

```





### grep命令忽略任何区分大小写

“ -i”告诉grep命令忽略任何区分大小写的命令。

> grep -i "linuxmi" linuxmi.txt





### **在搜索字符串前面或者后面显示行号**

显示匹配前后的行数

- **-A -** 指定匹配后要显示的行数
- **-B -** 指定要显示的行数
- **-C -** 指定匹配之前和之后要显示的行数

两个选项是-A和-B之间的切换，是用以显示匹配的行以及行号，分别控制在字符串前或字符串后显示的行数。Man页给出了更加详细的解释，我发现一个记忆的小窍门：-A=after、-B=before。

```bash
$ sudo ifconfig | grep -A 4 etho 
$ sudo ifconfig | grep -B 2 UP
```

```bash
$: grep  -n -A 2 -B 2 "wang" test.txt 
#输出
7-chen zhi wen
8-
9:wang yin
10:wangyin
11-
12:wang ting
13:wangting
14-
15:wangyuzhong
16-jiangxian xiu 
17-lin jin shun

```



###  **在匹配字符串周围打印出行号**

grep命令的-C选项和例4中的很相似，不过打印的并不是在匹配字符串的前面或后面的行，而是打印出两个方向都匹配的行（译注：同上面的记忆窍门一样：-C=center，以此为中心）：

```bash
$ grep  -n -C 2 "wang" test.txt 
#输出
7-chen zhi wen
8-
9:wang yin
10:wangyin
11-
12:wang ting
13:wangting
14-
15:wangyuzhong
16-jiangxian xiu 
17-lin jin shun

```



### **按给定字符串搜索文件中匹配的行号**

当你在编译出错时需要调试时，grep命令的-n选项是个非常有用的功能。它能告诉你所搜索的内容在文件的哪一行：

```bash
$ grep  -n "chen" test.txt 
#输出
1:chen jun jie
2:chen yin jie
3:chen he xian
4:chen yan xian
6:chen zhi hao
7:chen zhi wen

```





### **进行精确匹配搜索**

传递-w选项给grep命令可以在字符串中进行精确匹配搜索（译注：包含要搜索的单词，而不是通配）。例如，像下面这样输入：

```bash
# junjie @ Ubuntu in /mnt/c/公共的/ShellScript [日期: 周三 6月 30日, 时间:09:45:45]
$ grep -n -w  "abc" test.txt
21:abc def hij
# junjie @ Ubuntu in /mnt/c/公共的/ShellScript [日期: 周三 6月 30日, 时间:09:45:52]
$ grep -n   "abc" test.txt
21:abc def hij
22:abcdef hij
23:abcdefhij
```



### 多模式 Grep 命令

要搜索多个匹配模式，可以使用 **OR** ( **alternation** ) 运算符。我们可以用 **OR** 运算符 **|**（ **pipe** ）指定不同的匹配项，这些匹配项可以是文本字符串，也可以是表达式集。值得注意的是，在所有正则表达式运算符中，这个运算符的优先级是最低的。

使用 `grep` 命令基本正则表达式搜索多个匹配模式的语法如下：

```bash
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:54:32]
$ grep  -n "wang\|yin" test.txt 
2:chen yin jie
9:wang yin
10:wangyin
12:wang ting
13:wangting
15:wangyuzhong
18:lin yin shun
```



还需要注意的是，如果要搜索的字符串包含空格，需要用双引号将其括起来。

下面是使用扩展正则表达式的同一个示例，如果使用扩展模式，可以添加`-E`参数。使用扩展模式，就不需要为`|`管道符添加转义符了。也可以使用`egrep`命令，这个命令和`grep -E`用法一样。

```bash
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:20:00]
$ grep  -n "chen\|wang" test.txt 
1:chen jun jie
2:chen yin jie
3:chen he xian
4:chen yan xian
6:chen zhi hao
7:chen zhi wen
9:wang yin
10:wangyin
12:wang ting
13:wangting
15:wangyuzhong
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:25:49]
$ grep  -n -E "chen|wang" test.txt 
1:chen jun jie
2:chen yin jie
3:chen he xian
4:chen yan xian
6:chen zhi hao
7:chen zhi wen
9:wang yin
10:wangyin
12:wang ting
13:wangting
15:wangyuzhong

```

```bash
grep -E 'fatal|error|critical' /var/log/nginx/error.log
```

默认情况下，`grep` 命令是区分大小写的。要在搜索时忽略大小写，请调用 `grep` 加 `-i` （或 `--ignore-case` ）选项，示例如下：

```bash
grep -i 'fatal|error|critical' /var/log/nginx/error.log
```



当你只想搜索某个单词时，比如你想搜索的是单词 `error` ，`grep` 命令会输出所有包含 `error` 字符串的行，即它除了会输出包含 `error` 单词的行，还会输出包含 `errorless` 或 `antiterrorists` 等非 `error` 单词的行，这样是极不方便的。

因此要仅返回指定字符串是整词的行，或者是由非单词字符括起来的行，可以使用 `grep` 加 `-w` （或 `--word-regexp` ）选项：

```bash
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:54:32]
$ grep  -n "wang\|yin" test.txt 
2:chen yin jie
9:wang yin
10:wangyin
12:wang ting
13:wangting
15:wangyuzhong
18:lin yin shun
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:55:00]
$ grep  -n -w  "wang\|yin" test.txt 
2:chen yin jie
9:wang yin
12:wang ting
18:lin yin shun

```

值得注意的是，单词字符包括有字母、数字字符（比如 a-z、a-Z 和 0-9 ）以及下划线（ _ ），所有其他字符都被视为非单词字符。



### 正则表达式

grep命令最基本的用法是搜索文件中的文字字符或字符序列。例如，要显示/etc/passwd文件中包含字符串“bash”的所有行，需要运行以下命令:

```bash
grep bash /etc/passwd
```



默认情况下，grep命令是区分大小写的。这意味着大写和小写字符被视为不同的。要在搜索时忽略大小写，请使用-i选项。

如果搜索字符串包含空格，则需要用单引号或双引号将其括起:

```bash
grep "System message bus" /etc/passwd
```



`^`符号匹配行首的空字符串。在下面的示例中，字符串“root”只有在行首出现时才匹配。

```bash
grep '^root' /etc/passwd
```

$要查找以字符串“bash”结尾的行，可以使用以下命令:

```bash
grep 'bash$' /etc/passwd
```

您还可以使用两个锚来构造正则表达式。例如，查看配置文件，不显示空行，请运行以下命令:

```bash
grep -v '^$' /etc/samba/smb.conf
```

-v 反转匹配的意义，来选择不匹配的行。



`| `是或者的意思。例如：想查看cpu是否支持虚拟化：

```bash
grep 'vmx\|svm' /proc/cpuinfo
```

如果使用扩展正则表达式，则不需要转义|，如下所示:

```bash
grep -E 'svm|vmx' /proc/cpuinfo
```



通过grep命令，可以指定带有start和end关键字的正则表达式。输出将是包含指定的起始和结束关键字之间的整个表达式的句子。此功能非常强大，因为您无需在搜索命令中编写整个表达式。

句法：

$ grep “startingKeyword.*endingKeyword” filename

例子:

```bash
# jack @ unix in ~/公共的/shell_script [日期: 周二 6月 29日, 时间: 22:56:16]
$ grep  -n   "chen.*jie" test.txt 
1:chen jun jie
2:chen yin jie

```





## sed（流编辑器）

**一、初识 sed**

sed:Stream Editor

从名字上也可以直观的了解到它是一个流编辑工具。何为流编辑器？就是把文本中的文字按照特定的分隔方式，进行数据流处理。sed 就是基于这种方式，它是以换行符以分隔单位，对文本进行逐行的处理。

------

**二、初识 sed 的工作原理**

![图片](http://mmbiz.qpic.cn/mmbiz/IP70Vic417DMoxaN2W8JqB5DFmduELokz9e923j3zLZFgzMR68qkoz8Tx9p5wXOVCNPJs4TsIoIu0TqicjXBCVicA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

前提：首先对于一个文本文件来说，它是由至上而下的一行或 N 行组成。

1、当用 sed 命令对文本进行处理的时候，sed 先读取对象的文本文件的第一行到模式空间中。

2、当有内容进入 “模式空间” 时，sed 的编辑命令对模式空间中的内容进行编辑操作（修改，替换，删除，追加，显示等等）

3、模式空间中的内容编辑处理完成之后，sed 把此内容通过标准输出（默认为显示器）打印出来，并删除模式空间中的内容。

4、第一行处理结束。从新读取第二行的内容进行处理，直到最后一行。

**三、sed 命令的基本语法**

sed OPTIONS… [SCRIPT] [INPUTFILE…]

常用的选项：

-n,–quiet: 不输出模式空间中的内容

-i: 直接编辑原文件，默认不对原文件进行操作

-e: 可以使用多个命令（脚本）进行操作

-f /path/from/sed_script: 从指定的文本中读取处理脚本

-r: 使用扩展正则表达式

------

**四、模式空间中的编辑操作**

1、地址定界：

1）#：# 为数字，指定要进行处理操作的行

2）$：表示最后一行，多个文件进行操作的时候，为最后一个文件的最后一行

3）/regexp/：表示能够被 regexp 匹配到的行

regexp 及基于正则表达式的匹配：关于正则表达式的请参考 grep 的基本用法详解中的【三、了解正则表达式】

4）/regexp/I：匹配是忽略大小写

5）\% regexp%: 任何能够被 regexp 匹配到的行，换用 %（用其他字符也可以，如：#）为边界符号

6）addr1,addr2：指定范围内的所有的行（范围选定）

常用的以下几种表示方法：

a）0，/regexp/：从起始行开始到第一次能够被 regexp 匹配到的行

b）/regexp/,/regexp/：被模式匹配到的行内的所有的行

c）#,#：# 为数字，给定具体的行范围

d）#,+N：# 为数字，从 #开始的行开始，向下 N 行的所有的行

7）first~step：指定起始的位置及步长，例如：1~2 表示 1,3,5…

2、常用的编辑命令：

1）d：删除匹配到的行

2）p：打印模式空间中的内容

注意：sed 默认情况下是把 “模式空间” 中的内容全部进行显示，p 的意义在于把匹配到的行进行显示。

所以其显示的结果是 “默认的显示内容 + p 要显示的内容”。

因此通常与 - n 选项一起使用，表示只显示匹配到的行。

3）a \text：append, 表示在匹配到的行之后追加内容

4）i \text：insert, 表示在匹配到的行之前追加内容

5）c \text：change, 表示吧匹配到的行和给定的文本进行交换

6）s/regexp/replacement/flages：查找替换，把 text 替换为 regexp 匹配到的内容（其中 / 可以用其他字符代替，例如 @）

可能会用到的特殊 replacemen（通常 replacement 为固定的字符窜）：

\L：转换后面的内容第一个字母为小写字母

\l：后面的内容全部转换成小写，直到遇到 \E 为止

\U：转换后面的内容第一个字母为大写字母

\u：后面的内容全部转换成大写，直到遇到 \E 为止

\E：当以 \L 或 \U 开始的时候，\E 意味着停止字符的转换

详情请参考：sed 的官方文档

如果是 replacement 为变量时，用 '$VAR' 引用即可

常用的 flages：

g：全局替换，默认只替换第一个

i： 不区分大小写

p：如果成功替换则打印

7）w /path/to/somefile：将匹配到的文件另存到指定的文件中

8）r /path/from/somefile：将读取指定的文件内容到匹配的行处（如果指定文件为多行时，追加到匹配行之后）

**五、知识点练习**

1、显示文件中的偶数行：

1）用 first~step 的方式来实现，把奇数行删除，自然显示的事偶数行

`sed '1~2d' test.txt`

2）不输出默认的显示内容，用 p 指定显示偶数行

`sed '2~2p' test.txt`

2、在含有 “ftp” 这个行的前面加上 “#This is a command”

`sed '/junjie/i  # This is a command' etcpasswd.txt`

3、把以 /sbin/nologin 结尾的行的小写字母全部替换成大写

1）先用 /regexp/ 地址定界的来选定以 /sbin/nologin 结尾的行

`#显示所有以/sbin/nologin$结尾的行`

` sed -n '\#/sbin/nologin$#p' test.txt`

2）查找替换

查找所有的小写字符 [a-z]

其中 /\u&/ 中的 & 表示前面所匹配到的所有内容，所以 /\u&/g 为前面所匹配到的小写字母全部替换为大写字母

4、把etcpasswd.txt 文件所有不以 #开头的行保存到etcpass.txt的文件中。

其中多个脚本用 - e 来分别执行，其实用；也可以实现多个脚本的连接。例如：

`sed -n -e '/^m/d;w  ./etcpass.txt'  etcpasswd.txt`



**六、sed 的知识扩展**

在 sed 的工作原理图中我们了解到，sed 不仅存在模式空间，也存在一个保持空间（hold space)。顾名思义，保存空间是一段 sed 独有的内存空间片段，可以暂时存放一些数据。

其中与 “保持空间” 相关的编辑命令有：

h：把模式空间中的内容覆盖到保存空间中的内容

H：把模式空间中的内容追加到保存空间中（加在原有内容之后）

g：把保持空间中的内容覆盖到模式空间中的内容

G：把保持空间中的内容追加到模式空间中（加在原有内容之后）

x：把模式空间中的内容和保持空间中的内容进行交换

d：删除模式空间中的内容

D：如果模式空间中的内容为多行时，删除模式空间中的第一行

n：读取匹配到的行的下一行到模式空间中（覆盖原内容）

N：读取匹配到的行的下一行到模式空间中（追加在原内容之后）

例如：显示偶数行的时候就可以这样实现：sed -n 'n;p' FILE

----

----





## awk (gawk)（报告生成器）






















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
