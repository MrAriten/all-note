# 第一章 linux是什么以及如何学习

①linux具有“可移植性”

②linux的内核kernel以及distribution

# 第二章 主机规划和硬盘分区

## ①.iso后缀名

指的是image文件（映像档）。这种 image 文件是 由光盘直接刻录成文件的。

## ②挂载

指的就是将设备文件中的顶级目录连接到 Linux 根目录下的某一目录（最好是空目录），访问此目录就等同于访问设备文件。

[什么是挂载，Linux挂载详解 (biancheng.net)](http://c.biancheng.net/view/2859.html)

## ③swap分区：

swap分区，即交换区，swap空间的作用可简单描述为：当系统的物理[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap中恢复保存的数据到内存中。

# 第三章 安装LINUX

## ①MBR分区和GBT分区

**MBR格式分区方案**

　　**1.**主分区数量不能超过4个;

　　**2.**分区大小无法超过2TB容量;

　　**3.**支持安装所有的Windows操作系统。

　　**GPT格式分区方案**

　　**1.**磁盘分区数量几乎无限制，但是Windows系统只允许最多128个分区;

　　**2.**支持2TB以上容量的硬盘;

　　**3.**仅支持安装64位操作系统，因为UEFI引导启动只支持64位操作系统，如果想要用EFI引导32位系统，貌似必须主板开启CSM兼容模块支持;

　　**4.**GPT格式分区表自带备份。

### 　　mbr和gpt选哪个?

　　**1.**如果它小于2TB：您仅能将其初始化为MBR分区形式，因为MBR支持不超过2TB的分区大小。

　　**2.**如果它大于2TB：可以将其初始化为GPT分区形式，但是您需要确保主板支持UEFI引导模式并且您的操作系统也支持GPT分区形式，此外即将发布的Windows 11操作系统仅支持UEFI引导模式，换句话说未来的磁盘分区形式应该会以GPT为主。

# 第四章 首次登入与在线求助

## ① linux的关机

由于 Linux 系统使用了异步的磁盘/内存数据传输模式，同时又是个多人多任务的环境， 所以不能随便的不正常关机，关机有一定的程序，错误的关机方法可能会造成磁盘数据的损毁。

## ②隐藏文件查看

linux里是有很多系统相关文件的，就是平时在服务器看到的那些，不过这些在虚拟机中是要手动打开的。

## ③X Window

X Window即X Window图形用户接口，是一种计算机软件系统和网络协议，提供了一个基础的图形用户界面（GUI）和丰富的输入设备能力联网计算机。其中软件编写使用广义的命令集，它创建了一个硬件抽象层，允许设备独立性和重用方案的任何计算机上实现。

不要把Window和Windows搞混了

Ubuntu中使用的是Xwindow的一个分支Xorg

## ④切换至纯命令行

Linux 预设的情况下会提供六个Terminal 来让使用者登入， 切换的方式为使用：[Ctrl] + [Alt] + [F1]~[F6]的组合按钮。

那这六个终端接口如何命名呢，系统会将[F1] ~ [F6]命名为 tty1 ~ tty6 的操作接口环境。 也就是说，当你按下[crtl] + [Alt] + [F1]这三个组合按钮时 (按着[ctrl]与[Alt]不放，再按下[F1]功能键)， 就会进入到 tty1 的 terminal 界面中了。同样的[F2]就是 tty2。

那么如何回到刚刚的 X 窗口接口呢？很简单，按下[Ctrl] + [Alt] + [F1]就可以。

其实，所谓的窗口环境，就是：『文字界面加上 X 窗口软件』的组合！因此，文字界面是一定会存在的，只是窗口界面软件就看你要不要启动而已。 

## ⑤提示字符

在 Linux 当中，默认 root 的提示字符为 # ，而一般身份用户的提示字符为 $ 

文件路径中 ~ 符号代表的是『用户的家目录』的意思，他是个『变量』 举例来说，root 的家目录在/root， 所以 ~ 就代表/root 的意思。而 dmtsai 的家目录在/home/dmtsai， 所以如果你以 dmtsai 登入时，他看到的 ~ 就会等于/home/dmtsai，是作为linux的一个用户登录。

## ⑥ bash太长如何换行而不是enter后输出

可以输入\ + enter来换行，这样就不会执行命令。

## ⑦ linux会区分大小写

## ⑧基础指令操作

  显示日期与时间的指令： date
		 显示日历的指令： cal
		 简单好用的计算器： bc

## ⑨重要快捷键

### TAB：

这个[Tab]按键算是 Linux 的 Bash shell 最棒的功能之一了！他具有『命令补全』与『文件补齐』的功能喔！ 重点是，可以避免我们打错指令或文件名呢！很棒吧！但是[Tab]按键在不同的地方输入，会有不一样的结果喔！

  [Tab] 接在一串指令的第一个字的后面，则为『命令补全』；
		 [Tab] 接在一串指令的第二个字以后时，则为『文件补齐』！
		 若安装 bash-completion 软件，则在某些指令后面使用 [tab] 按键时，可以进行『选项/参数的补齐』功能！

### Ctrl+C：

中断当前运行的命令

### Ctrl+D：

类似EOF的功能，能帮助输入终止符。

### shift+PageUp/PageDown：

前后翻页

## ⑩帮助命令

一般的命令加个 --help 都能看到一些参数的解释

如果要==系统求助== 可以用 man date （date是你想看到帮助的指令，man是manual的简称）

man之后会列出一个表格，你会发现在Date后面有个(1)，这个数字代表的意义可以查阅：
		1 用户在 shell 环境中可以操作的指令或可执行文件
		2 系统核心可呼叫的函数与工具等
		3 一些常用的函数(function)与函式库(library)，大部分为 C 的函式库(libc)
		4 装置文件的说明，通常在/dev 下的文件
		5 配置文件或者是某些文件的格式
		6 游戏(games)
		7 惯例与协议等，例如 Linux 文件系统、网络协议、ASCII code 等等的说明
		8 系统管理员可用的管理指令9 跟 kernel 有关的文件

1、5、8是比较重要的几个。

man page中也有很多可以操作的命令来方便你查看你需要的信息，这些可以从鸟叔的189页去看。

==在线求助==是使用info命令

## 11、doc文件

对于较为复杂的指令或指令集，都会有一个doc来将所有指令的细节写入，这些又被称作说明文档

一般存在/usr/share/doc这个目录

## 12、文书编辑器nano

类似window的txt编辑器

## 13、如何正确关机

在 Linux 底下，由于每个程序 (或者说是服务) 都是在在背景下执行的，因此，在你看不到的屏幕背后其实可能有相当多人同时在你的主机上面工作， 例如浏览网页啦、传送信件啦以 FTP 传送文件啦等等的，如果你直接按下电源开关来关机时， 则其他人的数据可能就此中断！那可就伤脑筋了！

一个人用一般不存在这个问题

## 14、sync指令——不正常关机前保存数据

那就是万一你的系统因为某些特殊情况造成不正常关机 (例如停电或者是不小心踢到 power)时，由于数据尚未被写入硬盘当中，哇！所以就会造成数据的更新不正常啦！ 那要怎么办呢？这个时候就需要 sync 这个指令来进行数据的写入动作啦！ 直接在文字接口下输入 sync，那么在内存中尚未被更新的数据，就会被写入硬盘中！

# 第五章 linux的文件权限与目录配置

## ①身份

root,users,group,others

## ②文件权限

在root使用 ls -al 之后，会出现一个list，第一项是关于文件权限的，P208有详细说明。

要修改文件权限，可以看p212

三种权限：rwx的作用，p217

## ③文件类型

ls -al 后 在权限前的一个字符揭示了文件类型

p221

链接l：类似windows中的快捷方式

## ④FHS文件目录配置标准

p224开始看，有linux中常见目录的作用

usr是unix software resource的缩写，并不是usr，而是存放软件数据的地方

# 第六章 linux文件与目录管理

## ①常见目录

. 代表此层目录

.. 代表上一层目录

-代表前一个工作目录

~ 代表『目前用户身份』所在的家目录

~account 代表 account 这个用户的家目录(account 是个账号名称)

## ②常见指令

  cd：变换目录
		 pwd：显示当前目录
		 mkdir：建立一个新的目录
		 rmdir：删除一个空的目录

返回上一层目录可以 cd ..

仅输入cd的效果等于cd~

mkdir如果直接生成文件夹必须一层一层生成，除非加-p，mkdir -p /home/bird/testing/test1

## ③环境变量 PATH

PATH中会保存一些命令，也就是可执行文件的路径，这样我们才能在使用这些命令时不需要将其文件路径也输入进去

## ④echo命令

echo可以显示后面的变量，例如 echo $PATH，$表示后面加的是变量

## ⑤文件操作

P243，关于文件复制移动等操作，还有创建连接档的操作。

## ⑥查阅文件内容

p250，有很多有趣的操作，特别有一个可以侦测文件变化的命令tail

## ⑦文件预设权限

p262

## ⑧更多属性

p265

## ⑨特殊权限SUID/SGID

SUID:使用户暂时获得文件的root权限（仅能用于二进制程序不能用于sell script）

SGID:就是SUID变成了Group，群组在修改文件时能暂时获得root的权限

SBIT:只对目录有效，当用户在该目录（SBIT目录）下建立文件或目录时，仅有自己与 root 才有权力删除该文件

## ⑩文件搜寻which whereis locate find

例如which ls就可以知道ls到底在哪

whereis可以在特定目录中寻找文件名

其他具体可以看p270

# 第七章 linux磁盘与文件管理系统

## ①EXT2格式

window用的是FAT、FAT16或者NTFS格式，windows是不认识EXT2格式的

EXT2是索引式文件系统，利用inode和block来管理和保存文件

当我们在 Linux 下的文件系统建立一个目录时，文件系统会分配一个 inode 与至少一块 block 给该目录。其中，inode 记录该目录的相关权限与属性，并可记录分配到的那块 block 号码； 而 block 则是记录在这个目录下的文件名与该文件名占用的 inode 号码数据。

同一个 filesystem 的某个 inode 只会对应到一个文件内容而已(因为一个文件占用一个 inode 之故)， 因此我们可以透过判断 inode 号码来确认不同文件名是否为相同的文件。

## ①hard link

是在某个目录下的 block 多写入一个关连数据而已， 既不会增加 inode 也不会耗用 block 数量，类似于快捷方式

但是不能跨文件系统也不能链接目录，因为本质是对inode的一种复用，如果链接到目录的话，目录下的所有inode都要被复用，所以目前还不支持

如果有两个路径对应一个inode，这就是hardlink，且如果其中一个路径被删除，另一个也是可以访问的。

## ②symbolic link

也类似快捷方式，但是其与hardlink不同的是，会产生一个新的inode号，本质上并不属于一个路径，而是一种方向，如果原来的路径被删除，那么这个link是不能打开的，这是和hard的最大区别

但是他能够连接目录，所以适用范围更广

## ③新建目录的link数

如果建立目录时，他默认的 link 数量会是多少？ 让我们来 想一想，一个『空目录』里面至少会存在些什么？呵呵！就是存在 . 与 .. 这两个目录啊！ 那么， 当我们建立一个新目录名称为 /tmp/testing 时，基本上会有三个东西，那就是： 

 /tmp/testing 
 /tmp/testing/. 
 /tmp/testing/.. 

而其中 /tmp/testing 与 /tmp/testing/. 其实是一样的！都代表该目录啊～而 /tmp/testing/.. 则代表 /tmp  这个目录，所以说，当我们建立一个新的目录时， ==新的目录的 link 数为 2 ，而上层目录的 link  数则会增加 1==

在这个例子中，testing的link数为2，tmp的link数为之前的+1

## ④文件系统的挂载

 单一文件系统不应该被重复挂载在不同的挂载点(目录)中； 
 单一目录不应该重复挂载多个文件系统； 
 要作为挂载点的目录，理论上应该都是空目录才是。

如果你要用来挂载的目录里面并不是空的，那么挂载了文件系统之后，原目录 下的东西就会暂时的消失。举个例子来说，假设你的/home 原本与根目录 (/) 在同一个文件系统中， 底下原本就有 /home/test 与 /home/vbird 两个目录。然后你想要加入新的磁盘，并且直接挂载/home  底下，那么当你挂载上新的分区槽时，则 /home 目录显示的是新分区槽内的资料，至于原先的 test 与 vbird 这两个目录就会暂时的被隐藏掉了

等到新分区槽被卸除之后，则 /home 原本的内容就会再次的跑出来

==这个功能多用于插入U盘或者插入CD时==

# 第八章 文件与文件系统的压缩、打包与备份

## ①常见压缩指令

*.Z compress 程序压缩的文件； 
*.zip zip 程序压缩的文件； 
*.gz gzip 程序压缩的文件； 
*.bz2 bzip2 程序压缩的文件； 
*.xz xz 程序压缩的文件； 
*.tar tar 程序打包的数据，并没有压缩过； 
*.tar.gz tar 程序打包的文件，其中并且经过 gzip 的压缩 
*.tar.bz2 tar 程序打包的文件，其中并且经过 bzip2 的压缩 
*.tar.xz tar 程序打包的文件，其中并且经过 xz 的压缩

单纯的 tar 功 能仅是『打包』而已，亦即是将很多文件集结成为一个文件， 事实上，他并没有提供压缩的功能， 后来，GNU 计划中，将整个 tar 与压缩的功能结合在一起，如此一来提供使用者更方便并且更强大 的压缩与打包功能！ 底下我们就来谈一谈这些在 Linux 底下基本的压缩指令吧！

当你使用 gzip 进行压缩时，在预设的状态下原本的文件会被压缩成为 .gz 的档名，==源文件就不再存在==了。

## ②压缩时根目录的作用

P361

# 第九章 vim程序编辑器

# 第十章 认识与学习BASH

## ①什么是shell

只要能够操作应用程序的接口都能够称为壳程序。狭义的壳程序指的是指 令列方面的软件，包括本章要介绍的 bash 等。 广义的则包括图形接口的软件，因为图形接口也能够操作各种应用程序。

## ②~/.bash_history

能够记录一系列你的历史命令

## ③命令别名

你可以在指令列输入 alias 就可以知道目前的命令 别名有哪些了！也可以直接下达命令来设定别名呦：

  alias lm='ls -al‘

这样lm就和 ls -al命令一致了

## ④echo的作用

可以看到变量的具体内容

例如 echo $PATH

## ⑤变量的规则

1. 变量与变量内容以一个等号『=』来连结，如下所示： 『myname=VBird』 

2. ==等号两边不能直接接空格符==，如下所示为错误： 『myname = VBird』或『myname=VBird Tsai』 
3. 变量名称只能是英文字母与数字，但是开头字符不能是数字，如下为错误： 『2myname=VBird』 
4. 变量内容若有空格符可使用双引号『"』或单引号『'』将变量内容结合起来，但 o 双引号内的特殊字符如 $ 等，可以保有原本的特性，如下所示： 『var="lang is $LANG"』则『echo $var』可得『lang is zh_TW.UTF-8』 o 单引号内的特殊字符则仅为一般字符 (纯文本)，如下所示： 『var='lang is $LANG'』则『echo $var』可得『lang is $LANG』 
5. 可用跳脱字符『 \ 』将特殊符号(如 [Enter], $, \, 空格符, '等)变成一般字符，如： 『myname=VBird\ Tsai』 
6. 在一串指令的执行中，还需要藉由其他额外的指令所提供的信息时，可以使用反单引号『`指令`』或 『$(指 令)』。特别注意，那个 ` 是键盘上方的数字键 1 左边那个按键，而不是单引号！ 例如想要取得核心版本 的设定： 『version=$(uname -r)』再『echo $version』可得『3.10.0-229.el7.x86_64』 
7. 若该变量为扩增变量内容时，则可用 "$变量名称" 或 ${变量} 累加内容，如下所示： 『PATH="$PATH":/home/bin』或『PATH=${PATH}:/home/bin』 
8. 若该变量需要在其他子程序执行，则需要以 export 来使变量变成环境变量： 『export PATH』 
9. 通常大写字符为系统默认变量，自行设定变量可以使用小写字符，方便判断 (纯粹依照使用者兴趣与嗜好) ； 
10. 取消变量的方法为使用 unset ：『unset 变量名称』例如取消 myname 的设定： 『unset myname』

## ⑥环境变量

可以通过env命令查看系统的环境变量，用set命令可以查看包括自己设定的环境变量

==子程序仅会继承父程序的环境变量， 子程序不会继承父程序的自定义变量==

自定义变量可以通过export变为系统的环境变量

## ⑦回传值

?：(关于上个执行指令的回传值）

输入 echo $? 时可以看到上一个命令的回传值

## ⑧declare

declare 或 typeset 是一样的功能，就是在『宣告变量的类型』。如：declare -i sum=100+300+5

选项与参数：

-a ：将后面名为 variable 的变量定义成为数组 (array) 类型 
-i ：将后面名为 variable 的变量定义成为整数数字 (integer) 类型 
-x ：用法与 export 一样，就是将后面的 variable 变成环境变量； 
-r ：将变量设定成为 readonly 类型，该变量不可被更改内容，也不能 unset

## ⑨bash的默认设置

 变量类型默认为『字符串』，所以若不指定变量类型，则 1+2 为一个『字符串』而不是『计算式』。 所以 上述第一个执行的结果才会出现那个情况的；

  bash 环境中的数值运算，预设最多仅能到达整数形态，所以 1/3 结果是 0

## ⑩数组的创建与使用

var[index]=content

var是数组名，可以更改，index是在这个数组的位置

## 十一、变量的判断以及替换

p449

可以通过echo判断变量是否存在

new_var=${old_var-content} 

new_var会被old_var取代，如果old_var不存在，则用content去取代

 username=${username:-root)

加上『 : 』后若变量内容为空或者是未设定，都能够以后面的内容替换

## 十二、配置文件

在未登录环境下和登录环境下读取的配置文件都是不同的

登陆环境下读取的配置文件：~/.bashrc

source ：读入环境配置文件的指令

使用source可以将刚修改的bashrc文件配置apply到当前环境中

## 十三、数据流重导向

数据流重导向可以将 standard output  (简称 stdout) 与 standard error output (简称 stderr) 分别传送到其他的文件或装置去，而分别传送所 用的特殊字符则如下所示： 

1. 标准输入 (stdin) ：代码为 0 ，使用 < 或 << ；
2. 标准输出 (stdout)：代码为 1 ，使用 > 或 >> ； 
3. 标准错误输出(stderr)：代码为 2 ，使用 2> 或 2>> ；

ll / > ~/rootfile <==屏幕并无任何信息

这个输出会导出到rootfile中

1. 该文件 (本例中是 ~/rootfile) 若不存在，系统会自动的将他建立起来，但是 

2. 当这个文件存在的时候，那么系统就会先将这个文件内容==清空==，然后再将数据写入
3. 也就是若以 > 输出到一个已存在的文件中，那个文件就会==被覆盖==

如果想要将数据累加而不想要将旧的数据删除，那该如何是好？利用两个大于的符号 (>>) 就好 啦！以上面的范例来说，你应该要改成『 ll / >> ~/rootfile 』即可。 如此一来，

(1) ~/rootfile 不存 在时系统会主动建立这个文件；

(2)若该文件已存在， 则数据会在该文件的最下方累加进去！

stdinput就是可以键盘输入以及文本文档输入

## 十四、指令执行逻辑

分号 ； 类似C++中的分号，可以同时让一行的多个指令一起执行

command1 && command2 || command3

与或也和C++中一样，当command1执行成功时command2才会执行

## 十五、管线命令

p470

### 管线命令『 | 』：

 其实这个管线命令『 | 』仅能处理经由前面一个指令传来的正确信息，也就是 standard output 的信息，对于 stdandard error 并没有直接处理的能力。

在每个管线后面接的第一个数据必定是『指令』喔！而且这个指令必须要能够接受 standard input 的 数据才行，这样的指令才可以是为『管线命令』，例如 less, more, head, tail 等都是可以接受 standard  input 的管线命令啦。至于例如 ls, cp, mv 等就不是管线命令了！因为 ls, cp, mv 并不会接受来自 stdin 的数据。

 管线命令仅会处理 standard output，对于 standard error output 会予以忽略 
 管线命令必须要能够接受来自前一个指令的数据成为 standard input 继续处理才行。

 ls -al /etc | less

如此一来，使用 ls 指令输出后的内容，就能够被 less 读取，并且利用 less 的功能，我们就能够前 后翻动相关的信息

### 截取命令：cut，grep

撷取讯息通常是针对『一行一行』 来分析的

cut 主要的用途在于将『同一行里面的数据进行分解！』最常使用在分析一些数据或文字数据的时候！ 这是因为有时候我们会以某些字符当作分区的参数，然后来将数据加以切割，以取得我们所需要的数据。

grep则是分析一行讯息， 若当中有我 们所需要的信息，就将该行拿出来

范例三：在 last 的输出讯息中，只要有 root 就取出，并且仅取第一栏

last | grep 'root' | cut -d ' ' -f1 

在取出 root 之后，利用上个指令 cut 的处理，就能够仅取得第一栏啰！

### 排序命令：sort，uniq，wc

### 双向重导向：tee

就是可以将输出流中间输出到别的地方去

### 参数代换：xargs

xargs 可以读入 stdin 的数据，并 且以空格符或断行字符作为分辨，将 stdin 的资料分隔成为 arguments 。 因为是以空格符作为分隔， 所以，如果有一些档名或者是其他意义的名词内含有空格符的时候， xargs 可能就会误判了

会使用 xargs 的原因是， 很多指令其实并不支持管 线命令，因此我们可以透过 xargs 来提供该指令引用 standard input 之用

`rm` ````find` `/path` `-``type` `f``

如果path目录下文件过多就会因为“参数列表过长”而报错无法执行。但改用xargs以后，问题即获解决。

```
find` `/path` `-``type` `f -print0 | ``xargs` `-0 ``rm
```

本例中xargs将[find](https://baike.baidu.com/item/find?fromModule=lemma_inlink)产生的长串文件列表[拆散](https://baike.baidu.com/item/拆散?fromModule=lemma_inlink)成多个子串，然后对每个子串调用rm。这样要比如下使用find命令效率高的多。

### 减号-

管线命令在 bash 的连续的处理程序中是相当重要的！另外，在 log file 的分析当中也是相当重要的 一环， 所以请特别留意！另外，在管线命令当中，常常会使用到前一个指令的 stdout 作为这次的 stdin ， 某些指令需要用到文件名 (例如 tar) 来进行处理时，该 stdin 与 stdout 可以利用减号 "-"  来替代， 举例来说：

mkdir /tmp/homeback 

tar -cvf ==-== /home | tar -xvf ==-== -C /tmp/homeback 

上面这个例子是说：『我将 /home 里面的文件给他打包，但打包的数据不是纪录到文件，而是传送 到 stdout； 经过管线后，将 tar -cvf - /home 传送给后面的 tar -xvf - 』。后面的这个 - 则是==取用前 一个指令的 stdout==， 因此，我们就不需要使用 filename 了！这是很常见的例子喔！注意注意！

有点像python里面的==占位符==

# 第十一章 正规表示法与文件处理格式

正规表示法基本上是一种『表示法』， 只要工具程序支持这种表 示法，那么该工具程序就可以用来作为正规表示法的字符串处理之用。

 例如 vi, grep, awk ,sed 等 等工具，因为她们有支持正规表示法， 所以，这些工具就可以使用正规表示法的特殊字符来进行 字符串的处理。但例如 cp, ls 等指令并未支持正规表示法， 所以就==只能使用 bash 自己本身的通 配符==而已。

『正规表示法与通配符是完全不一样的东西！』 这很 重要喔！因为『通配符 (wildcard) 代表的是 bash 操作接口的一个功能』，但正规表示法则是一种字符串处理的表 示方式！ 这两者要分的很清楚才行喔！所以，学习本章，请将前一章 bash 的通配符意义先忘掉吧！

有点像其他语言里面的==正则表达式==

## 数据处理工具awk sed p510

这两个工具能够控制输出的格式

## 文件比对工具 diff cmp

p515 能够比较文件之间的差异

# 第十二章 学习shell script

1. 指令的执行是从上而下、从左而右的分析与执行； 
2. 指令的下达就如同第四章内提到的： 指令、选项与参数间的多个空白都会被忽略掉； 
3. 空白行也将被忽略掉，并且 [tab] 按键所推开的空白同样视为空格键；
4.  如果读取到一个 Enter 符号 (CR) ，就尝试开始执行该行 (或该串) 命令； 
5. 至于如果一行的内容太多，则可以使用『 \[Enter] 』来延伸至下一行；
6.  『 # 』可做为批注！任何加在 # 后面的资料将全部被视为批注文字而被忽略！

## 如何执行？

直接指令下达： shell.sh 文件必须要具备可读与可执行 (rx) 的权限，然后： 

o 绝对路径：使用 /home/dmtsai/shell.sh 来下达指令； 
o 相对路径：假设工作目录在 /home/dmtsai/ ，则使用 ./shell.sh 来执行 
o 变量『PATH』功能：将 shell.sh 放在 PATH 指定的目录内，例如： ~/bin

以 bash 程序来执行：透过『 bash shell.sh 』或『 sh shell.sh 』来执行

==这两种方式都是让shell文件在子程序中运行==

==用source运行——让shell文件在父程序中运行==

## 计算pi

p530 用bc命令来计算无穷小数

## test检查是否存在

test不仅能判断文档或者文件是否存在，还能判断两个表达式的正确与否，类似其他语言中的 if

## 中括号[ ]判断符

因为中括号用在很多地方，包括通配符与正规表示法等等，所以如果要 在 bash 的语法当中使用中括号作为 shell 的判断式时，==必须要注意中括号的两端需要有空格符来分隔==

其实就类似C中if后面接的小括号()

## 设定脚本变量

```bash
echo "The script name is ==> ${0}"
echo "Total parameter number is ==> $#"
[ "$#" -lt 2 ] && echo "The number of parameter is less than 2. Stop here." && exit 0
echo "Your whole parameter is ==> '$@'"
echo "The 1st parameter ==> ${1}"
echo "The 2nd parameter ==> ${2}"
```

输出结果为

```bash
[dmtsai@study bin]$ sh how_paras.sh theone haha quot
The script name is ==> how_paras.sh <==檔名
Total parameter number is ==> 3 <==果然有三个参数
Your whole parameter is ==> 'theone haha quot' <==参数的内容全部
The 1st parameter ==> theone <==第一个参数
The 2nd parameter ==> haha <==第二个参
```

## 变量偏移

```bash
echo "Total parameter number is ==> $#"
echo "Your whole parameter is ==> '$@'"
shift # 进行第一次『一个变量的 shift 』
echo "Total parameter number is ==> $#"
echo "Your whole parameter is ==> '$@'"
shift 3 # 进行第二次『三个变量的 shift 』
echo "Total parameter number is ==> $#"
echo "Your whole parameter is ==> '$@'
```

输出如下：

```bash
[dmtsai@study bin]$ sh shift_paras.sh one two three four five six <==给予六个参数
Total parameter number is ==> 6 <==最原始的参数变量情况
Your whole parameter is ==> 'one two three four five six'
Total parameter number is ==> 5 <==第一次偏移，看底下发现第一个 one 不见了
Your whole parameter is ==> 'two three four five six'
Total parameter number is ==> 2 <==第二次偏移掉三个，two three four 不见了
Your whole parameter is ==> 'five six'
```

## 条件判断式

### if...then elif

```bash
if [ "${yn}" == "Y" ] || [ "${yn}" == "y" ]; then
echo "OK, continue"
```

注意中括号要有空格！

### case...esac

```bash
case $变量名称 in <==关键词为 case ，还有变数前有钱字号
 "第一个变量内容") <==每个变量内容建议用双引号括起来，关键词则为小括号 )
程序段
;; <==每个类别结尾使用两个连续的分号来处理！
 "第二个变量内容")
