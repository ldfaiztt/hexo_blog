title: 12个scp传输文件的命令栗子
date: 2015-01-10 13:13:25
tags: [Scp,Linux,Translation]
categories: [Linux,Translation]
---

一直在用scp进行简单的远程复制文件的功能，今天无意间看到一篇介绍scp的文章，便想着学习学习并将其翻译了过来。原文戳这里[12 scp command examples to transfer files on Linux](http://www.binarytides.com/linux-scp-command/)。翻译不对的地方，敬请指正。

## 安全复制

Scp(Secure Copy)
是一个在各个主机之间进行复制或者文件传输的一个命令行工具。它使用一种同ssh一样的安全机制。事实上，它在后台使用ssh连接来进行文件的传输。scp既指一种定义安全复制应该如何工作的协议，也指一种可以被安装的作为OpenSSH工具套的一部分的软件或是指令。

在这篇简单的教程中，我们可以看到一些scp指令的栗子以及如何使用它进行安全的文件传输。

## 使用scp

scp的基础语法很容易记忆，它看起来就像酱紫：

	$ scp source_file_path destination_file_path

根据不同的主机，文件路径应该包扩：完整的主机地址，端口号，用户名，密码以及文件路径。

所以如果你正在从你的本地计算机“发送”文件到远程计算机（上传）的语法是这样的：

	$ scp ~/my_local_file.txt user@remote_host.com:/some/remote/directory

当从远程主机复制文件到本地主机（下载），他看起来正好相反：

	$ scp user@remote_host.com:/some/remote/directory ~/my_local_file.txt
	
	# just download the file
	$ scp user@192.168.1.3:/some/path/file.txt .

这里很多是有关用scp来完成常规任务的。除了这些，scp也支持很多其他的选项和功能。让我们快速看一下他们的综述。

没错，默认情况下，scp总是覆盖目标地址的文件。如果你想避免它，那就使用功能更为强大的rsync工具吧。

##1.详细输出

有了详细的输出，SCP的程序将输出大量关于它在后台做什么的信息。当程序失败或无法完成请求时这是非常有用的。详细的输出将正确的指明该程序哪里出了问题。

栗子：

	$ scp -v ~/test.txt root@192.168.1.3:/root/help2356.txt
	Executing: program /usr/bin/ssh host 192.168.1.3, user root, command scp -v -t /root/help2356.txt
	OpenSSH_6.2p2 Ubuntu-6ubuntu0.1, OpenSSL 1.0.1e 11 Feb 2013
	debug1: Reading configuration data /home/enlightened/.ssh/config
	debug1: Reading configuration data /etc/ssh/ssh_config
	debug1: /etc/ssh/ssh_config line 19: Applying options for *
	debug1: Connecting to 192.168.1.3 [192.168.1.3] port 22.
	debug1: Connection established.
	..... OUTPUT TRUNCATED

输出的信息将会很多，而且包含有关连接如何建立，正在使用什么配置和认证文件等等的详细信息。

##2.多文件传输

多个文件可以像下面那样用空格分隔开

栗子：

	$ scp foo.txt bar.txt username@remotehost:/path/directory/

从远程主机复制多个文件到当前目录

栗子：

	$ scp username@remotehost:/path/directory/\{foo.txt,bar.txt\} .

	$ scp root@192.168.1.3:~/\{abc.log,cde.txt\} .


##3.复制整个文件夹（递归的复制）

为了从一个主机往另一个主机复制整个文件夹，需要使用r switch并且指定目录

栗子如下：

	$ scp -v -r ~/Downloads root@192.168.1.3:/root/Downloads

##4.在两个远程主机之间复制文件

scp也可以把文件从一个远程主机复制到另一个远程主机。

举个栗子：

	$ scp user1@remotehost1:/some/remote/dir/foobar.txt user2@remotehost2:/some/remote/dir/

##5.用压缩来加快传输

一个用于加快传输，节省时间和带宽的超酷的选项！你所需要做的就是用C选项来启用压缩功能。该文件在传输过程中被压缩，在目的主机上被解压缩。

栗子如下：	

	$ scp -vrC ~/Downloads root@192.168.1.3:/root/Downloads

在上面的栗子中我们开启压缩选项移动了整个文件夹。速度的增长取决于多少文件能被压缩。

##6.限制带宽的使用

如果你不想scp占用所有的带宽，那么用选项“l”来限制最大传输速度，Kbit/s

栗子如下：
	
	$ scp -vrC -l 400 ~/Downloads root@192.168.1.3:/root/Downloads

##7.在远程主机上连接一个不同的端口

如果远程服务器有ssh守护进程运行在不同的端口上（默认是22），那么你需要告诉scp使用“-P”选项来使用指定的端口。

栗子如下：

	$ scp -vC -P 2200 ~/test.txt root@192.168.1.3:/some/path/test.txt

##8.保存文件属性

“-p”选项（小写），将会保存源文件的修改时间，访问时间以及方式。

举例如下：

	$ scp -C -p ~/test.txt root@192.168.1.3:/some/path/test.txt

##9.安静模式

在安静模式（“-p”选项），scp输出将会减少，并且不再显示进度表以及警告和诊断信息。

栗子如下：

	$ scp -vCq ~/test.txt root@192.168.1.3:/some/path/test.txt

##10.特殊标识文件

当使用基于秘钥认证（无密码）。你将使用特殊的包含私有秘钥的标识文件。这个选项直接传递到ssh命令并且以同样的方式工作。

举个栗子：

	$ scp -vCq -i private_key.pem ~/test.txt root@192.168.1.3:/some/path/test.txt

##11.使用不同的ssh_config文件

用"F"选项指定不同的ssh_config文件

栗子如下：

	$ scp -vC -F /home/user/my_ssh_config ~/test.txt root@192.168.1.3:/some/path/test.txt

##12.使用不同的加密

scp默认使用AES加密，有时候你可能想使用不同的加密。用不同的加密可能会加快转移过程，举例来说，blowfish和arcfour被认为比AES更快的存在（但是安全上不如AES）。

举个栗子：

	$ scp -c blowfish -C ~/local_file.txt username@remotehost:/remote/path/file.txt

在上面的栗子中我们用blowfish加密并同时压缩，这可以得到显著的速度上的提升，当然也取决于可用的带宽。


##总结

尽管SCP在安全地传输文件方面是非常有效的，它缺乏一个文件同步工具必要的功能。它所能做的就是复制粘贴上述所有文件从一个位置到另一个位置。

一个更强大的工具的Rsync它不仅具有SCP的所有功能，而且增加了更多的功能用来在2个主机智能同步文件。例如，它可以检查并上传只有修改过的文件，忽略现有的文件等等。



	

	


