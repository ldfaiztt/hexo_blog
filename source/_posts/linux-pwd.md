title: 玩转Linux之pwd命令
date: 2015-02-15 19:49:21
tags: [pwd,Linux]
categories: Linux
description: 本文介绍了linux命令中的pwd命令，并举了8个栗子帮助理解使用pwd，还介绍了pwd与/bin/pwd的区别
---

你有没有遇到过需要知道当前所在目录却无从得知？有没有想要复制出当前所在目录层次却不知如何下手？俗话说有困难找警察，想知道目录层次自然要找pwd了。那么问题来了：

##什么是pwd

pwd的意思是Print Working Directory，也就是打印工作目录，意如其名，就是说打印出用户当前所在目录，它会打印出从根目录（/）开始到当前所在目录的完整路径。这条命令是一条shell的内置命令，并且在大多数shell中都可以使用，如bash、Bourne shell，ksh、zsh等等。

**命令格式：**

	# pwd [OPTION]

**常用参数：**

选项 | 描述 |
-----|------|
 -L (即逻辑路径logical )    | 使用环境中的路径，即使包含了符号链接    |
 -P (即物理路径physical)    | 避免所有的符号链接    |
–help    | 显示帮助并退出    |
 –version|输出版本信息并退出|

 如果同时使用了‘-L‘和‘-P‘，‘-L‘会有更高的优先级。如果没有指定参数，pwd会避免所有的符号链接，也就是说会使用‘-P‘参数。好了下面介绍具体栗子。我们的栗子都是使用“/bin/pwd”的。那么它和“pwd”有什么区别呢？

 ##pwd与/bin/pwd的区别

 这有什么区别呢？直接使用“pwd”意味着使用shell内置的pwd。你的shell可能有不同版本的pwd。具体请参考手册。当你使用的是/bin/pwd时，我们调用的是二进制版本的命令。虽然二进制的版本有更多的选项，但是它们两者都能打印当前的目录。好了下面继续我们的pwd的栗子实战。

 ##8个pwd命令栗子

 **1.打印当前目录：**

	avi@tecmint:~$ /bin/pwd
	
	/home/avi

 **2.为文件夹创建一个符号链接（比如说在home目录下创建一个htm链接指向/var/www/html）。进入新创建的目录并打印出含有以及不含符号链接的目录：**

	avi@tecmint:~$ ln -s /var/www/html/ htm
	avi@tecmint:~$ cd htm

**3.从当前环境中打印目录即使它含有符号链接：**

	avi@tecmint:~$ /bin/pwd -L
	
	/home/avi/htm

**4.通过解析所有的符号链接来打印当前实际的物理目录：**

	avi@tecmint:~$ /bin/pwd -P
	
	/var/www/html 

**5.打印pwd命令的版本：**

	avi@tecmint:~$ /bin/pwd --version
	pwd (GNU coreutils) 8.23
	Copyright (C) 2014 Free Software Foundation, Inc.
	License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
	This is free software: you are free to change and redistribute it.
	There is NO WARRANTY, to the extent permitted by law.
	
	Written by Jim Meyering.

**6.打印所有含有可执行pwd的路径：**

	avi@tecmint:~$ type -a pwd
	
	pwd is a shell builtin
	pwd is /bin/pwd

**7.存储“pwd”命令的值到变量中（比如说：a ），并从中打印变量的值（对于观察shell脚本很重要）。**

	avi@tecmint:~$ a=$(pwd)
	avi@tecmint:~$ echo "Current working directory is : $a"
	
	Current working directory is : /home/avi

**8.一次性获取当前工作目录以及先前工作的目录：**

	avi@tecmint:~$ echo “$PWD $OLDPWD”
	
	/home /home/avi

 


 > 参考资料：<http://www.tecmint.com/pwd-command-examples/>



---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**