程序段
;;
 *) <==最后一个变量内容都会用 * 来代表所有其他值
不包含第一个变量内容与第二个变量内容的其他程序执行段
exit 1
;;
esac <==最终的 case 结尾！『反过来写』思考一下！


case ${1} in
 "hello")
echo "Hello, how are you ?"
;;
 "")
echo "You MUST input parameters, ex> {${0} someword}"
;;
 *) # 其实就相当于通配符，0~无穷多个任意字符之意！
echo "Usage ${0} {hello}"
;;
esac
```

## 参数输入

```bash
echo "This program will print your selection !"
read -p "Input your choice: " choice
case ${choice} in
 "one")
echo "Your choice is ONE"
;;
 "two")
echo "Your choice is TWO"
;;
 "three")
echo "Your choice is THREE"
;;
 *)
echo "Usage ${0} {one|two|three}"
;;
esac
```

choice就是用户可以输入的参数了

## 函数function

p549

```bash
function fname() {
程序段
}
```

```bash
function printit(){
echo -n "Your choice is " # 加上 -n 可以不断行继续在同一行显示
}
echo "This program will print your selection !"
case ${1} in
 "one")
printit; echo ${1} | tr 'a-z' 'A-Z' # 将参数做大小写转换！
;;
```

## 循环loop

### while循环

```bash
while [ condition ] <==中括号内的状态就是判断式 
do <==do 是循环的开始！
程序段落
done <==done 是循环的结束
```

```bash
until [ condition ] 
do
程序段落
done
```

### for循环

```bash
for var in con1 con2 con3 ...
do
程序段
done

