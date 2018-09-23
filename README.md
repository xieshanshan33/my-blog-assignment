# Linux基础

## 计算机硬件组成


运算器：计算机中执行各种算术和逻辑运算操作的部件

控制器：由程序计数器、指令寄存器、指令译码器、时序产生器和操作控制器组成，它是发布命令的“决策机构”，即完成协调和指挥整个计算机系统的操作

存储器：是现代信息技术中用于保存信息的记忆设备

输入设备：向计算机输入数据和信息的设备，是计算机与用户或其他设备通信的桥梁

输出设备：是计算机硬件系统的终端设备，用于接收计算机数据的输出显示、打印、声音、控制外围设备操作等，也是把各种计算结果数据或信息以数字、字符、图像、声音等形式表现出来

## Linux哲学思想

>  **一切都是一个文件（包括硬件）** 
>
> 小型，单一用途的程序
>
> 链接程序，共同完成复杂的任务
>
> 避免令人困惑的用户界面
>
> 配置数据存储在文本中


# Linux基础和帮助

## 终端terminal

* 设备终端

&ensp;&ensp;&ensp;&ensp; 键盘鼠标显示器

* 物理终端（/dev/console ）

&ensp;&ensp;&ensp;&ensp; 控制台console

* 虚拟终端(tty： teletypewriters， /dev/tty# #为[1-6])

&ensp;&ensp;&ensp;&ensp; tty 可有n个， Ctrl+Alt+F[1-6]

* 图形终端（/dev/tty7 ） startx, xwindows

&ensp;&ensp;&ensp;&ensp; CentOS 6: Ctrl + Alt + F7

&ensp;&ensp;&ensp;&ensp; CentOS 7: 在哪个终端启动，即位于哪个虚拟终端

* 串行终端（/dev/ttyS# ）

&ensp;&ensp;&ensp;&ensp; ttyS

* 伪终端（pty： pseudo-tty ， /dev/pts/# ）

&ensp;&ensp;&ensp;&ensp; pty, SSH远程连接

* 查看当前的终端设备：

&ensp;&ensp;&ensp;&ensp; tty

## shell

* Shell 是Linux系统的用户界面，提供了用户与内核进行交互操作的一种接口。它接收用户输入的命令并把它送入内核去执行

* shell也被称为LINUX的命令解释器（command interpreter）
* shell是一种高级程序设计语言

显示当前使用的shell

&ensp;&ensp;&ensp;&ensp; echo ${SHELL}

显示当前系统使用的所有shell

&ensp;&ensp;&ensp;&ensp; cat /etc/shells

## 命令提示符

在shell中可执行的命令有两类

&ensp;&ensp;&ensp;&ensp; 内部命令：由shell自带的，而且通过某命令形式提供
help 内部命令列表

&ensp;&ensp;&ensp;&ensp; enable cmd 启用内部命令

&ensp;&ensp;&ensp;&ensp; enable –n cmd 禁用内部命令

&ensp;&ensp;&ensp;&ensp; enable –n 查看所有禁用的内部命令

&ensp;&ensp;&ensp;&ensp; 外部命令：在文件系统路径下有对应的可执行程序文件

&ensp;&ensp;&ensp;&ensp; 查看路径： which -a |--skip-alias ; whereis

区别指定的命令是内部或外部命令

&ensp;&ensp;&ensp;&ensp; type COMMAND

## 执行外部命令

hash缓存表

&ensp;&ensp;&ensp;&ensp; 初始为空，在执行外部命令时默认在PATH下寻找该命令，并记录到hash表里，再次执行此命令时，首先会查看hash表，存在就执行，不存在去PATH寻找，提高调用速率

用法

&ensp;&ensp;&ensp;&ensp; hash 显示hash缓存

&ensp;&ensp;&ensp;&ensp; ash –l 显示hash缓存，可作为输入使用

&ensp;&ensp;&ensp;&ensp; hash –p path name 将命令全路径path起别名为name

&ensp;&ensp;&ensp;&ensp; hash –t name  打印缓存中name的路径

&ensp;&ensp;&ensp;&ensp; hash –d name 清除name缓存

&ensp;&ensp;&ensp;&ensp; hash –r 清除缓存

## 命令别名

定义别名NAME,等于执行命令VALUE

&ensp;&ensp;&ensp;&ensp; alias NAME ='VALUE'

命令行中定义的别名仅对当前shell进程有效，想永久有效，要定义在配置文件里

&ensp;&ensp;&ensp;&ensp; 仅对当前用户：~/.bashrc

&ensp;&ensp;&ensp;&ensp; 对所有用户有效：/etc/bashrc

编辑配置给出的新配置不会立即生效

bash进程重新读取配置文件
&ensp;&ensp;&ensp;&ensp; source /path/to/config_file

&ensp;&ensp;&ensp;&ensp; ./path/to/config_file

撤消别名： unalias

&ensp;&ensp;&ensp;&ensp; unalias [-a] name [name ...]

&ensp;&ensp;&ensp;&ensp; -a 取消所有别名

&ensp;&ensp;&ensp;&ensp; 如果别名同原命令同名，如果要执行原命令，可使用

&ensp;&ensp;&ensp;&ensp; \ALIASNAME

&ensp;&ensp;&ensp;&ensp; “ALIASNAME”

&ensp;&ensp;&ensp;&ensp; ‘ALIASNAME’

&ensp;&ensp;&ensp;&ensp; command ALIASNAME

&ensp;&ensp;&ensp;&ensp; /path/commmand

COMMAND [OPTIONS...] [ARGUMENTS...]

&ensp;&ensp;&ensp;&ensp; 选项：用于启用或关闭命令的某个或某些功能

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; 短选项： -c 例如： -l, -h

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; 长选项： --word 例如： --all, 
--human-readable

&ensp;&ensp;&ensp;&ensp; 参数：命令的作用对象，比如文件名，用户名等

* 注意: 

&ensp;&ensp;&ensp;&ensp; 多个选项以及多参数和命令之间使用空白字符分隔

&ensp;&ensp;&ensp;&ensp; 取消和结束命令执行： Ctrl+c， Ctrl+d

&ensp;&ensp;&ensp;&ensp; 多个命令可以用;符号分开

&ensp;&ensp;&ensp;&ensp; 一个命令可以用\分成多行

## 时间日期

date 显示和设置系统时间

&ensp;&ensp;&ensp;&ensp; date +%s

&ensp;&ensp;&ensp;&ensp; date -d @1509536033

hwclock， clock: 显示硬件时钟

&ensp;&ensp;&ensp;&ensp; -s, --hctosys 以硬件时钟为准，校正系统时钟

&ensp;&ensp;&ensp;&ensp; -w, --systohc 以系统时钟为准， 校正硬件时钟

时区： /etc/localtime

显示日历： cal –y

## 简单命令

关机： halt, poweroff

重启： reboot

&ensp;&ensp;&ensp;&ensp; -f: 强制，不调用shutdown

&ensp;&ensp;&ensp;&ensp; -p: 切断电源

关机或重启： shutdown

&ensp;&ensp;&ensp;&ensp; shutdown [OPTION]... [TIME] [MESSAGE]

&ensp;&ensp;&ensp;&ensp; -r: reboot

&ensp;&ensp;&ensp;&ensp; -h: halt

&ensp;&ensp;&ensp;&ensp; -c： cancel

TIME：无指定，默认相当于+1（CentOS7）

&ensp;&ensp;&ensp;&ensp; now: 立刻,相当于+0

&ensp;&ensp;&ensp;&ensp; +m: 相对时间表示法，几分钟之后；例如 +3

&ensp;&ensp;&ensp;&ensp; hh:mm: 指明具体时间

whoami: 显示当前登录有效用户

who: 系统当前所有的登录会话

w: 系统当前所有的登录会话及所做的操作

nano 文本编辑,写入或编辑文件

screen -S[SESSION] 创建会话

screen -x[SESSION] 加入会话

screen -r[SESSION] 恢复会话

screen -ls 显示已有会话

echo 显示字符

-E 默认不支持 \ 解释功能

-n 不自动换行

-e 启用 \ 字符的解释功能

&ensp;&ensp;&ensp;&ensp; \a 发出警告声

&ensp;&ensp;&ensp;&ensp; \b 退格键

&ensp;&ensp;&ensp;&ensp; \c 最后不加上换行符号

&ensp;&ensp;&ensp;&ensp; \n 换行且光标移至行首

&ensp;&ensp;&ensp;&ensp; \r 回车，即光标移至行首，但不换行

&ensp;&ensp;&ensp;&ensp; \t 插入tab

&ensp;&ensp;&ensp;&ensp; \\ 插入\字符

&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; \0nnn 插入nnn（八进制）所代表的ASCII字符

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; echo -e '\033[43;31;5mmagedu\033[0m'

&ensp;&ensp;&ensp;&ensp; \xHH插入HH（十六进制）所代表的ASCII数字（man 7 ascii）

echo "$VAR_NAME” 变量会替换，弱引用，识别变量不识别命令

echo '$VAR_NAME’ 变量不会替换，强引用，不识别变量和命令

`` 识别变量和命令

> 一个命令调用另一个命令，被调用的命令放在``里

## 命令行历史

重复前一个命令，有4种方法

&ensp;&ensp;&ensp;&ensp; 重复前一个命令使用pgup键，并回车执行

&ensp;&ensp;&ensp;&ensp; 按 !! 并回车执行

&ensp;&ensp;&ensp;&ensp; 输入 !-1 并回车执行

&ensp;&ensp;&ensp;&ensp; 按 Ctrl+p 并回车执行

!:0 执行前一条命令（去除参数）

Ctrl + n 显示当前历史中的下一条命令，但不执行

Ctrl + j 执行当前命令

!n 执行history命令输出对应序号n的命令

!-n 执行history历史中倒数第n个命令

!string 重复前一个以“string”开头的命令

!?string 重复前一个包含string的命令

!string:p 仅打印命令历史，而不执行

!$:p 打印输出 !$ （上一条命令的最后一个参数）的内容

!*:p 打印输出 !*（上一条命令的所有参数）的内容

^string 删除上一条命令中的第一个string

^string1^string2 将上一条命令中的第一个string1替换为string2

!:gs/string1/string2 将上一条命令中所有的string1都替换为 string2

Ctrl+g：从历史搜索模式退出

要重新调用前一个命令中最后一个参数

&ensp;&ensp;&ensp;&ensp; !$ 表示

&ensp;&ensp;&ensp;&ensp; Esc, .（点击Esc键后松开，然后点击 . 键）

&ensp;&ensp;&ensp;&ensp; Alt+ .（按住Alt键的同时点击 . 键）

## 调用历史参数

command !^ 利用上一个命令的第一个参数做cmd的参数

command !$ 利用上一个命令的最后一个参数做cmd的参数

command !* 利用上一个命令的全部参数做cmd的参数

command !:n 利用上一个命令的第n个参数做cmd的参数

command !n:^ 调用第n条命令的第一个参数

command !n:$ 调用第n条命令的最后一个参数

command !n:m 调用第n条命令的第m个参数

command !n:* 调用第n条命令的所有参数

command !string:^ 从命令历史中搜索以 string 开头的命令，并获取它的第一
个参数

command !string:$ 从命令历史中搜索以 string 开头的命令,并获取它的最后一
个参数

command !string:n 从命令历史中搜索以 string 开头的命令，并获取它的第n
个参数

command !string:* 从命令历史中搜索以 string 开头的命令，并获取它的所有
参数

## history

-c: 清空命令历史

-d offset: 删除历史中指定的第offset个命令

n: 显示最近的n条历史

-a: 追加本次会话新执行的命令历史列表至历史文件

-r: 读历史文件附加到历史列表

-w: 保存历史列表到指定的历史文件

-n: 读历史文件中未读过的行到历史列表

-p: 展开历史参数成多行，但不存在历史列表中

-s: 展开历史参数成一行，附加在历史列表后

HISTFILE：指定历史文件，默认为~/.bash_history

HISTSIZE：命令历史记录的条数

HISTFILESIZE：命令历史文件记录历史的条数

HISTTIMEFORMAT=“%F %T “ 显示时间

HISTIGNORE=“str1:str2*:… “忽略str1命令， str2开头的历史

存放在 /etc/profile 或 ~/.bash_profile

## 命令帮助

内部命令： help COMMAND 或 man bash

外部命令：COMMAND --help
COMMAND -h

## man命令

查看man手册页

&ensp;&ensp;&ensp;&ensp; man [章节] keyword

列出所有帮助

&ensp;&ensp;&ensp;&ensp; man –a keyword

搜索man手册

&ensp;&ensp;&ensp;&ensp; man -k keyword 列出所有匹配的页面

&ensp;&ensp;&ensp;&ensp; 使用 whatis 数据库

相当于whatis

&ensp;&ensp;&ensp;&ensp; man –f keyword

打印man帮助文件的路径

&ensp;&ensp;&ensp;&ensp; man –w [章节] keyword

man命令的操作方法：使用less命令实现

&ensp;&ensp;&ensp;&ensp; space, ^v, ^f, ^F: 向文件尾翻屏

&ensp;&ensp;&ensp;&ensp; b, ^b: 向文件首部翻屏

&ensp;&ensp;&ensp;&ensp; d, ^d: 向文件尾部翻半屏

&ensp;&ensp;&ensp;&ensp; u, ^u: 向文件首部翻半屏

&ensp;&ensp;&ensp;&ensp; RETURN, ^N, e, ^E or j or ^J: 向文件尾部翻一行 y or ^Y or ^P or k

&ensp;&ensp;&ensp;&ensp; or ^K：向文件首部翻一行

&ensp;&ensp;&ensp;&ensp; q: 退出

&ensp;&ensp;&ensp;&ensp; #：跳转至第#行

&ensp;&ensp;&ensp;&ensp; 1G: 回到文件首部

&ensp;&ensp;&ensp;&ensp; G：翻至文件尾部

/KEYWORD:从当前位置向文件尾部搜索，n：下一个，N：上一个

/KEYWORK:从当前位置向文件首部搜索，n：同方向下一个，N：反方向上一个