users=$(cut -d ':' -f1 /etc/passwd) # 撷取账号名称
for username in ${users} # 开始循环进行！
do
 id ${username}
done
```

```bash
for (( 初始值; 限制值; 执行步阶 )) 
do
程序段
done
```

```bash
for (( i=1; i<=${nu}; i=i+1 ))
do
s=$((${s}+${i}))
don
```

## shell的debug

p558

这里的debug只能一行一行看输出结果，很捞

# 第十三章 linux的账号管理和ACL权限设定

## linux的账号和群组

### UID和GID

一个是使用者 ID (User ID ，简称 UID)、一个是群组 ID (Group ID ，简称 GID)。

每一个文件都会有所谓的 拥有者 ID 与拥有群组 ID ，当我们有要显示文件属性的需求时，系统会依据 /etc/passwd 与 /etc/group 的内容， 找到 UID / GID 对应的账号与组名再显示出来

## sudo

sudo能够让root以用户的权限去执行命令

## 用户查询

p617

w，who，last，lastlog可以查询所有用户以及最近登陆的用户

# 第十四章 磁盘配额与进阶文件系统管理

## Quota

P628 一个分配用户能使用的空间容量的东西

## 软件磁盘阵列

### RAID

磁盘阵列全名是『 Redundant Arrays of Inexpensive Disks, RAID 』，英翻中的意思是：容错式廉价磁 盘阵列。 RAID 可以透过一个技术(软件或硬件)，将多个较小的磁盘整合成为一个较大的磁盘装置； 而这个较大的磁盘功能可不止是储存而已，他还具有数据保护的功能。

### 优点

1. 数据安全与可靠性：指的并非网络信息安全，而是当硬件 (指磁盘) 损毁时，数据是否还能够安全的救援或 使用之意； 
2. 读写效能：例如 RAID 0 可以加强读写效能，让你的系统 I/O 部分得以改善；
3. 容量：可以让多颗磁盘组合起来，故单一文件系统可以有相当大的容量。

## 逻辑滚动条管理员 (Logical Volume Manager)LVM

LVM 的重点在于『可以弹性的调整 filesystem 的容量！』而并非在于效能与数据保全上面。 需要 文件的读写效能或者是数据的可靠性，请参考前面的 RAID 小节。LVM 可以整合多个实体 partition  在一起， 让这些 partitions 看起来就像是一个磁盘一样！而且，还可以在未来新增或移除其他的实体 partition 到这个 LVM 管理的磁盘当中。

LVM 的作法是将几个实体的 partitions (或 disk) 透过软件组合成为一块看起来是独立的大磁盘 (VG) ，然后将这块大磁盘再经过 分区成为可使用分区槽 (LV)， 最终就能够挂载使用了。

# 第十五章 例行性工作排程

就是一个能指定linux在何时干何事的程序

# 第十六章 进程管理与SElinux初探

## PID

在 Linux 系统当中：『触发 任何一个事件时，系统都会将他定义成为一个进程，并且给予这个进程一个 ID ，称为 PID，同时 依据启发这个进程的用户与相关属性关系，给予这个 PID 一组有效的权限设定。』

为了操作系统可管理这个进程，因此进程有给予执行者的权限/属性等参 数，并包括程序所需要的脚本与数据或文件数据等， 最后再给予一个 PID 。系统就是透过这个 PID  来判断该 process 是否具有权限进行工作。

连续执行两个 bash 后，第二 个 bash 的父进程就是前一个 bash。因为每个进程都有一个 PID ，那某个进程的父进程该如何判断？ 就透过 Parent PID (PPID) 来判断即可。

常驻在内存当中的进程通常都是负责一些系统所提供的功能以服务用户各项任务，因此这些常驻程序 就会被我们称为：服务 (daemon)。

## 前景与背景

p700

如果想要同时进行多个工作， 那么可以将某些 工作直接丢到背景环境当中，让我们可以继续操作前景的工作

在输入一个指令后，在该指令的最后面加上一个『 & 』代表将该指令丢到背景中， 此时 bash 会给予这个指令一个『工作号码(job number)』，就是那个 [1] 啦！至于后面那个 14432 则 是该指令所触发的『 PID 』了！而且，有趣的是，我们可以继续操作 bash 呢！很不赖吧！

在背景当中执行的指令，如果有 stdout 及 stderr 时，他的数据依旧是输出到屏幕上面 的，所以，我们会无法看到提示字符，当然也就无法完好的掌握前景工作。同时由于是背景工作的 tar ， 此时你怎么按下 [ctrl]+c 也无法停止屏幕被搞的花花绿绿的！所以啰，最佳的状况就是利用数据流重 导向， 将输出数据传送至某个文件中。

最佳的状况就是利用数据流重 导向， 将输出数据传送至某个文件中。举例来说，我可以这样做：

```bash
[root@study ~]# tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/log.txt 2>&1 &
[1] 14547
[root@study ~]#
```

呵呵！如此一来，输出的信息都给他传送到 /tmp/log.txt 当中，当然就不会影响到我们前景的作业了。 这样说，您应该可以更清楚数据流重导向的重要性了吧！^_^

如果想要知道目前有多少的工作在背景当中，就用 jobs 这个指令

```bash
[root@study ~]# jobs -l
[1]- 14566 Stopped vim ~/.bashrc
[2]+ 14567 Stopped find / -prin
```

## 进程管理

```bash
[root@study ~]# ps aux <==观察系统所有的进程数据
[root@study ~]# ps -lA <==也是能够观察所有系统的数据
[root@study ~]# ps axjf <==连同部分进程树状态
选项与参数：
-A ：所有的 process 均显示出来，与 -e 具有同样的效用；
-a ：不与 terminal 有关的所有 process ；
-u ：有效使用者 (effective user) 相关的 process ；
x ：通常与 a 这个参数一起使用，可列出较完整信息。
输出格式规划：
l ：较长、较详细的将该 PID 的的信息列出；
j ：工作的格式 (jobs format)
-f ：做一个更为完整的输出。
```

top可以动态观察进程的状况

```bash
范例一：每两秒钟更新一次 top ，观察整体信息：
[root@study ~]# top -d 2
top - 00:53:59 up 6:07, 3 users, load average: 0.00, 0.01, 0.05
Tasks: 179 total, 2 running, 177 sleeping, 0 stopped, 0 zombie
%Cpu(s): 0.0 us, 0.0 sy, 0.0 ni,100.0 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem : 2916388 total, 1839140 free, 353712 used, 723536 buff/cache
KiB Swap: 1048572 total, 1048572 free, 0 used. 2318680 avail Me
```

kill可以强行关闭进程

## SELINUX(Security Enhanced Linux)

p733

很多企业发现， 通常系统出现问题的原因大部分都在于『内部员工的资源误用』所导致的，实际由外部发动的攻击反而 没有这么严重。 

因此 SELinux 导入了委任式访问控制 (Mandatory Access Control,  MAC) 的方法！ 委任式访问控制 (MAC) 有趣啦！他可以针对特定的进程与特定的文件资源来进行权限的控管！ 也 就是说，即使你是 root ，那么在使用不同的进程时，你所能取得的权限并不一定是 root ， 而得要 看当时该进程的设定而定。如此一来，我们针对控制的『主体』变成了『进程』而不是使用者喔！

再次的重复说明一下，SELinux 是透过 MAC 的方式来控管进程，他控制的主体是进程

# 第十七章 认识系统服务daemons

 daemon 的字面上的意思就是『守护神、 恶魔』

简单的说，系统为了某些功能必须要提供一些服务 (不论是系统本身还是网络方面)，这个服务就称 为 service 。 但是 service 的提供总是需要程序的运作吧！否则如何执行呢？所以达成这个 service  的程序我们就称呼他为 daemon 啰！

从 CentOS 7.x 以后，Red Hat 系列的 distribution 放弃沿用多年的 System V 开机启动服务的流程， 就是前一小节提到的 init 启动脚本的方法， 改用 systemd 这个启动服务管理机制

# 第十八章 认识和分析登陆档

log就是日志，能够记录系统在何时做了何事

# 第十九章 开机流程、模块管理与loader

# 第二十章 基础系统设定与备份策略
