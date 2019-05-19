[TOC]

# Linux

## Linux操作命令（一）

### 1.0 文件和目录属性 

#### 1.0.1 Linux 目录结构

| 可分享的(shareable)      | 不可分享的(unshareable)    |                     |
| ------------------------ | -------------------------- | ------------------- |
| 不变的(static)           | /usr (软件放置处)          | /etc (配置文件)     |
| /opt (第三方协力软件)    | /boot (开机与核心档)       |                     |
| 可变动的(variable)       | /var/mail (使用者邮件信箱) | /var/run (程序相关) |
| /var/spool/news (新闻组) | /var/lock (程序相关)       |                     |

> 四中类型:

1. 可分享的：

   可以分享给其他系统挂载使用的目录，所以包括执行文件与用户的邮件等数据， 是能够分享给网络上其他主机挂载用的目录；

2. 不可分享的：

		自己机器上面运作的装置文件或者是与程序有关的socket文件等， 由于仅与自身机器有关，所以当然就不适合分享给其他主机了。

3. 不变的：

		有些数据是不会经常变动的，跟随着distribution而不变动。 例如函式库、文件说明文件、系统管理员所管理的主机服务配置文件等等；

4. 可变动的：

		经常改变的数据，例如登录文件、一般用户可自行收受的新闻组等。

事实上，FHS针对目录树架构仅定义出三层目录底下应该放置什么数据而已，分别是底下这三个目录的定义：

/ (root, 根目录)：与开机系统有关；

/usr (unix software resource)：与软件安装/执行有关；

/var (variable)：与系统运作过程有关。

| 目录   | 应放置档案内容                                               |
| ------ | ------------------------------------------------------------ |
| /bin   | 系统有很多放置执行档的目录，但/bin比较特殊。因为/bin放置的是在单人维护模式下还能够被操作的指令。在/bin底下的指令可以被root与一般帐号所使用，主要有：cat,[chmod(修改权限)](http://hi.baidu.com/haifengjava/blog/item/e540a894c0f36a1bd21b70d1.html), chown, date, mv, mkdir, cp, bash等等常用的指令。 |
| /boot  | 主要放置开机会使用到的档案，包括Linux核心档案以及开机选单与开机所需设定档等等。Linux kernel常用的档名为：vmlinuz ，如果使用的是grub这个开机管理程式，则还会存在/boot/grub/这个目录。 |
| /dev   | 在Linux系统上，任何装置与周边设备都是以档案的型态存在于这个目录当中。 只要通过存取这个目录下的某个档案，就等于存取某个装置。比要重要的档案有/dev/null, /dev/zero, /dev/tty , /dev/lp*, / dev/hd*, /dev/sd*等等 |
| /etc   | 系统主要的设定档几乎都放置在这个目录内，例如人员的帐号密码档、各种服务的启始档等等。 一般来说，这个目录下的各档案属性是可以让一般使用者查阅的，但是只有root有权力修改。 FHS建议不要放置可执行档(binary)在这个目录中。 比较重要的档案有：/etc/inittab, /etc/init.d/, /etc/modprobe.conf, /etc/X11/, /etc/fstab, /etc/sysconfig/等等。 另外，其下重要的目录有：/etc/init.d/ ：所有服务的预设启动script都是放在这里的，例如要启动或者关闭iptables的话： /etc/init.d/iptables start、/etc/init.d/ iptables stop/etc/xinetd.d/ ：这就是所谓的super daemon管理的各项服务的设定档目录。/etc/X11/ ：与X Window有关的各种设定档都在这里，尤其是xorg.conf或XF86Config这两个X Server的设定档。                                                                                           /etc/group文件与/etc/passwd和/etc/shadow文件都是有关于系统管理员对用户和用户组管理时相关的文件。linux /etc/group文件是有关于系统管理员对用户和用户组管理的文件,linux用户组的所有信息都存放在/etc/group文件中。具有某种共同特征的用户集合起来就是用户组（Group）。用户组（Group）配置文件主要有 /etc/group和/etc/gshadow，其中/etc/gshadow是/etc/group的加密信息文件。 |
| /home  | 这是系统预设的使用者家目录(home directory)。 在你新增一个一般使用者帐号时，预设的使用者家目录都会规范到这里来。比较重要的是，家目录有两种代号：  ~ ：代表当前使用者的家目录，而 ~guest：则代表用户名为guest的家目录。 |
| /lib   | 系统的函式库非常的多，而/lib放置的则是在开机时会用到的函式库，以及在/bin或/sbin底下的指令会呼叫的函式库而已 。 什么是函式库呢？妳可以将他想成是外挂，某些指令必须要有这些外挂才能够顺利完成程式的执行之意。 尤其重要的是/lib/modules/这个目录，因为该目录会放置核心相关的模组(驱动程式)。 |
| /media | media是媒体的英文，顾名思义，这个/media底下放置的就是可移除的装置。 包括软碟、光碟、DVD等等装置都暂时挂载于此。 常见的档名有：/media/floppy, /media/cdrom等等。 |
| /mnt   | 如果妳想要暂时挂载某些额外的装置，一般建议妳可以放置到这个目录中。在古早时候，这个目录的用途与/media相同啦。 只是有了/media之后，这个目录就用来暂时挂载用了。 |
| /opt   | 这个是给第三方协力软体放置的目录 。 什么是第三方协力软体啊？举例来说，KDE这个桌面管理系统是一个独立的计画，不过他可以安装到Linux系统中，因此KDE的软体就建议放置到此目录下了。 另外，如果妳想要自行安装额外的软体(非原本的distribution提供的)，那么也能够将你的软体安装到这里来。 不过，以前的Linux系统中，我们还是习惯放置在/usr/local目录下。 |
| /root  | 系统管理员(root)的家目录。 之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时，该目录就能够拥有root的家目录，所以我们会希望root的家目录与根目录放置在同一个分区中。 |
| /sbin  | Linux有非常多指令是用来设定系统环境的，这些指令只有root才能够利用来设定系统，其他使用者最多只能用来查询而已。放在/sbin底下的为开机过程中所需要的，里面包括了开机、修复、还原系统所需要的指令。至于某些伺服器软体程式，一般则放置到/usr/sbin/当中。至于本机自行安装的软体所产生的系统执行档(system binary)，则放置到/usr/local/sbin/当中了。常见的指令包括：fdisk, fsck, ifconfig, init, mkfs等等。 |
| /srv   | srv可以视为service的缩写，是一些网路服务启动之后，这些服务所需要取用的资料目录。 常见的服务例如WWW, FTP等等。 举例来说，WWW伺服器需要的网页资料就可以放置在/srv/www/里面。呵呵，看来平时我们编写的代码应该放到这里了。 |
| /tmp   | 这是让一般使用者或者是正在执行的程序暂时放置档案的地方。这个目录是任何人都能够存取的，所以你需要定期的清理一下。当然，重要资料不可放置在此目录啊。 因为FHS甚至建议在开机时，应该要将/tmp下的资料都删除。 |

/etc：配置文件

/bin：重要执行档

/dev：所需要的装置文件

/lib：执行档所需的函式库与核心所需的模块

/sbin：重要的系统执行文件

这五个目录千万不可与根目录分开在不同的分区。



> /usr

| 目录          | 应放置文件内容                                               |
| ------------- | ------------------------------------------------------------ |
| /usr/X11R6/   | 为X Window System重要数据所放置的目录，之所以取名为X11R6是因为最后的X版本为第11版，且该版的第6次释出之意。 |
| /usr/bin/     | 绝大部分的用户可使用指令都放在这里。请注意到他与/bin的不同之处。(是否与开机过程有关) |
| /usr/include/ | c/c++等程序语言的档头(header)与包含档(include)放置处，当我们以tarball方式 (*.tar.gz 的方式安装软件)安装某些数据时，会使用到里头的许多包含档。 |
| /usr/lib/     | 包含各应用软件的函式库、目标文件(object file)，以及不被一般使用者惯用的执行档或脚本(script)。 某些软件会提供一些特殊的指令来进行服务器的设定，这些指令也不会经常被系统管理员操作， 那就会被摆放到这个目录下啦。要注意的是，如果你使用的是X86_64的Linux系统， 那可能会有/usr/lib64/目录产生 |
| /usr/local/   | 统管理员在本机自行安装自己下载的软件(非distribution默认提供者)，建议安装到此目录， 这样会比较便于管理。举例来说，你的distribution提供的软件较旧，你想安装较新的软件但又不想移除旧版， 此时你可以将新版软件安装于/usr/local/目录下，可与原先的旧版软件有分别啦。 你可以自行到/usr/local去看看，该目录下也是具有bin, etc, include, lib...的次目录 |
| /usr/sbin/    | 非系统正常运作所需要的系统指令。最常见的就是某些网络服务器软件的服务指令(daemon) |
| /usr/share/   | 放置共享文件的地方，在这个目录下放置的数据几乎是不分硬件架构均可读取的数据， 因为几乎都是文本文件嘛。在此目录下常见的还有这些次目录：/usr/share/man：联机帮助文件/usr/share/doc：软件杂项的文件说明/usr/share/zoneinfo：与时区有关的时区文件 |
| /usr/src/     | 一般原始码建议放置到这里，src有source的意思。至于核心原始码则建议放置到/usr/src/linux/目录下。 |

> /var

| 目录        | 应放置文件内容                                               |
| ----------- | ------------------------------------------------------------ |
| /var/cache/ | 应用程序本身运作过程中会产生的一些暂存档                     |
| /var/lib/   | 程序本身执行的过程中，需要使用到的数据文件放置的目录。在此目录下各自的软件应该要有各自的目录。 举例来说，MySQL的数据库放置到/var/lib/mysql/而rpm的数据库则放到/var/lib/rpm去 |
| /var/lock/  | 某些装置或者是文件资源一次只能被一个应用程序所使用，如果同时有两个程序使用该装置时， 就可能产生一些错误的状况，因此就得要将该装置上锁(lock)，以确保该装置只会给单一软件所使用。 举例来说，刻录机正在刻录一块光盘，你想一下，会不会有两个人同时在使用一个刻录机烧片？ 如果两个人同时刻录，那片子写入的是谁的数据？所以当第一个人在刻录时该刻录机就会被上锁， 第二个人就得要该装置被解除锁定(就是前一个人用完了)才能够继续使用 |
| /var/log/   | 非常重要。这是登录文件放置的目录。里面比较重要的文件如/var/log/messages, /var/log/wtmp(记录登入者的信息)等。 |
| /var/mail/  | 放置个人电子邮件信箱的目录，不过这个目录也被放置到/var/spool/mail/目录中，通常这两个目录是互为链接文件。 |
| /var/run/   | 某些程序或者是服务启动后，会将他们的PID放置在这个目录下      |
| /var/spool/ | 这个目录通常放置一些队列数据，所谓的“队列”就是排队等待其他程序使用的数据。 这些数据被使用后通常都会被删除。举例来说，系统收到新信会放置到/var/spool/mail/中， 但使用者收下该信件后该封信原则上就会被删除。信件如果暂时寄不出去会被放到/var/spool/mqueue/中， 等到被送出后就被删除。如果是工作排程数据(crontab)，就会被放置到/var/spool/cron/目录中。 |

![img](https://images.cnblogs.com/cnblogs_com/peida/linux%E7%9B%AE%E5%BD%95%E6%A0%91.png) 

> 绝对路径与相对路径 

绝对路径：

由根目录(/)开始写起的文件名或目录名称， 例如 /home/dmtsai/.bashrc；

相对路径：

相对于目前路径的文件名写法。 例如 ./home/dmtsai 或 http://www.cnblogs.com/home/dmtsai/ 等等。开头不是 / 就属于相对路径的写法。



.  ：代表当前的目录，也可以使用 ./ 来表示；

.. ：代表上一层目录，也可以 ../ 来代表。



> 如何先进入/var/spool/mail/目录，再进入到/var/spool/cron/目录内？ 

```sh
cd /var/spool/mail
cd ../cron
```



#### 1.0.2 Linux文件类型与扩展名

**一. 文件类型**

​	Linux文件类型常见的有：普通文件、目录文件、字符设备文件和块设备文件、符号链接文件等。

> 1、普通文件 

​	用 ls -lh 来查看某个文件的属性，可以看到有类似-rwxrwxrwx，第一个符号是 - ，这样的文件在Linux中就是普通文件。这些文件一般是用一些相关的应用程序创建，比如图像工具、文档工具、归档工具... .... 或 cp工具等。这类文件的删除方式是用rm 命令。 另外，依照文件的内容，又大略可以分为：

1）、 纯文本档(ASCII)：

​	这是Linux系统中最多的一种文件类型，称为纯文本档是因为内容为我们人类可以直接读到的数据，例如数字、字母等等。 几乎只要可以用来做为设定的文件都属于这一种文件类型。 举例来说，你可以用命令： cat ~/.bashrc 来看到该文件的内容。 (cat 是将一个文件内容读出来的指令).

2）、 二进制文件(binary)：

​	Linux系统其实仅认识且可以执行二进制文件(binary file)。Linux当中的可执行文件(scripts, 文字型批处理文件不算)就是这种格式的文件。 刚刚使用的命令cat就是一个binary file。

3）、 数据格式文件(data)： 

​	有些程序在运作的过程当中会读取某些特定格式的文件，那些特定格式的文件可以被称为数据文件 (data file)。举例来说，我们的Linux在使用者登录时，都会将登录的数据记录在 /var/log/wtmp那个文件内，该文件是一个data file，他能够透过last这个指令读出来！ 但是使用cat时，会读出乱码～因为他是属于一种特殊格式的文件？

> 2. 目录文件

​	当在某个目录下执行，看到有类似 drwxr-xr-x ，这样的文件就是目录，目录在Linux是一个比较特殊的文件。注意它的第一个字符是d。创建目录的命令可以用 mkdir 命令，或cp命令，cp可以把一个目录复制为另一个目录。删除用rm 或rmdir命令。 

> 3. 字符设备或块设备文件 

​	进入/dev目录，列一下文件，会看到类似如下的:

```sh
[root@localhost ~]# ls -al /dev/tty
crw-rw-rw- 1 root tty 5, 0 11-03 15:11 /dev/tty
[root@localhost ~]# ls -la /dev/sda1
brw-r----- 1 root disk 8, 1 11-03 07:11 /dev/sda1
```

​	/dev/tty的属性是 crw-rw-rw- ，注意前面第一个字符是 c ，这表示字符设备文件。比如猫等串口设备。 /dev/sda1 的属性是 brw-r----- ，注意前面的第一个字符是b，这表示块设备，比如硬盘，光驱等设备。

这个种类的文件，是用mknode来创建，用rm来删除。目前在最新的Linux发行版本中，我们一般不用自己来创建设备文件。因为这些文件是和内核相关联的。

与系统周边及储存等相关的一些文件， 通常都集中在/dev这个目录之下！通常又分为两种：

1）、区块(block)设备档 ：

​	就是一些储存数据， 以提供系统随机存取的接口设备，举例来说，硬盘与软盘等， 你可以随机的在硬盘的不同区块读写，这种装置就是成组设备！你可以自行查一下/dev/sda看看， 会发现第一个属性为[ b ]。

2）、字符(character)设备文件：

​	亦即是一些串行端口的接口设备， 例如键盘、鼠标等等！这些设备的特色就是一次性读取的，不能够截断输出。 举例来说，你不可能让鼠标跳到另一个画面，而是滑动到另一个地方！第一个属性为 [ c ]。

> 4. 数据接口文件(sockets)： 

​	数据接口文件（或者：套接口文件），这种类型的文件通常被用在网络上的数据承接了。我们可以启动一个程序来监听客户端的要求， 而客户端就可以透过这个socket来进行数据的沟通了。第一个属性为 [ s ]， 最常在/var/run这个目录中看到这种文件类型了。

例如：当我们启动MySQL服务器时，会产生一个mysql.sock的文件。

```sh
[root@localhost ~]# ls -lh /var/lib/mysql/mysql.sock 
srwxrwxrwx 1 mysql mysql 0 04-19 11:12 /var/lib/mysql/mysql.sock
```

注意这个文件的属性的第一个字符是 s。

> 5. 符号链接文件： 

​	当查看文件属性时，会看到有类似 lrwxrwxrwx,注意第一个字符是l，这类文件是链接文件。是通过ln -s 源文件名 新文件名 。上面是一个例子，表示setup.log是install.log的软链接文件。怎么理解呢？这和Windows操作系统中的快捷方式有点相似。

符号链接文件的创建方法举例:

```sh
[root@localhost test]# ls -lh log2012.log
-rw-r--r-- 1 root root 296K 11-13 06:03 log2012.log
[root@localhost test]# ln -s log2012.log  linklog.log
[root@localhost test]# ls -lh *.log
lrwxrwxrwx 1 root root   11 11-22 06:58 linklog.log -> log2012.log
-rw-r--r-- 1 root root 296K 11-13 06:03 log2012.log
```

> 6. 数据输送文件（FIFO,pipe）:

​	FIFO也是一种特殊的文件类型，他主要的目的在解决多个程序同时存取一个文件所造成的错误问题。 FIFO是first-in-first-out的缩写。第一个属性为[p] 。

**二. Linux文件扩展名**

> 1. 扩展名类型

​	基本上，Linux的文件是没有所谓的扩展名的，一个Linux文件能不能被执行，与他的第一栏的十个属性有关， 与档名根本一点关系也没有。这个观念跟Windows的情况不相同，在Windows底下， 能被执行的文件扩展名通常是 .com .exe .bat等等，而在Linux底下，只要你的权限当中具有x的话，例如[ -rwx-r-xr-x ] 即代表这个文件可以被执行。

​	不过，可以被执行跟可以执行成功是不一样的～举例来说，在root家目录下的install.log 是一个纯文本档，如果经由修改权限成为 -rwxrwxrwx 后，这个文件能够真的执行成功吗？ 当然不行～因为他的内容根本就没有可以执行的数据。所以说，这个x代表这个文件具有可执行的能力， 但是能不能执行成功，当然就得要看该文件的内容。

​	虽然如此，不过我们仍然希望可以藉由扩展名来了解该文件是什么东西，所以，通常我们还是会以适当的扩展名来表示该文件是什么种类的。底下有数种常用的扩展名：

*.sh ： 脚本或批处理文件 (scripts)，因为批处理文件为使用shell写成的，所以扩展名就编成 .sh 

*Z, *.tar, *.tar.gz, *.zip, *.tgz： 经过打包的压缩文件。这是因为压缩软件分别为 gunzip, tar 等等的，由于不同的压缩软件，而取其相关的扩展名！

*.html, *.php：网页相关文件，分别代表 HTML 语法与 PHP 语法的网页文件。 .html 的文件可使用网页浏览器来直接开启，至于 .php 的文件， 则可以透过 client 端的浏览器来 server 端浏览，以得到运算后的网页结果。

​	基本上，Linux系统上的文件名真的只是让你了解该文件可能的用途而已，真正的执行与否仍然需要权限的规范才行。例如虽然有一个文件为可执行文件，如常见的/bin/ls这个显示文件属性的指令，不过，如果这个文件的权限被修改成无法执行时，那么ls就变成不能执行。

​	上述的这种问题最常发生在文件传送的过程中。例如你在网络上下载一个可执行文件，但是偏偏在你的 Linux系统中就是无法执行！那么就是可能文件的属性被改变了。不要怀疑，从网络上传送到你的 Linux系统中，文件的属性与权限确实是会被改变的。

> 2. Linux文件名长度限制：

​	在Linux底下，使用预设的Ext2/Ext3文件系统时，针对文件名长度限制为：

​	单一文件或目录的最大容许文件名为 255 个字符

​	包含完整路径名称及目录 (/) 之完整档名为 4096 个字符

​	是相当长的档名！我们希望Linux的文件名可以一看就知道该文件在干嘛的， 所以档名通常是很长很长。

> 3. Linux文件名的字符的限制：

​	由于Linux在文字接口下的一些指令操作关系，一般来说，你在设定Linux底下的文件名时， 最好可以避免一些特殊字符比较好！例如底下这些：

```
* ? > < ; & ! [ ] | \ ' " ` ( ) { }
```

​	因为这些符号在文字接口下，是有特殊意义的。另外，文件名的开头为小数点“.”时， 代表这个文件为隐藏文件！同时，由于指令下达当中，常常会使用到 -option 之类的选项， 所以你最好也避免将文件档名的开头以 - 或 + 来命名。



#### 1.0.3 Linux文件属性详解

​	Linux 文件或目录的属性主要包括：文件或目录的节点、种类、权限模式、链接数量、所归属的用户和用户组、最近访问或修改的时间等内容。 

```sh
[root@localhost test2]# ls -lih
总用量 2.8M
25556546 -rw-r--r--. 1 root root 1.8M 5月  18 15:59 bili.log
25556547 -rw-r--r--. 1 root root    0 5月  18 08:11 test3.txt
25556548 -rw-r--r--. 1 root root    0 5月  18 08:11 text1.txt
25556583 -rw-r--r--. 1 root root   46 5月  18 09:23 text.txt
```

第一列：inode

第二列：文件种类和权限；

第三列： 硬链接个数；

第四列： 属主；

第五列：所归属的组；

第六列：文件或目录的大小；

第七列和第八列：最后访问或修改时间；

第九列：文件名或目录名



**inode 的值是**：25556546

**文件类型**：文件类型是-，表示这是一个普通文件； 关于文件的类型。

**文件权限**：文件权限是rw-r--r-- ，表示文件属主可读、可写、不可执行，文件所归属的用户组不可写，可读，不可执行，其它用户不可写，可读，不可执行；

**硬链接个数**： log2012.log这个文件没有硬链接；因为数值是1，就是他本身；

**文件属主**：也就是这个文件归哪于哪个用户 ，它归于root，也就是第一个root；

**文件属组**：也就是说，对于这个文件，它归属于哪个用户组，在这里是root用户组；

**文件大小**：文件大小是296k个字节；

**访问可修改时间** ：这里的时间是最后访问的时间，最后访问和文件被修改或创建的时间，有时并不是一致的。



​	inode 译成中文就是索引节点。每个存储设备或存储设备的分区（存储设备是硬盘、软盘、U盘等等）被格式化为文件系统后，应该有两部份，一部份是inode，另一部份是Block，Block是用来存储数据用的。而inode呢，就是用来存储这些数 据的信息，这些信息包括文件大小、属主、归属的用户组、读写权限等。inode为每个文件进行信息索引，所以就有了inode的数值。操作系统根据指令， 能通过inode值最快的找到相对应的文件。 

​	当我们用ls 查看某个目录或文件时，如果加上-i 参数，就可以看到inode节点了 ：

```sh
[root@localhost test2]# ls -li bili.log 
25556546 -rw-r--r--. 1 root root 1863420 5月  18 16:03 bili.log
```



### 1.1 文件目录操作命令 

#### 1.1.1 ls

​	ls 命令是 linux 下最常用的命令，ls 命令就是 list 的缩写。 ls 用来打印出当前目录的清单。如果 ls 指定其他目录，那么就会显示指定目录里的文件及文件夹清单。 通过 ls 命令不仅可以查看 linux 文件夹包含的文件，而且可以查看文件权限(包括目录、文件夹、文件权限)查看目录信息等等 。

​	命令格式：ls （选项）  目录名

| 参数 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| -a   | –all 列出目录下的所有文件，包括以 . 开头的隐含文件           |
| -l   | 除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来 |
| -d   | –directory 将目录象文件一样显示，而不是显示其下的文件        |
| -h   | –human-readable 以容易理解的格式列出文件大小 (例如 1K 234M 2G) |
| -t   | 以文件修改时间排序                                           |

参数可以连着用：`ls -alh /home`

#### 1.1.2 cd命令

​	cd 命令可以说是 Linux 中最基本的命令语句，其他的命令语句要进行操作，都是建立在使用 cd 命令上的。cd 命令是 change directory 的缩写,切换当前目录至指定的目录。

​	命令格式： cd 目录名

 	进入系统根目录：cd /

 	进入父目录：cd ..

​	进入当前用户主目录：cd ~

​	进入上次目录：cd -

#### 1.1.3 pwd

​	Linux 中用 pwd 命令来查看“当前工作目录”的完整路径。 简单得说，每当你在终端进行操作时，你都会有一个当前工作目录。 在不太确定当前位置时，就会使用 pwd 来判定当前目录在文件系统内的确切位置。 pwd 命令是 Print Working Directory 的缩写。 

​	命令格式：pwd （选项）

| 参数 | 描述                                       |
| ---- | ------------------------------------------ |
| -P   | 显示实际物理路径，而非使用连接（link）路径 |
| -L   | 当目录为连接路径时，显示连接路径           |

#### 1.1.4 mkdir

​	mkdir 命令用来创建指定名称的目录，要求创建目录的用户在当前目录中具有写权限，并且指定的目录名不能是当前目录中已有的目录。 mkdir 命令是 make directory 的缩写。 

​	命令格式：mkdir （选项） 目录

| 参数           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| -m --mode=模式 | 设定权限<模式>                                               |
| -p --parents   | 可以是一个路径名称。若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录 |
| -v --verbose   | 每次创建新目录都显示信息                                     |

创建空目录：`mkdir test1`

递归创建多个目录：`mkdir -p test1/test11`

创建权限为777的目录：`mkdir -m 777 test2`

创建新目录都显示信息：`mkdir -v test4`

一个命令创建项目的目录结构 :

`mkdir -vp scf/{lib/,bin/,doc/{info,product},logs/{info,product},service/deploy/{info,product}}` 



#### 1.1.5 rm

​	rm 是常用的命令，该命令的功能为删除一个目录中的一个或多个文件或目录，它也可以将某个目录及其下的所有文件及子目录均删除。对于链接文件，只是删除了链接，原有文件均保持不变。

​	rm 是一个危险的命令，使用的时候要特别当心，尤其对于新手，否则整个系统就会毁在这个命令（比如在/（根目录）下执行 rm * -rf）。所以，我们在执行 rm 之前最好先确认一下在哪个目录，到底要删除什么东西，操作时保持高度清醒的头脑。

​	rm 命令是 remove 的缩写。

​	命令格式： `rm (选项) 文件或目录`

| 参数             | 描述                                               |
| ---------------- | -------------------------------------------------- |
| -f --force       | 忽略不存在的文件，从不给出提示                     |
| -i --interactive | 进行交互式删除                                     |
| -r --recursive   | 指示 rm 将参数中列出的全部目录和子目录均递归地删除 |
| -v --verbose     | 详细显示进行的步骤                                 |

删除文件系统会先询问是否删除 ：`rm test1.log`

系统会先询问是否删除 : `rm -f test1.log`

删除后缀名为.log 的所有，删除前逐一询问 : `rm *.log`     `rm -i  *.log`

删除目录及子目录中所有: `rm -r test`

强制删除目录及子目录中所有：`rm -rf test`

删除以 -f 开头的文件：`rm -- -f`

```sh
root@localhost test]# touch -- -f
[root@localhost test]# ls -- -f
-f[root@localhost test]# rm -- -f
rm：是否删除 一般空文件 “-f”? y
[root@localhost test]# ls -- -f
ls: -f: 没有那个文件或目录
[root@localhost test]#
```



#### 1.1.6 mv

​	mv 命令功能是用来移动文件或更改文件名，是 Linux 系统下常用的命令，经常用来备份文件或者目录。 mv 命令根据第二个参数类型（是目标文件还是目标目录），决定执行将文件重命名或将其移至一个新的目录中。当第二个参数类型是文件时，mv 命令完成文件重命名，此时，源文件只能有一个（也可以是源目录名），它将所给的源文件或目录重命名为给定的目标文件名。当第二个参数是已存在的目录名称时，源文件或目录参数可以有多个，mv 命令将各参数指定的源文件均移至目标目录中。 mv 命令是 move 的缩写。 

​	命令格式： `mv (选项) 源文件或目录 目标文件或目录`

| 参数             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| -b --back        | 若需覆盖文件，则覆盖前先行备份                               |
| -f --force       | 如果目标文件已经存在，不会询问而直接覆盖                     |
| -i --interactive | 若目标文件已经存在时，就会询问是否覆盖                       |
| -u --update      | 若目标文件已经存在，且源文件比较新，才会更新                 |
| -t --target      | 该选项适用于移动多个源文件到一个目录的情况，此时目标目录在前，源文件在后 |

文件改名： `mv test1.log test2.txt`

移动文件：`mv text.txt /usr/test2`

移动多个文件：`mv text* ../test1`



#### 1.1.7 cp

​	cp 命令用来复制文件或者目录，是 Linux 系统中最常用的命令之一。一般情况下，shell 会设置一个别名，在命令行下复制文件时，如果目标文件已经存在，就会询问是否覆盖，不管你是否使用-i 参数。但是如果是在 shell 脚本中执行 cp 时，没有-i 参数时不会询问是否覆盖。这说明命令行和 shell 脚本的执行方式有些不同。 cp 命令是 copy 的缩写。 

​	命令格式：：`cp (选项)  源文件  目录`   `cp  (选项)  -t 目录  源文件`

| 参数                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| -t --target-directory | 指定目标目录                                                 |
| -i --interactive      | 覆盖前询问(使前面的 -n 选项失效)                             |
| -n --no-clobber       | 不要覆盖已存在的文件(使前面的 -i 选项失效)                   |
| -s --symbolic-link    | 对源文件建立符号链接，而非复制文件                           |
| -f --force            | 强行复制文件或目录， 不论目的文件或目录是否已经存在          |
| -u --update           | 使用这项参数之后，只会在源文件的修改时间较目的文件更新时，或是对应的目的文件并不存在，才复制文件 |

复制单个文件到目标目录：`cp test.log /usr/test2`

复制文件，覆盖前询问： `cp -i test1/*  test2`



#### 1.1.8 cat

​	cat 命令的功能是将文件或标准输入组合输出到标准输出。这个命令常用来显示文件内容，或者将几个文件连接起来显示，或者从标准输入读取内容并显示，它常与重定向符号配合使用。 cat 命令是 concatenate 的缩写。 

​	命令格式： `cat (选项) 文件`

| 参数                  | 描述                                             |
| --------------------- | ------------------------------------------------ |
| -A --show-all         | 等价于 -vET                                      |
| -b --number-nonblank  | 对非空输出行编号                                 |
| -e                    | 等价于 -vE                                       |
| -E --show-ends        | 在每行结束处显示 $                               |
| -n --number           | 对输出的所有行编号,由 1 开始对所有输出的行数编号 |
| -s --squeeze-blank    | 有连续两行以上的空白行，就代换为一行的空白行     |
| -t                    | 与 -vT 等价                                      |
| -T --show-tabs        | 将跳格字符显示为 ^I                              |
| -u                    | (被忽略)                                         |
| -v --show-nonprinting | 使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外          |

内容加上行号：`cat -n text.txt` 



#### 1.1.9 touch

​	linux的touch命令不常用，一般在使用make的时候可能会用到，用来修改文件时间戳，或者新建一个不存在的文件。 

​	命令格式：`touch (选项) 文件`

| 参数 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| -a   | 只更改存取时间                                               |
| -c   | 不建立任何文档                                               |
| -d   | 使用指定的日期时间，而非现在的时间                           |
| -m   | 只更改变动时间                                               |
| -r   | 把指定文档或目录的日期时间，统统设成和参考文档或目录的日期时间相同。 |
| -t   | 使用指定的日期时间，而非现在的时间。                         |



#### 1.1.10 nl

​	nl 命令在 linux 系统中用来计算文件中行号。nl 可以将输出的文件内容自动的加上行号。其默认的结果与 cat -n 有点不太一样， nl 可以将行号做比较多的显示设计，包括位数与是否自动补齐 0 等等的功能。 nl 命令是 number of lines 的缩写。 ·

​	命令格式： `nl （选项）文件`

| 参数  | 描述                                            |
| ----- | ----------------------------------------------- |
| -b    | 指定行号指定的方式，主要有两种：                |
| -b a  | 表示不论是否为空行，也同样列出行号(类似 cat -n) |
| -b t  | 如果有空行，空的那一行不要列出行号(默认值)      |
| -n    | 列出行号表示的方法，主要有三种：                |
| -n ln | 行号在屏幕的最左方显示                          |
| -n rn | 行号在自己栏位的最右方显示，且不加 0            |
| -n rz | 行号在自己栏位的最右方显示，且加 0              |
| -w    | 行号栏位的占用的位数                            |

#### 1.1.11 more

​	more 命令，功能类似 cat ，cat 命令是将整个文件的内容从上到下显示在屏幕上。 more 命令会一页一页的显示，方便使用者逐页阅读，而最基本的指令就是按空白键（space）往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能 。more 命令从前向后读取文件，因此在启动时就加载整个文件。 

​	命令格式： `more (选项) 文件`



| 参数      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| +n        | 从笫 n 行开始显示                                            |
| -n        | 定义屏幕大小为 n 行                                          |
| +/pattern | 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示 |
| -c        | 从顶部清屏，然后显示                                         |
| -d        | 提示“Press space to continue，’q’ to quiet”，禁用响铃功能    |
| -p        | 通过清除窗口而不是滚屏来对文件进行换页，与-c 选项相似        |
| -s        | 把连续的多个空行显示为一行                                   |
| -u        | 把文件内容中的下画线去掉                                     |

| 符号   | 描述             |
| ------ | ---------------- |
| =      | 输出当前行的行号 |
| q      | 退出 more        |
| 空格键 | 向下滚动一屏     |
| b      | 返回上一屏       |

显示文件中从第三行起的内容： `more +3 text.txt`

从文件中查找第一个出现"xxx"字符串的行，并从该处前两行开始显示输出  : `more +/xxx text.txt`

设定每屏显示行数 : `more +5 text.txt`

列一个目录下的文件，内容太多，用more来分页显示 : `ls -l | more -5`    `ll /etc | more -10`



#### 1.1.12 less命令

​	less 工具也是对文件或其它输出进行分页显示的工具，应该说是 linux 正统查看文件内容的工具，功能极其强大。 	

​	命令格式：`less (选项) 文件`

| 参数 | 描述                                                 |
| ---- | ---------------------------------------------------- |
| -e   | 当文件显示结束后，自动离开                           |
| -f   | 强迫打开特殊文件，例如外围设备代号、目录和二进制文件 |
| -i   | 忽略搜索时的大小写                                   |
| -m   | 显示类似 more 命令的百分比                           |
| -N   | 显示每行的行号                                       |
| -s   | 显示连续空行为一行                                   |

| 符号    | 描述                                 |
| ------- | ------------------------------------ |
| /字符串 | 向下搜索“字符串”的功能               |
| ?字符串 | 向上搜索“字符串”的功能               |
| n       | 重复前一个搜索（与 / 或 ? 有关）     |
| N       | 反向重复前一个搜索（与 / 或 ? 有关） |
| b       | 向前翻一页                           |
| d       | 向后翻半页                           |
| q       | 退出 less 命令                       |
| 空格键  | 向后翻一页                           |
| 向上键  | 向上翻动一行                         |
| 向下键  | 向下翻动一行                         |

查看文件： `less text.txt`

查看进程信息并通过less分页显示: `ps -ef |less`

查看命令历史使用记录并通过less分页显示 : `history | less`

浏览多个文件：`less text1.txt text2.txt`

**1.全屏导航**

ctrl + F - 向前移动一屏

ctrl + B - 向后移动一屏

ctrl + D - 向前移动半屏

ctrl + U - 向后移动半屏

**2.单行导航**

j - 向前移动一行

k - 向后移动一行

**3.其它导航**

G - 移动到最后一行

g - 移动到第一行

q / ZZ - 退出 less 命令

**4.其它有用的命令**

v - 使用配置的编辑器编辑当前文件

h - 显示 less 的帮助文档

&pattern - 仅显示匹配模式的行，而不是整个文件

**5.标记导航**

当使用 less 查看大文件时，可以在任何一个位置作标记，可以通过命令导航到标有特定标记的文本位置：

ma - 使用 a 标记文本的当前位置

'a - 导航到标记 a 处



#### 1.1.13 head

​	head 命令就像它的名字一样浅显易懂，主要是用来显示档案的开头至标准输出中，默认 head 命令打印其相应文件的开头 10 行。 

​	命令格式： `head (选项) 文件`

| 参数     | 描述       |
| -------- | ---------- |
| -q       | 隐藏文件名 |
| -v       | 显示文件名 |
| -c<字节> | 显示字节数 |
| -n<行数> | 显示的行数 |

显示文件的前5行：`head -n 5 text.txt` 

显示文件前n个字节 : `head -c 20 text.txt`

显示多个文件前5行内容： `head -n 5  text1.txt  text2.txt`

显示 文件中除了最后 5 行的内容 : `head -n -5 text.txt`



#### 1.1.14 tail

​	tail 命令主要用于显示指定文件末尾内容。常用查看日志文件。

​	命令格式： `tail (选项)  文件`

| 参数     | 描述               |
| -------- | ------------------ |
| -f       | 循环读取           |
| -q       | 不显示处理信息     |
| -v       | 显示详细的处理信息 |
| -c<字节> | 显示的字节数       |
| -n<行数> | 显示行数           |

显示文件末尾5行内容：`tail -n  5   log2019.log` 

循环查看文件内容 :  `ping www.bilibili.com   > bili.log &`    `tail -f bili.log` 

从第5行开始显示文件 : `tail -n  +5 bili.log`



### 1.2 文件查找命令

#### 1.2.1  which

​	which 命令的作用是，在 PATH 变量指定的路径中搜索可执行文件的所在位置。它一般用来确认系统中是否安装了指定的软件。 

​	命令格式  ：  `which  可执行文件名次`

```sh
[root@localhost test2]# which java
/usr/bin/java
[root@localhost test2]# which which 
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
        /usr/bin/alias
        /usr/bin/which
[root@localhost test2]# which cd
/usr/bin/cd
```



#### 1.2.2 whereis

​	whereis 命令主要用于定位可执行文件、源代码文件、帮助文件在文件系统中的位置。whereis 命令还具有搜索源代码、指定备用搜索路径和搜索不寻常项的能力。

​	whereis 命令查找速度非常快，这是因为它根本不是在磁盘中漫无目的乱找，而是在一个数据库中（/var/lib/mlocate/）查询。这个数据库是 Linux 系统自动创建的，包含有本地所有文件的信息，并且每天通过自动执行 updatedb 命令更新一次。也正是因为这个数据库要每天才更新一次，就会使得 whereis 命令的搜索结果有时候会不准确，比如刚添加的文件可能搜不到。

​	命令格式： `whereis (选项) 文件`

| 参数 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| -b   | 定位可执行文件                                               |
| -m   | 定位帮助文件                                                 |
| -s   | 定位源代码文件                                               |
| -u   | 搜索默认路径下除可执行文件、源代码文件、帮助文件以外的其它文件 |
| -B   | 指定搜索可执行文件的路径                                     |
| -M   | 指定搜索帮助文件的路径                                       |
| -S   | 指定搜索源代码文件的路径                                     |

```sh
[root@localhost test2]# whereis -b java
java: /usr/bin/java /usr/lib/java /etc/java /usr/share/java
[root@localhost test2]# whereis -m java
java: /usr/share/man/man1/java.1.gz
[root@localhost test2]# whereis -s java
java:[root@localhost test2]# 
[root@localhost test2]# 
```



#### 1.2.3 locate

​	locate 命令跟 whereis 命令类似，且它们使用的是相同的数据库。但 whereis 命令只能搜索可执行文件、联机帮助文件和源代码文件，如果要获得更全面的搜索结果，可以使用 locate 命令。

locate 命令使用了十分复杂的匹配语法，可以使用特殊字元（如’*’和’?’）来指定需要查找的样本。

​	命令格式：`locate (选项) （搜索字符串）`

| 参数 | 描述                           |
| ---- | ------------------------------ |
| -q   | 安静模式，不会显示任何错误讯息 |
| -n   | 至多显示 n 个输出              |
| -r   | 使用正规表达式做寻找的条件     |
| -V   | 显示版本讯息                   |

```sh
[root@localhost /]# locate /etc/sh
/etc/shadow
/etc/shadow-
/etc/shells
[root@localhost /]# locate /etc/*lou*
/etc/selinux/targeted/active/modules/100/cloudform
/etc/selinux/targeted/active/modules/100/cloudform/cil
/etc/selinux/targeted/active/modules/100/cloudform/hll
/etc/selinux/targeted/active/modules/100/cloudform/lang_ext
```



#### 1.2.4 find

​	find 命令主要作用是沿着文件层次结构向下遍历，匹配符合条件的文件，并执行相应的操作。Linux 下 find 命令提供了相当多的查找条件，功能很强大，对应的学习难度也比较大。 

​	命令格式： `find [选项][搜索路径] [表达式]`

默认路径是当前目录，默认表达式为-print。

表达式可能由下列成份组成：操作符、选项、测试表达式以及动作。

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| -print       | find 命令将匹配的文件输出到标准输出                          |
| -exec        | find 命令对匹配的文件执行该参数所给出的 shell 命令           |
| -name        | 按照文件名查找文件                                           |
| -type        | 查找某一类型的文件                                           |
| -prune       | 使用这一选项可以使 find 命令不在当前指定的目录中查找，如果同 时使用-depth 选项，那么-prune 将被 find 命令忽略 |
| -user        | 按照文件属主来查找文件                                       |
| -group       | 按照文件所属的组来查找文件                                   |
| -mtime -n +n | 按照文件的更改时间来查找文件，-n 表示文件更改时间距现在小于 n天，+n 表示文件更改时间距现在大于 n 天，find 命令还有-atime 和 -ctime 选项 |

> 打印当前目录下的文件目录列表 

`find .  -print`

> 打印当前目录下的文件目录列表 :

 `find .  -name '*.txt'  -print`

-iname 选项跟-name 选项作用一样，-iname 会忽略字母大小写。 

> 打印当前目录下所有以.txt 或.pdf 结尾的文件名 

 `find  .  \( -name "*.pdf" -or -name "*.txt" \)`

> 打印当前目录下所有**不**以.txt 结尾的文件名 :

 `find  .  ! -name "*.txt"`

> 查找指定时间（48h）内修改过的文件：

 `find -atime -2` 

> 查找当前所有目录并排序 : 

`find . -type d | sort` 

> 按大小查找文件(查找当前目录大于1K的文件  ) :  

`find . -size +1000c -print` 



**-exec** 

​	-exec  参数后面跟的是command命令，它的终止是以;为结束标志的，所以这句命令后面的分号是不可缺少的，考虑到各个系统中分号会有不同的意义，所以前面加反斜杠。

​	{}   花括号代表前面find查找出来的文件名。

​	使用find时，只要把想要的操作写在一个文件里，就可以用exec来配合find查找，很方便的。在有些操作系统中只允许-exec选项执行诸如l s或ls -l这样的命令。大多数用户使用这一选项是为了查找旧文件并删除它们。建议在真正执行rm命令删除文件之前，最好先用ls命令看一下，确认它们是所要删除的文件。 exec选项后面跟随着所要执行的命令或脚本，然后是一对儿{ }，一个空格和一个\，最后是一个分号。为了使用exec选项，必须要同时使用print选项。如果验证一下find命令，会发现该命令只输出从当前路径起的相对路径及文件名。



> ls -l命令放在find命令的-exec选项中  :

  `find . -type f -exec ls -l {} \;` 

```sh
[root@localhost test2]# find . -type f -exec ls -l {} \;
-rw-r--r--. 1 root root 0 5月  18 08:11 ./test3.txt
-rw-r--r--. 1 root root 0 5月  18 08:11 ./text1.txt
-rw-r--r--. 1 root root 46 5月  18 09:23 ./text.txt
-rw-r--r--. 1 root root 532822 5月  18 11:39 ./bili.log
```

> 在目录中查找更改时间在n日以前的文件并删除它们 

 `find . -type f -mtime +14 -exec rm {} \;`  

> 在目录中查找更改时间在n日以前的文件并删除它们，在删除之前先给出提示 

`find . -name "*.log" -mtime +5 -ok rm {} \;` 

> 查找文件移动到指定目录   

`find . -name "*.log" -exec mv {} .. \;` 

`find . -name "bili.log" -exec mv {} ../test1 \;`

> 用exec选项执行cp命令 

`find . -name  "bili.log" -exec cp {} ../test1 \;`



**xargs**

​	我们可以用管道将一个命令的 stdout（标准输出）重定向到另一个命令的 stdin（标准输入）。但有些命令只能以命令行参数的形式接收数据，而无法通过 stdin 接收数据流。在这种情况下，无法通过管道将数据重定向给这些命令。

​	这时 xargs 就可以发挥它的作用了，xargs 命令可以从标准输入接收输入，并把输入转换为一个特定的参数列表。

​	命令格式： `command | xargs (选项)  (command )`

​	xargs 命令应该紧跟在管道操作符之后，因为它以标准输入作为主要的源数据流。 

| 参数 | 描述                   |
| ---- | ---------------------- |
| -n   | 指定每行最大的参数数量 |
| -d   | 指定分隔符             |

>  查找系统中的每一个普通文件，然后使用xargs命令来测试它们分别属于哪类文件  

```sh
[root@localhost test1]# find -type f -print | xargs file
./text1.txt: empty
./text.txt:  empty
./bili.log:  ASCII text
```

> 在整个系统中查找内存信息转储文件(core dump) ，然后把结果保存到/usr/test2/core.log 文件中 

```sh
find / -name "core" -print | xargs echo "" > /usr/test1/core.log
```

> 在当前目录下查找所有用户具有读、写和执行权限的文件，并收回相应的写权限 

```sh
find . -perm -7 -print | xargs chmod o-w
```

> 用grep命令在所有的普通文件中搜索hostname这个词 

```sh
find . -type f -print | xargs grep "hostname
```

> 用grep命令在当前目录下的所有普通文件中搜索hostnames这个词 

```sh
find . -name \* -type f -print | xargs grep "hostnames"
```

​	\用来取消find命令中的*在shell中的特殊含义 

> 使用xargs执行mv 

```sh
find . -name "*.log" | xargs -i mv {} test4
```



### 1.3 文件打包和上传

#### 1.3.1  使用filezilla上传下载文件

![1558167251134](D:\Documents\GitHub\LinuxStuNote\img\1558167251134.png)



#### 1.3.2  tar

​	linux下最常用的打包程序就是tar了，使用tar程序打出来的包我们常称为tar包，tar包文件的命令通常都是以.tar结尾的。生成tar包后，就可以用其它的程序来进行压缩。 

> 1、格式命令

```sh
tar [必要参数] [选择参数] [文件]
```

> 2、命令功能

​	用来压缩和解压文件，tar本身不具有压缩功能，它是调用压缩功能实现的 。

> 3、命令参数

​	必要参数 ：

| 参数 | 描述                                       |
| :--: | :----------------------------------------- |
|  -A  | 新增压缩文件到已存在的压缩                 |
|  -B  | 设置区块大小                               |
|  -c  | 建立新的压缩文件                           |
|  -d  | 记录文件的差别                             |
|  -r  | 添加文件到已经压缩的文件                   |
|  -u  | 添加改变了和现有的文件到已经存在的压缩文件 |
|  -x  | 从压缩的文件中提取文件                     |
|  -t  | 显示压缩文件的内容                         |
|  -z  | 支持gzip解压文件                           |
|  -j  | 支持bzip2解压文件                          |
|  -Z  | 支持compress解压文件                       |
|  -v  | 显示操作过程                               |
|  -l  | 文件系统边界设置                           |
|  -k  | 保留原有文件不覆盖                         |
|  -m  | 保留文件不被覆盖                           |
|  -W  | 确认压缩文件的正确性                       |

​	可选参数:

|   参数    | 描述           |
| :-------: | -------------- |
|    -b     | 设置区块数目   |
|    -C     | 切换到指定目录 |
|    -f     | 指定压缩文件   |
|   -help   | 显示帮助信息   |
| --version | 显示版本信息   |

> 4、常见解压、压缩命令

**tar**

```sh
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
```

**.gz**

```
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName
```

**.tar.gz 和 .tgz**

```
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
```

**.bz2**

```
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName
```

**.tar.bz2**

```
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
```

**.bz**

```
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知
```

**.tar.bz**

```
解压：tar jxvf FileName.tar.bz
压缩：未知
```

**.Z**

```
解压：uncompress FileName.Z
压缩：compress FileName

.tar.Z
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName

.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName
```

**.tar.Z**

```
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName
```

**.zip**

```
解压：unzip FileName.zip
压缩：zip FileName.zip DirName
```

**.rar** 

```
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName 
```

> 5、使用例子

1)、将文件打包成tar包：

```sh
仅打包，不压缩:
tar -cvf test2.tar test2
打包后，以 gzip 压缩:
tar -zcv test2.tar.gz test2
打包后，以 bzip2 压缩:
tar -jcvf test2.tar.bz2 test2
```

2）、查阅上述 tar包内有哪些文件 ：

​	由于使用 gzip 压缩的log.tar.gz，所以要查阅log.tar.gz包内的文件时，就得要加上 z 这个参数 。

```sh
[root@localhost usr]# tar -ztvf test2.tar.gz
drwxr-xr-x root/root         0 2019-05-18 13:53 test2/
-rw-r--r-- root/root         0 2019-05-18 08:11 test2/test3.txt
-rw-r--r-- root/root         0 2019-05-18 08:11 test2/text1.txt
-rw-r--r-- root/root        46 2019-05-18 09:23 test2/text.txt
-rw-r--r-- root/root   2082793 2019-05-18 16:46 test2/bili.log
```

3）、将tar 包解压缩 ：

```sh
[root@localhost usr]# tar -zxvf test2.tar.gz test2
test2/
test2/test3.txt
test2/text1.txt
test2/text.txt
test2/bili.log
```

4）、文件备份下来，并且保存其权限  ：

```sh
tar -zcvpf log31.tar.gz log2014.log log2015.log log2016.log
```

5)、在 文件夹当中，比某个日期新的文件才备份 ：

```sh
tar -N "2012/11/13" -zcvf log17.tar.gz test
```

6）、备份文件夹内容是排除部分文件 ：

```sh
tar --exclude scf/service -zcvf scf.tar.gz scf/*
```

#### 1.3.3 gzip

​	减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。gzip是在Linux系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。gzip不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和tar命令一起构成Linux操作系统中比较流行的压缩文件格式。据统计，gzip命令对文本文件有60%～70%的压缩率。 

> 1、命令格式

```
gzip [参数] [文件或目录]
```

> 2、命令功能

​	gzip是个使用广泛的压缩程序，文件经它压缩过后，其名称后面会多出".gz"的扩展名。 

> 3、命令参数

| 参数 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| -a   | 使用ASCII文字模式。                                          |
| -c   | 把压缩后的文件输出到标准输出设备，不去更动原始文件。         |
| -d   | 解开压缩文件。                                               |
| -f   | 强行压缩文件。不理会文件名称或硬连接是否存在以及该文件是否为符号连接。 |
| -h   | 在线帮助。                                                   |
| -l   | 列出压缩文件的相关信息。                                     |
| -L   | 显示版本与版权信息。                                         |
| -n   | 压缩文件时，不保存原来的文件名称及时间戳记。                 |
| -N   | 压缩文件时，保存原来的文件名称及时间戳记。                   |
| -q   | 不显示警告信息。                                             |
| -r   | 递归处理，将指定目录下的所有文件及子目录一并处理。           |
| -S   | 更改压缩字尾字符串。                                         |
| -t   | 测试压缩文件是否正确无误。                                   |
| -v   | 显示指令执行过程。                                           |
| -V   | 显示版本信息。                                               |
| -num | 用指定的数字num调整压缩的速度，-1或--fast表示最快压缩方法（低压缩比），-9或--best表示最慢压缩方法（高压缩比）。系统缺省值为6。 |

> 4、使用例子

1）、把test2目录下的每个文件压缩成.gz文件 `gzip *`

```sh
[root@localhost test2]# gzip *
[root@localhost test2]# ll
总用量 120
-rw-r--r--. 1 root root 106636 5月  18 16:46 bili.log.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 test3.txt.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 text1.txt.gz
-rw-r--r--. 1 root root     53 5月  18 09:23 text.txt.gz
```

2)、把test2每个压缩的文件解压，并列出详细的信息 `gzip -dv *`

```sh
[root@localhost test2]# ll
总用量 120
-rw-r--r--. 1 root root 106636 5月  18 16:46 bili.log.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 test3.txt.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 text1.txt.gz
-rw-r--r--. 1 root root     53 5月  18 09:23 text.txt.gz
[root@localhost test2]# gzip -dv *
bili.log.gz:     94.9% -- replaced with bili.log
test3.txt.gz:     0.0% -- replaced with test3.txt
text1.txt.gz:     0.0% -- replaced with text1.txt
text.txt.gz:     43.5% -- replaced with text.txt
[root@localhost test2]# ll
总用量 2040
-rw-r--r--. 1 root root 2082793 5月  18 16:46 bili.log
-rw-r--r--. 1 root root       0 5月  18 08:11 test3.txt
-rw-r--r--. 1 root root       0 5月  18 08:11 text1.txt
-rw-r--r--. 1 root root      46 5月  18 09:23 text.txt
```

3)、详细显示例1中每个压缩的文件的信息，并不解压 `gzip -l *` 

```sh
[root@localhost test2]# ll
总用量 120
-rw-r--r--. 1 root root 106636 5月  18 16:46 bili.log.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 test3.txt.gz
-rw-r--r--. 1 root root     30 5月  18 08:11 text1.txt.gz
-rw-r--r--. 1 root root     53 5月  18 09:23 text.txt.gz
[root@localhost test2]# gzip -l *
         compressed        uncompressed  ratio uncompressed_name
             106636             2082793  94.9% bili.log
                 30                   0   0.0% test3.txt
                 30                   0   0.0% text1.txt
                 53                  46  43.5% text.txt
             106749             2082839  94.9% (totals)
```

4）、压缩一个tar备份文件，此时压缩文件的扩展名为.tar.gz 

```sh
gzip -r log.tar
```

5）、递归的压缩目录 、解压目录 

```sh
gzip -rv test6
gzip -dr test6
```



### 1.4 linux文件权限设置 

#### 1.4.1  chmod

​	chmod命令用于改变linux系统文件或目录的访问权限。用它控制文件或目录的访问权限。该命令有两种用法。一种是包含字母和操作符表达式的文字设定法；另一种是包含数字的数字设定法。 

​	Linux系统中的每个文件和目录都有访问许可权限，用它来确定谁可以通过何种方式对文件和目录进行访问和操作。 　　

​	文件或目录的访问权限分为只读，只写和可执行三种。以文件为例，只读权限表示只允许读其内容，而禁止对其做任何的更改操作。可执行权限表示允许将该文件作为一个程序执行。文件被创建时，文件所有者自动拥有对该文件的读、写和可执行权限，以便于对文件的阅读和修改。用户也可根据需要把访问权限设置为需要的任何组合。 　　

​	有三种不同类型的用户可对文件或目录进行访问：文件所有者，同组用户、其他用户。所有者一般是文件的创建者。所有者可以允许同组用户有权访问文件，还可以将文件的访问权限赋予系统中的其他用户。在这种情况下，系统中每一位用户都能访问该用户拥有的文件或目录。 　　

​	每一文件或目录的访问权限都有三组，每组用三位表示，分别为文件属主的读、写和执行权限；与属主同组的用户的读、写和执行权限；系统中其他用户的读、写和执行权限。当用ls -l命令显示文件或目录的详细信息时，最左边的一列为文件的访问权限 。

```sh
[root@localhost test2]# ls -al
总用量 120
drwxr-xr-x.  2 root root     84 5月  18 17:14 .
drwxr-xr-x. 15 root root    181 5月  18 17:09 ..
-rw-r--r--.  1 root root 106636 5月  18 16:46 bili.log.gz
-rw-r--r--.  1 root root     30 5月  18 08:11 test3.txt.gz
-rw-r--r--.  1 root root     30 5月  18 08:11 text1.txt.gz
-rw-r--r--.  1 root root     53 5月  18 09:23 text.txt.gz
```

​	第一列共有10个位置，第一个字符指定了文件类型。在通常意义上，一个目录也是一个文件。如果第一个字符是横线，表示是一个非目录的文件。如果是d，表示是一个目录。从第二个字符开始到第十个共9个字符，3个字符一组，分别表示了3组用户对文件或者目录的权限。权限字符用横线代表空许可，r代表只读，w代表写，x代表可执行。 

​	`- rw- r-- r--` 

​	是一个普通文件；属主有读写权限；与属主同组的用户只有读权限；其他用户也只有读权限。 

> 1、命令格式

```sh
chmod [-cfvR][-help][-version] mode file
```

> 2、命令功能

​	用于改变文件或目录的访问权限，用它控制文件或目录的访问权限。 

> 3、命令参数

​	必要参数：

| 参数 | 描述                                 |
| ---- | ------------------------------------ |
| -c   | 当发生改变时，报告处理信息           |
| -f   | 错误信息不输出                       |
| -R   | 处理指定目录以及其子目录下的所有文件 |
| -v   | 运行时显示详细处理信息               |

​	选择参数：

| 参数 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 读权限，数字4表示                                            |
| w    | 写权限，数字2表示                                            |
| x    | 执行权限，数字1表示                                          |
| -    | 删除权限，数字0表示                                          |
| s    | 特殊权限， 该命令有两种用法。一种是包含字母和操作符表达式的文字设定法；另一种是包含数字的数字设定法。 |

1）、文字设定法

```sh
chmod [who] [+|-|=] [mode] 文件名
```

2）、数字设定法

​	0表示没有权限，1表示可执行(x)权限，2表示可写(r)权限，4表示可读(w)权限 。然后将其相加。所以数字属性的格式应为3个从0到7的八进制数，其顺序是（u）（g）（o）。 如某个文件的属主有“读/写”二种权限，需要把4（可读）+2（可写）＝6（读/写）。 

```sh
chmod [mode] 文件名
```

`rwx`: 4+2+1=7

`rw-`:4+2=6

`r-x`:4+1=5



> 4、使用例子

1）、增加文件所有组可执行权限

​	文件属主（u） 增加执行权限；与文件属主同组用户（g） 增加执行权限；其他用户（o） 增加执行权限。 

```sh
[root@localhost test1]# ls -al text.txt 
-rw-r--r--. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chmod a+x text.txt 
[root@localhost test1]# ls -al text.txt 
-rwxr-xr-x. 1 root root 0 5月  18 07:59 text.txt
```

2）、同时修改不同用户权限 

```sh
[root@localhost test1]# ls -al text.txt 
-rwxr-xr-x. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chmod ug+w,o-x text.txt 
[root@localhost test1]# ls -al text.txt 
-rwxrwxr--. 1 root root 0 5月  18 07:59 text.txt
```

3）、删除文件权限 

```sh
[root@localhost test1]# ls -al text.txt 
-rwxrwxr--. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chmod a-x text.txt
[root@localhost test1]# ls -al text.txt 
-rw-rw-r--. 1 root root 0 5月  18 07:59 text.txt
```

4）、使用“=”设置权限  

```sh
[root@localhost test1]# chmod u=x text.txt
[root@localhost test1]# ls -al text.txt 
---xrw-r--. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chmod u=rwx text.txt 
[root@localhost test1]# ls -al text.txt 
-rwxrw-r--. 1 root root 0 5月  18 07:59 text.txt
```

5）、对一个目录及其子目录所有文件添加权限  

```sh
[root@localhost test2]# ls -al
总用量 120
drwxr-xr-x.  2 root root     84 5月  18 17:14 .
drwxr-xr-x. 15 root root    181 5月  18 17:09 ..
-rw-r--r--.  1 root root 106636 5月  18 16:46 bili.log.gz
-rw-r--r--.  1 root root     30 5月  18 08:11 test3.txt.gz
-rw-r--r--.  1 root root     30 5月  18 08:11 text1.txt.gz
-rw-r--r--.  1 root root     53 5月  18 09:23 text.txt.gz
[root@localhost test2]# chmod -R u+x *
[root@localhost test2]# ls -al
总用量 120
drwxr-xr-x.  2 root root     84 5月  18 17:14 .
drwxr-xr-x. 15 root root    181 5月  18 17:09 ..
-rwxr--r--.  1 root root 106636 5月  18 16:46 bili.log.gz
-rwxr--r--.  1 root root     30 5月  18 08:11 test3.txt.gz
-rwxr--r--.  1 root root     30 5月  18 08:11 text1.txt.gz
-rwxr--r--.  1 root root     53 5月  18 09:23 text.txt.gz
```

6）、数字修改权限

​	给file的属主分配读、写、执行(7)的权限，给file的所在组分配读、执行(5)的权限，给其他用户分配执行(1)的权限 。

```sh
[root@localhost test1]# ls -al text.txt 
-rwxrw-r--. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chmod 751 text.txt 
[root@localhost test1]# ls -al text.txt 
-rwxr-x--x. 1 root root 0 5月  18 07:59 text.txt
```



#### 1.4.2 chgrp

​	lunix系统里，文件或目录的权限的掌控以拥有者及所诉群组来管理。可以使用chgrp指令取变更文件与目录所属群组，这种方式采用群组名称或群组识别码都可以。Chgrp命令就是change group的缩写！要被改变的组名必须要在/etc/group文件内存在才行。 

> 1、命令格式

```
chgrp [选项] [组] [文件]
```

> 2、命令功能

​	chgrp命令可采用群组名称或群组识别码的方式改变文件或目录的所属群组。使用权限是超级用户。  

> 3、命令参数

​	必要参数：

| 参数             | 描述                                     |
| ---------------- | ---------------------------------------- |
| -c               | 当发生改变时输出调试信息                 |
| -f               | 不显示错误信息                           |
| -R               | 处理指定目录以及其子目录下的所有文件     |
| -v               | 运行时显示详细的处理信息                 |
| --dereference    | 作用于符号链接的指向，而不是符号链接本身 |
| --no-dereference | 作用于符号链接本身                       |

​	选择参数：

| 参数        | 描述            |
| ----------- | --------------- |
| --reference | =<文件或者目录> |
| --help      | 显示帮助信息    |
| -version    | 显示版本信息    |

> 4、使用例子

1）、改变文件的群组属性

​	`chgrp -v bin text.txt` 

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  18 08:02 text1.txt
-rwxr-x--x. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chgrp -v bin text.txt 
changed group of "text.txt" from root to bin
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  18 08:02 text1.txt
-rwxr-x--x. 1 root bin  0 5月  18 07:59 text.txt
```

2）、根据指定文件改变文件的群组属性  

​	改变文件text的群组属性，使得文件text1的群组属性和参考文件text的群组属性相同 。

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  19 10:42 text1.txt
-rwxr-x--x. 1 root bin  0 5月  18 07:59 text.txt
[root@localhost test1]# chgrp --reference=text.txt text1.txt
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root bin  0 5月  19 10:42 text1.txt
-rwxr-x--x. 1 root bin  0 5月  18 07:59 text.txt
```

3）、改变指定目录以及其子目录下的所有文件的群组属性 

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root bin  0 5月  19 10:47 text1.txt
-rwxr-x--x. 1 root bin  0 5月  18 07:59 text.txt
[root@localhost test1]# chgrp -R root *
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  19 10:47 text1.txt
-rwxr-x--x. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# 
```

4）、通过群组识别码改变文件群组属性 

​	通过群组识别码改变文件群组属性，100为users群组的识别码，具体群组和群组识别码可以去/etc/group文件中查看 。

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  19 10:49 text1.txt
-rwxr-x--x. 1 root root 0 5月  18 07:59 text.txt
[root@localhost test1]# chgrp -R 100 *
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 10:49 text1.txt
-rwxr-x--x. 1 root users 0 5月  18 07:59 text.txt
```



#### 1.4.3 chown

​	chown将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID；组可以是组名或者组ID；文件是以空格分开的要改变权限的文件列表，支持通配符。系统管理员经常使用chown命令，在将文件拷贝到另一个用户的名录下之后，让用户拥有使用该文件的权限。

> 1、命令格式  

```
chown [选项]...[所有者][:[组]] 文件...
```

> 2、命令功能

​	通过chown改变文件的拥有者和群组。在更改文件的所有者或所属群组时，可以使用用户名称和用户识别码设置。普通用户不能将自己的文件改变成其他的拥有者。其操作权限一般为管理员。 

> 3、命令参数

​	

| 参数          | 描述                                     |
| ------------- | ---------------------------------------- |
| -c            | 当发生改变时输出调试信息                 |
| -f            | 不显示错误信息                           |
| -R            | 处理指定目录以及其子目录下的所有文件     |
| -v            | 运行时显示详细的处理信息                 |
| -h            | 修复符号链接                             |
| --dereference | 作用于符号链接的指向，而不是符号链接本身 |

​	选择参数：

| 参数                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| --reference =<目录或文件>    | 把指定的目录/文件作为参考，把操作的文件/目录设置成参考文件/目录相同拥有者和群组 |
| --from =<当前用户：当前群组> | 只有当前用户和群组跟指定的用户和群组相同时才进行改变         |
| --help                       | 显示帮助信息                                                 |
| --version                    | 显示版本信息                                                 |

> 4、使用例子

1）、改变拥有者和群组

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 10:53 text1.txt
-rwxr-x--x. 1 root users 0 5月  18 07:59 text.txt
[root@localhost test1]# chown mail:mail text.txt
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 10:53 text1.txt
-rwxr-x--x. 1 mail mail  0 5月  18 07:59 text.txt
```

2）、改变文件拥有者和群组 

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 11:47 text1.txt
-rwxr-x--x. 1 mail mail  0 5月  18 07:59 text.txt
[root@localhost test1]# chown root: text.txt 
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 11:47 text1.txt
-rwxr-x--x. 1 root root  0 5月  18 07:59 text.txt
```

3）、改变文件群组

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 12:18 text1.txt
-rwxr-x--x. 1 root root  0 5月  18 07:59 text.txt
[root@localhost test1]# chown :mail text.txt 
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 12:18 text1.txt
-rwxr-x--x. 1 root mail  0 5月  18 07:59 text.txt
```

4）、改变指定目录以及其子目录下的所有文件的拥有者和群组 

```sh
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root users 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root users 0 5月  19 12:23 text1.txt
-rwxr-x--x. 1 root mail  0 5月  18 07:59 text.txt
[root@localhost test1]# chown -R -v root:root *
changed ownership of "test3.txt" from root:users to root:root
changed ownership of "text1.txt" from root:users to root:root
changed ownership of "text.txt" from root:mail to root:root
[root@localhost test1]# ll
总用量 0
lrwxrwxrwx. 1 root root 9 5月  18 08:09 test3.txt -> text1.txt
-rw-r--r--. 1 root root 0 5月  19 12:23 text1.txt
-rwxr-x--x. 1 root root 0 5月  18 07:59 text.txt
```



### 1.5 磁盘储存

#### 1.5.1 df

​	df命令的功能是用来检查linux服务器的文件系统的磁盘空间占用情况。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。 

> 1、命令格式

```sh
df [选项] [文件]
```

> 2、命令功能

​	显示指定磁盘文件的可用空间。如果没有文件名被指定，则所有当前被挂载的文件系统的可用空间将被显示。默认情况下，磁盘空间将以 1KB 为单位进行显示，除非环境变量 POSIXLY_CORRECT 被指定，那样将以512字节为单位进行显示 。

> 3、命令参数

必要参数：

| 参数      | 描述                                         |
| --------- | -------------------------------------------- |
| -a        | 显示文件系统列表                             |
| -h        | 方便阅读方式显示                             |
| -H        | 等于“-h”，但是计算式，1K=1000，而不是1K=1024 |
| -i        | 显示inode信息                                |
| -k        | 区块为1024字节                               |
| -l        | 只显示本地文件系统                           |
| -m        | 区块为1048576字节                            |
| --no-sync | 忽略 sync 命令                               |
| -P        | 输出格式为POSIX                              |
| --sync    | 在取得磁盘信息前，先执行sync命令             |
| -T        | 文件系统类型                                 |

选择参数

| 参数                    | 描述                         |
| ----------------------- | ---------------------------- |
| --block-size=<区块大小> | 指定区块大小                 |
| -t<文件系统类型>        | 只显示选定文件系统的磁盘信息 |
| -x<文件系统类型>        | 不显示选定文件系统的磁盘信息 |
| --help                  | 显示帮助信息                 |
| --version               | 显示版本信息                 |

> 4、使用例子

1）、显示磁盘使用情况 

```sh
[root@localhost test1]# df
文件系统          1K-块    已用     可用 已用% 挂载点
/dev/sda3      15815680 4171480 11644200   27% /
devtmpfs         915664       0   915664    0% /dev
tmpfs            931624       0   931624    0% /dev/shm
tmpfs            931624    9260   922364    1% /run
tmpfs            931624       0   931624    0% /sys/fs/cgroup
/dev/sda1       1038336  189196   849140   19% /boot
tmpfs            186328       0   186328    0% /run/user/0
```

​	第1列是代表文件系统对应的设备文件的路径名（一般是硬盘上的分区）；第2列给出分区包含的数据块（1024字节）的数目；第3，4列分别表示已用的和可用的数据块数目。用户也许会感到奇怪的是，第3，4列块数之和不等于第2列中的块数。这是因为缺省的每个分区都留了少量空间供系统管理员使用。即使遇到普通用户空间已满的情况，管理员仍能登录和留有解决问题所需的工作空间。清单中Use% 列表示普通用户空间使用的百分比，即使这一数字达到100％，分区仍然留有系统管理员使用的空间。最后，Mounted on列表示文件系统的挂载点。 

2）、以inode模式来显示磁盘使用情况 

```sh
[root@localhost test1]# df -i
文件系统         Inode 已用(I) 可用(I) 已用(I)% 挂载点
/dev/sda3      7912960  136611 7776349       2% /
devtmpfs        228916     385  228531       1% /dev
tmpfs           232906       1  232905       1% /dev/shm
tmpfs           232906     564  232342       1% /run
tmpfs           232906      16  232890       1% /sys/fs/cgroup
/dev/sda1       524288     354  523934       1% /boot
tmpfs           232906       1  232905       1% /run/user/0
```

3）、列出各文件系统的i节点使用情况 

```sh
[root@localhost usr]# df -ia
```

4）、列出文件系统的类型 

```sh
[root@localhost usr]# df -T
文件系统       类型        1K-块    已用     可用 已用% 挂载点
/dev/sda3      xfs      15815680 4171540 11644140   27% /
devtmpfs       devtmpfs   915664       0   915664    0% /dev
tmpfs          tmpfs      931624       0   931624    0% /dev/shm
tmpfs          tmpfs      931624    9260   922364    1% /run
tmpfs          tmpfs      931624       0   931624    0% /sys/fs/cgroup
/dev/sda1      xfs       1038336  189196   849140   19% /boot
tmpfs          tmpfs      186328       0   186328    0% /run/user/0
```

5）、以更易读的方式显示目前磁盘空间和使用情况  

​	-h更具目前磁盘空间和使用情况 以更易读的方式显示

​	-H根上面的-h参数相同,不过在根式化的时候,采用1000而不是1024进行容量转换

​	-k以单位显示磁盘的使用情况

​	-l显示本地的分区的磁盘空间使用率,如果服务器nfs了远程服务器的磁盘,那么在df上加上-l后系统显示的是过	滤nsf驱动器后的结果

​	-i显示inode的使用情况。linux采用了类似指针的方式管理磁盘空间影射.这也是一个比较关键应用

```sh
[root@localhost usr]# df -h
文件系统        容量  已用  可用 已用% 挂载点
/dev/sda3        16G  4.0G   12G   27% /
devtmpfs        895M     0  895M    0% /dev
tmpfs           910M     0  910M    0% /dev/shm
tmpfs           910M  9.1M  901M    1% /run
tmpfs           910M     0  910M    0% /sys/fs/cgroup
/dev/sda1      1014M  185M  830M   19% /boot
tmpfs           182M     0  182M    0% /run/user/0
[root@localhost usr]# df -H
文件系统        容量  已用  可用 已用% 挂载点
/dev/sda3        17G  4.3G   12G   27% /
devtmpfs        938M     0  938M    0% /dev
tmpfs           954M     0  954M    0% /dev/shm
tmpfs           954M  9.5M  945M    1% /run
tmpfs           954M     0  954M    0% /sys/fs/cgroup
/dev/sda1       1.1G  194M  870M   19% /boot
tmpfs           191M     0  191M    0% /run/user/0
[root@localhost usr]# df -lh
文件系统        容量  已用  可用 已用% 挂载点
/dev/sda3        16G  4.0G   12G   27% /
devtmpfs        895M     0  895M    0% /dev
tmpfs           910M     0  910M    0% /dev/shm
tmpfs           910M  9.1M  901M    1% /run
tmpfs           910M     0  910M    0% /sys/fs/cgroup
/dev/sda1      1014M  185M  830M   19% /boot
tmpfs           182M     0  182M    0% /run/user/0
[root@localhost usr]# df -k
文件系统          1K-块    已用     可用 已用% 挂载点
/dev/sda3      15815680 4171484 11644196   27% /
devtmpfs         915664       0   915664    0% /dev
tmpfs            931624       0   931624    0% /dev/shm
tmpfs            931624    9260   922364    1% /run
tmpfs            931624       0   931624    0% /sys/fs/cgroup
/dev/sda1       1038336  189196   849140   19% /boot
tmpfs            186328       0   186328    0% /run/user/0
```

#### 1.5.2 du

​	du命令也是查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的。

> 1、命令格式

```
du [选项] [文件]
```

> 2、命令功能

显示每个文件和目录的磁盘使用空间。

> 3、命令参数

| 参数                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| -a                     | 列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。 |
| -b                     | 显示目录或文件大小时，以byte为单位。                         |
| -c                     | 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。 |
| -k                     | 以KB(1024bytes)为单位输出。                                  |
| -m                     | 以MB为单位输出。                                             |
| -s                     | 仅显示总计，只列出最后加总的值。                             |
| -h                     | 以K，M，G为单位，提高信息的可读性。                          |
| -x                     | 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。 |
| -L<符号链接>           | 显示选项中所指定符号链接的源文件大小。                       |
| -S                     | 显示个别目录的大小时，并不含其子目录的大小。                 |
| -X<文件>               | 在<文件>指定目录或文件。                                     |
| --exclude=<目录或文件> | 略过指定的目录或文件。                                       |
| -D                     | 显示指定符号链接的源文件大小。                               |
| -H                     | 与-h参数相同，但是K，M，G是以1000为换算单位。                |
| -l                     | 重复计算硬件链接的文件 。                                    |

> 4、使用例子

1）、显示目录或者文件所占空间 

```sh
[root@localhost test2]# du
0       ./tx1
0       ./tx2
120     .
```

2）、显示指定文件所占空间 

```sh
[root@localhost test2]# du bili.log.gz 
108     bili.log.gz
```

3）、查看指定目录的所占空间 

```sh
[root@localhost usr]# du test2
0       test2/tx1
0       test2/tx2
120     test2
```

4）、显示多个文件所占空间 

```sh
[root@localhost test2]# du bili.log.gz  text.txt.gz 
108     bili.log.gz
4       text.txt.gz
```

5）、只显示总和的大小 

```sh
[root@localhost test2]# du -s
120  
[root@localhost usr]# du -s test2
120     test2
```

6）、方便阅读的格式显示 

```sh
[root@localhost usr]# du -h test2
0       test2/tx1
0       test2/tx2
120K    test2
```

7）、文件和目录都显示 

```sh
[root@localhost usr]# du -ah test2
108K    test2/bili.log.gz
4.0K    test2/test3.txt.gz
4.0K    test2/text1.txt.gz
4.0K    test2/text.txt.gz
0       test2/tx1
0       test2/tx2
120K    test2
```

8）、显示几个文件或目录各自占用磁盘空间的大小，还统计它们的总和 

```sh
[root@localhost test2]# du -c bili.log.gz  text.txt.gz 
108     bili.log.gz
4       text.txt.gz
112     总用量
```

9）、按照空间大小排序 

```sh
[root@localhost test2]# du|sort -nr|more
120     .
0       ./tx2
0       ./tx1
```

10）、输出当前目录下各个子目录所使用的空间 

```sh
[root@localhost test2]# du -h --max-depth=1
0       ./tx1
0       ./tx2
120K    .
```

### 1.6 性能监控和优化命令 

#### 1.6.1 top

​	top命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况，类似于Windows的任务管理器。 top是一个动态显示过程,即可以通过用户按键来不断刷新当前状态.如果在前台执行该命令,它将独占前台,直到用户终止该程序为止.比较准确的说,top命令提供了实时的对系统处理器的状态监视.它将显示系统中CPU最“敏感”的任务列表.该命令可以按CPU使用.内存使用和执行时间对任务进行排序；而且该命令的很多特性都可以通过交互式命令或者在个人定制文件中进行设定。

> 1、命令格式

```
top [参数]
```

> 2、命令功能

​	显示当前系统正在执行的进程的相关信息，包括进程ID、内存占用率、CPU占用率等 

> 3、命令参数

| 参数       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| -b         | 批处理                                                       |
| -c         | 切换显示模式，共有两种模式，一是只显示执行档的名称，另一种是显示完整的路径与名称S : 累积模式，会将己完成或消失的子行程 ( dead child process ) 的 CPU time 累积起来 |
| -I         | 忽略失效（闲置无用）过程                                     |
| -s         | 保密模式                                                     |
| -S         | 累积模式                                                     |
| -i<时间>   | 设置间隔时间                                                 |
| -u<用户名> | 指定用户名                                                   |
| -p<进程号> | 指定进程                                                     |
| -n<次数>   | 循环显示的次数                                               |

> 4、使用例子

1）、显示进程信息

```sh
[root@localhost /]# top
top - 14:45:38 up  5:52,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 131 total,   1 running, 130 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1863248 total,  1137464 free,   281092 used,   444692 buff/cache
KiB Swap:  4095996 total,  4095996 free,        0 used.  1321676 avail Mem 

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                
  6155 root      20   0  303076   6248   4820 S   0.3  0.3   0:27.10 vmtoolsd                               
     1 root      20   0  128120   6756   4132 S   0.0  0.4   0:06.20 systemd                                
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.03 kthreadd                               
     3 root      20   0       0      0      0 S   0.0  0.0   0:00.11 ksoftirqd/0                            
     5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H                           
     6 root      20   0       0      0      0 S   0.0  0.0   0:00.03 kworker/u256:0                         
     7 root      rt   0       0      0      0 S   0.0  0.0   0:00.23 migration/0                            
     8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh                                 
     9 root      20   0       0      0      0 S   0.0  0.0   0:06.32 rcu_sched                              
    10 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 lru-add-drain                          
    11 root      rt   0       0      0      0 S   0.0  0.0   0:06.84 watchdog/0                             
    12 root      rt   0       0      0      0 S   0.0  0.0   0:00.22 watchdog/1                             
    13 root      rt   0       0      0      0 S   0.0  0.0   0:00.21 migration/1                            
    14 root      20   0       0      0      0 S   0.0  0.0   0:00.61 ksoftirqd/1                            
    15 root      20   0       0      0      0 S   0.0  0.0   2:17.76 kworker/1:0                            
    16 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/1:0H                           
    18 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kdevtmpfs                              
    19 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 netns                                  
    20 root      20   0       0      0      0 S   0.0  0.0   0:00.01 khungtaskd                             
    21 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 writeback                              
    22 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kintegrityd                            
    23 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset                                 
    24 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset                                 
    25 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset                                 
    26 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kblockd                                
    27 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 md                                     
    28 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 edac-poller                            
    29 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 watchdogd                              
    35 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kswapd0 
```

**1.第一行，任务队列信息，同 uptime 命令的执行结果** 	

| 参数                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| 14:45:38                       | 当前系统时间                                                 |
| up  5:52                       | 系统运行了5小时52分钟（未重启）                              |
| 2 users                        | 当前有2个用户登陆                                            |
| load average: 0.00, 0.01, 0.05 | load average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。  oad average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了。 |

**2.第二行，Tasks — 任务（进程）** 

Tasks: 131 total,   1 running, 130 sleeping,   0 stopped,   0 zombie

系统现在共有131个进程，其中处于运行中的有1个，130个在休眠（sleep），stoped状态的有0个，zombie状态（僵尸）的有0个。 



**3.第三行，cpu状态信息**

%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

| 参数    | 说明                                         |
| ------- | -------------------------------------------- |
| 0.2 us  | 用户空间占用CPU的百分比                      |
| 0.2 sy  | 内核空间占用CPU的百分比                      |
| 0.0 ni  | 改变过优先级的进程占用CPU的百分比            |
| 99.7 id | 空闲CPU百分比                                |
| 0.0 wa  | IO等待占用CPU的百分比                        |
| 0.0 hi  | 硬中断（Hardware IRQ）占用CPU的百分比        |
| 0.0 si  | 软中断（Software Interrupts）占用CPU的百分比 |
| 0.0 st  | 实时                                         |

**4.第四行，内存状态**

KiB Mem :  1863248 total,  1137464 free,   281092 used,   444692 buff/cache

物理内存总量 、使用中的内存总量 、空闲内存总量 、缓存的内存量 



**5.第五行，swap交换分区信息** 

KiB Swap:  4095996 total,  4095996 free,        0 used.  1321676 avail Mem 

交换区总量 、使用的交换区总量 、空闲交换区总量 、缓冲的交换区总量 

​	对于内存监控，在top里我们要时刻监控第五行swap交换分区的used，如果这个数值在不断的变化，说明内核在不断进行内存和swap的数据交换，这是真正的内存不够用了。 

**6.第七行，各进程（任务）的状态监控** 

| 参数    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| PID     | 进程id                                                       |
| USER    | 进程所有者                                                   |
| PR      | 进程优先级                                                   |
| NI      | nice值。负值表示高优先级，正值表示低优先级                   |
| VIRT    | 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES                |
| RES     | 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA    |
| SHR     | 共享内存大小，单位kb                                         |
| S       | 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程 |
| %CPU    | 上次更新到现在的CPU时间占用百分比                            |
| %MEM    | 进程使用的物理内存百分比                                     |
| TIME+   | 进程使用的CPU时间总计，单位1/100秒                           |
| COMMAND | 进程名称（命令名/命令行）                                    |

**使用技巧**

1、多U多核CPU监控 

​	在top基本视图中，按键盘数字“1”，可监控每个逻辑CPU的状况

2、高亮显示当前运行进程 

​	敲击键盘“b”（打开/关闭加亮效果），可以通过敲击“y”键关闭或打开运行态进程的加亮效果 

3、进程字段排序 

​	默认进入top时，各进程是按照CPU的占用量来排序的 ，敲击键盘“x”（打开/关闭排序列的加亮效果），

通过”shift + >”或”shift + <”可以向右或左改变排序列 。



显示完整命令：`top -c` 

以批处理模式显示程序信息 ： `top -b`

以累积模式显示程序信息 :`top -S`

设置信息更新次数 :`top -n 2` (更新两次后终止更新显示 )

设置信息更新时间 : `top -d 3` (更新周期为3秒 )

显示指定的进程信息 : `top -p 574`



**top交互命令**

​	在top 命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了s 选项， 其中一些命令可能会被屏蔽。

h 显示帮助画面，给出一些简短的命令总结说明

k 终止一个进程。

i 忽略闲置和僵死进程。这是一个开关式命令。

q 退出程序

r 重新安排一个进程的优先级别

S 切换到累计模式

s 改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成m s。输入0值则系统将不断刷新，默认值是5 s

f或者F 从当前显示中添加或者删除项目

o或者O 改变显示项目的顺序

l 切换显示平均负载和启动时间信息

m 切换显示内存信息

t 切换显示进程和CPU状态信息

c 切换显示命令名称和完整命令行

M 根据驻留内存大小进行排序

P 根据CPU使用百分比大小进行排序

T 根据时间/累计时间进行排序

W 将当前设置写入~/.toprc文件中



#### 1.6.2 free

​	free命令可以显示Linux系统中空闲的、已用的物理内存及swap内存,及被内核使用的buffer。在Linux系统监控的工具中，free命令是最经常使用的命令之一。 

> 1、命令格式

```
free [参数]
```

> 2、命令功能

​	free 命令显示系统使用和空闲的内存情况，包括物理内存、交互区内存(swap)和内核缓冲区内存。共享内存将被忽略 。

> 3、命令参数

| 参数         | 描述                           |
| ------------ | ------------------------------ |
| -b           | 以Byte为单位显示内存使用情况。 |
| -k           | 以KB为单位显示内存使用情况。   |
| -m           | 以MB为单位显示内存使用情况。   |
| -g           | 以GB为单位显示内存使用情况。   |
| -o           | 不显示缓冲区调节列。           |
| -s<间隔秒数> | 持续观察内存使用状况。         |
| -t           | 显示内存总和列。               |
| -V           | 显示版本信息。                 |

> 4、使用例子

1）、显示内存使用情况

```sh
[root@localhost /]# free
              total        used        free      shared  buff/cache   available
Mem:        1863248      281740     1136996        9264      444512     1321084
Swap:       4095996           0     4095996
[root@localhost /]# free -g
              total        used        free      shared  buff/cache   available
Mem:              1           0           1           0           0           1
Swap:             3           0           3
[root@localhost /]# free -m
              total        used        free      shared  buff/cache   available
Mem:           1819         274        1110           9         434        1290
Swap:          3999           0        3999
```

```tiki wiki
total: 总计物理内存的大小。

used: 已使用多大。

free: 可用有多少。

Shared: 多个进程共享的内存总额。

Buffers/cached: 磁盘缓存的大小。
```

**buffers和cached缓存区别:**

​	为了提高磁盘存取效率, Linux做了一些精心的设计, 除了对dentry进行缓存(用于VFS,加速文件路径名到inode的转换), 还采取了两种主要Cache方式：Buffer Cache和Page Cache。前者针对磁盘块的读写，后者针对文件inode的读写。这些Cache有效缩短了 I/O系统调用(比如read,write,getdents)的时间。

​	磁盘的操作有逻辑级（文件系统）和物理级（磁盘块），这两种Cache就是分别缓存逻辑和物理级数据的。

​	Page cache实际上是针对文件系统的，是文件的缓存，在文件层面上的数据会缓存到page cache。文件的逻辑层需要映射到实际的物理磁盘，这种映射关系由文件系统来完成。当page cache的数据需要刷新时，page cache中的数据交给buffer cache，因为Buffer Cache就是缓存磁盘块的。但是这种处理在2.6版本的内核之后就变的很简单了，没有真正意义上的cache操作。

​	Buffer cache是针对磁盘块的缓存，也就是在没有文件系统的情况下，直接对磁盘进行操作的数据会缓存到buffer cache中，例如，文件系统的元数据都会缓存到buffer cache中。

​	简单说来，page cache用来缓存文件数据，buffer cache用来缓存磁盘数据。在有文件系统的情况下，对文件操作，那么数据会缓存到page cache，如果直接采用dd等工具对磁盘进行读写，那么数据会缓存到buffer cache。

​	所以我们看linux,只要不用swap的交换空间,就不用担心自己的内存太少.如果常常swap用很多,可能你就要考虑加物理内存了.这也是linux看内存是否够用的标准.

​	如果是应用服务器的话，一般只看第二行，+buffers/cache,即对应用程序来说free的内存太少了，也是该考虑优化程序或加内存了。



#### 1.6.3 vmstat

​	vmstat是Virtual Meomory Statistics（虚拟内存统计）的缩写，可对操作系统的虚拟内存、进程、CPU活动进行监控。他是对系统的整体情况进行统计，不足之处是无法对某个进程进行深入分析。vmstat 工具提供了一种低开销的系统性能观察方式。因为 vmstat 本身就是低开销工具，在非常高负荷的服务器上，你需要查看并监控系统的健康情况,在控制窗口还是能够使用vmstat 输出结果。 

**物理内存和虚拟内存区别：**

​	我们知道，直接从物理内存读写数据要比从硬盘读写数据要快的多，因此，我们希望所有数据的读取和写入都在内存完成，而内存是有限的，这样就引出了物理内存与虚拟内存的概念。

​	物理内存就是系统硬件提供的内存大小，是真正的内存，相对于物理内存，在linux下还有一个虚拟内存的概念，虚拟内存就是为了满足物理内存的不足而提出的策略，它是利用磁盘空间虚拟出的一块逻辑内存，用作虚拟内存的磁盘空间被称为交换空间（Swap Space）。

​	作为物理内存的扩展，linux会在物理内存不足时，使用交换分区的虚拟内存，更详细的说，就是内核会将暂时不用的内存块信息写到交换空间，这样以来，物理内存得到了释放，这块内存就可以用于其它目的，当需要用到原始的内容时，这些信息会被重新从交换空间读入物理内存。

​	linux的内存管理采取的是分页存取机制，为了保证物理内存能得到充分的利用，内核会在适当的时候将物理内存中不经常使用的数据块自动交换到虚拟内存中，而将经常使用的信息保留到物理内存。

​	要深入了解linux内存运行机制，需要知道下面提到的几个方面：

​	首先，Linux系统会不时的进行页面交换操作，以保持尽可能多的空闲物理内存，即使并没有什么事情需要内存，Linux也会交换出暂时不用的内存页面。这可以避免等待交换所需的时间。

​	其次，linux进行页面交换是有条件的，不是所有页面在不用时都交换到虚拟内存，linux内核根据”最近最经常使用“算法，仅仅将一些不经常使用的页面文件交换到虚拟内存，有时我们会看到这么一个现象：linux物理内存还有很多，但是交换空间也使用了很多。其实，这并不奇怪，例如，一个占用很大内存的进程运行时，需要耗费很多内存资源，此时就会有一些不常用页面文件被交换到虚拟内存中，但后来这个占用很多内存资源的进程结束并释放了很多内存时，刚才被交换出去的页面文件并不会自动的交换进物理内存，除非有这个必要，那么此刻系统物理内存就会空闲很多，同时交换空间也在被使用，就出现了刚才所说的现象了。关于这点，不用担心什么，只要知道是怎么一回事就可以了。

​	最后，交换空间的页面在使用时会首先被交换到物理内存，如果此时没有足够的物理内存来容纳这些页面，它们又会被马上交换出去，如此以来，虚拟内存中可能没有足够空间来存储这些交换页面，最终会导致linux出现假死机、服务异常等问题，linux虽然可以在一段时间内自行恢复，但是恢复后的系统已经基本不可用了。

因此，合理规划和设计linux内存的使用，是非常重要的。

**虚拟内存原理：**

​	在系统中运行的每个进程都需要使用到内存，但不是每个进程都需要每时每刻使用系统分配的内存空间。当系统运行所需内存超过实际的物理内存，内核会释放某些进程所占用但未使用的部分或所有物理内存，将这部分资料存储在磁盘上直到进程下一次调用，并将释放出的内存提供给有需要的进程使用。

​	在Linux内存管理中，主要是通过“调页Paging”和“交换Swapping”来完成上述的内存调度。调页算法是将内存中最近不常使用的页面换到磁盘上，把活动页面保留在内存中供进程使用。交换技术是将整个进程，而不是部分页面，全部交换到磁盘上。

​	分页(Page)写入磁盘的过程被称作Page-Out，分页(Page)从磁盘重新回到内存的过程被称作Page-In。当内核需要一个分页时，但发现此分页不在物理内存中(因为已经被Page-Out了)，此时就发生了分页错误（Page Fault）。

​	当系统内核发现可运行内存变少时，就会通过Page-Out来释放一部分物理内存。经管Page-Out不是经常发生，但是如果Page-out频繁不断的发生，直到当内核管理分页的时间超过运行程式的时间时，系统效能会急剧下降。这时的系统已经运行非常慢或进入暂停状态，这种状态亦被称作thrashing(颠簸)。



> 1、命令格式

```
vmstat [-a] [-n] [-S unit] [delay [ count]]
vmstat [-s] [-n] [-S unit]
vmstat [-m] [-n] [delay [ count]]
vmstat [-d] [-n] [delay [ count]]
vmstat [-p disk partition] [-n] [delay [ count]]
vmstat [-f]
vmstat [-V]
```

> 2、命令功能

​	用来显示虚拟内存的信息 

> 3、命令参数

-a：显示活跃和非活跃内存

-f：显示从系统启动至今的fork数量 。

-m：显示slabinfo

-n：只在开始时显示一次各字段名称。

-s：显示内存相关统计信息及多种系统活动数量。

delay：刷新时间间隔。如果不指定，只显示一条结果。

count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。

-d：显示磁盘相关统计信息。

-p：显示指定磁盘分区统计信息

-S：使用指定单位显示。参数有 k 、K 、m 、M ，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）

-V：显示vmstat版本信息。



> 4、使用例子

1）、显示虚拟内存使用情况 

```sh
[root@localhost /]# vmstat 
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 1135932    932 443564    0    0     6     2   22   32  0  0 100  0  0
```

字段说明：

Procs（进程）：

​	r: 运行队列中进程数量

​	b: 等待IO的进程数量

Memory（内存）：

​	swpd: 使用虚拟内存大小

​	free: 可用内存大小

​	buff: 用作缓冲的内存大小

​	cache: 用作缓存的内存大小

Swap：

​	si: 每秒从交换区写到内存的大小

​	so: 每秒写入交换区的内存大小

​	IO：（现在的Linux版本块的大小为1024bytes）

​	bi: 每秒读取的块数

​	bo: 每秒写入的块数

system：

​	in: 每秒中断数，包括时钟中断。

​	cs: 每秒上下文切换数。

​	CPU（以百分比表示）：

​	us: 用户进程执行时间(user time)

​	sy: 系统进程执行时间(system time)

​	id: 空闲时间(包括IO等待时间),中央处理器的空闲时间 。以百分比表示。

​	wa: 等待IO时间

备注： 如果 r经常大于 4 ，且id经常少于40，表示cpu的负荷很重。如果pi，po 长期不等于0，表示内存不足。如果disk 经常不等于0， 且在 b中的队列 大于3， 表示 io性能不好。Linux在具有高稳定性、可靠性的同时，具有很好的可伸缩性和扩展性，能够针对不同的应用和硬件环境调整，优化出满足当前应用需要的最佳性能。因此企业在维护Linux系统、进行系统调优时，了解系统性能分析工具是至关重要的。



2）、显示活跃和非活跃内存 

```sh
[root@localhost /]# vmstat -a 2 5
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free  inact active   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 1136056 180648 197052    0    0     6     2   22   32  0  0 100  0  0
 0  0      0 1136032 180648 197120    0    0     0     0   34   42  0  0 100  0  0
 0  0      0 1136032 180648 197132    0    0     0     0   35   47  0  0 100  0  0
 0  0      0 1136032 180648 197132    0    0     0     0   31   43  0  0 100  0  0
 0  0      0 1135908 180648 197132    0    0     0     0   35   47  0  0 100  0  0
```

​	使用-a选项显示活跃和非活跃内存时，所显示的内容除增加inact和active外，其他显示内容与例子1相同。

字段说明：

​	Memory（内存）：

​	inact: 非活跃内存大小（当使用-a选项时显示）

​	active: 活跃的内存大小（当使用-a选项时显示）



3）、查看系统已经fork了多少次 

```sh
[root@localhost /]# vmstat -f
        30332 forks
```

这个数据是从/proc/stat中的processes字段里取得的 。

4）、查看内存使用的详细信息 

```sh
[root@localhost /]# vmstat -s
      1863248 K total memory
       282820 K used memory
       197072 K active memory
       180624 K inactive memory
      1135900 K free memory
          932 K buffer memory
       443596 K swap cache
      4095996 K total swap
            0 K used swap
      4095996 K free swap
         2422 non-nice user cpu ticks
           13 nice user cpu ticks
         7383 system cpu ticks
      4974917 idle cpu ticks
         3827 IO-wait cpu ticks
            0 IRQ cpu ticks
          154 softirq cpu ticks
            0 stolen cpu ticks
       302798 pages paged in
        76875 pages paged out
            0 pages swapped in
            0 pages swapped out
      1108375 interrupts
      1586434 CPU context switches
   1558227212 boot time
        30368 forks
```

这些信息的分别来自于/proc/meminfo,/proc/stat和/proc/vmstat。 

5）、查看磁盘的读/写 

```sh
[root@localhost /]# vmstat -d
disk- ------------reads------------ ------------writes----------- -----IO------
       total merged sectors      ms  total merged sectors      ms    cur    sec
sr0        0      0       0       0      0      0       0       0      0      0
sda    13790     60  605597  150050   5718    849  153899  188420      0     82
```

这些信息主要来自于/proc/diskstats.

merged:表示一次来自于合并的写/读请求,一般系统会把多个连接/邻近的读/写请求合并到一起来操作.

6）、查看/dev/sda1磁盘的读/写 

```sh
[root@localhost /]# df
文件系统          1K-块    已用     可用 已用% 挂载点
/dev/sda3      15815680 4171580 11644100   27% /
devtmpfs         915664       0   915664    0% /dev
tmpfs            931624       0   931624    0% /dev/shm
tmpfs            931624    9260   922364    1% /run
tmpfs            931624       0   931624    0% /sys/fs/cgroup
/dev/sda1       1038336  189196   849140   19% /boot
tmpfs            186328       0   186328    0% /run/user/0
[root@localhost /]# vmstat -p /dev/sda3
sda3          reads   read sectors  writes    requested writes
               12874     552308       5679     123698
```

这些信息主要来自于/proc/diskstats。

​	reads:来自于这个分区的读的次数。

​	read sectors:来自于这个分区的读扇区的次数。

​	writes:来自于这个分区的写的次数。

​	requested writes:来自于这个分区的写请求次数。



7）、查看系统的slab信息 

```sh
[root@localhost /]# vmstat -m
Cache                       Num  Total   Size  Pages
nf_conntrack_ffffffff921116c0    102    102    320     51
rpc_inode_cache              51     51    640     51
kcopyd_job                    0      0   3312      9
dm_uevent                     0      0   2608     12
dm_rq_target_io               0      0    136     60
xfs_dqtrx                     0      0    528     62
xfs_dquot                     0      0    488     67
xfs_ili                    8208   8208    168     48
xfs_inode                 99960  99960    960     34
xfs_efd_item                117    117    416     39
xfs_log_ticket               88     88    184     44
bio-2                       102    102    320     51
ip6_dst_cache               108    108    448     36
RAWv6                       262    416   1216     26
UDPLITEv6                     0      0   1216     26
UDPv6                        52     52   1216     26
tw_sock_TCPv6                 0      0    256     64
TCPv6                        30     30   2112     15
cfq_queue                     0      0    232     70
bsg_cmd                       0      0    312     52
mqueue_inode_cache           36     36    896     36
hugetlbfs_inode_cache       106    106    608     53
configfs_dir_cache            0      0     88     46
dquot                         0      0    256     64
kioctx                       56     56    576     56
userfaultfd_ctx_cache         0      0    192     42
pid_namespace                 0      0   2200     14
posix_timers_cache          132    132    248     66
UDP-Lite                      0      0   1088     30
flow_cache                    0      0    144     56
UDP                          60     60   1088     30
tw_sock_TCP                   0      0    256     64
TCP                          32     32   1984     16
Cache                       Num  Total   Size  Pages
dax_cache                    42     42    768     42
blkdev_queue                 26     26   2456     13
blkdev_ioc                   78     78    104     39
user_namespace              136    136    480     68
dmaengine-unmap-128          30     30   1088     30
sock_inode_cache            969   1224    640     51
fsnotify_mark_connector  135660 135660     24    170
net_namespace                12     12   5248      6
shmem_inode_cache          1056   1056    680     48
Acpi-State                  204    204     80     51
task_delay_info             432    432    112     36
taskstats                    98     98    328     49
proc_inode_cache           2998   3136    656     49
sigqueue                    102    102    160     51
bdev_cache                   78     78    832     39
kernfs_node_cache         36516  36516    120     68
mnt_cache                  6636   6636    384     42
inode_cache               21285  21285    592     55
dentry                   135492 135492    192     42
iint_cache                    0      0    128     64
avc_xperms_node             146    146     56     73
avc_node                   6757   7504     72     56
selinux_inode_security   130662 130662     40    102
buffer_head                7605   7605    104     39
vm_area_struct             7733   7733    216     37
mm_struct                   100    100   1600     20
files_cache                 255    255    640     51
signal_cache                300    392   1152     28
sighand_cache               291    315   2112     15
task_xstate                 429    429    832     39
task_struct                 311    343   4160      7
anon_vma                   3271   3519     80     51
shared_policy_node         2805   2805     48     85
Cache                       Num  Total   Size  Pages
numa_policy                 186    186    264     62
radix_tree_node            5880   5880    584     56
idr_layer_cache             225    225   2112     15
dma-kmalloc-8192              0      0   8192      4
dma-kmalloc-4096              0      0   4096      8
dma-kmalloc-2048              0      0   2048     16
dma-kmalloc-1024              0      0   1024     32
dma-kmalloc-512              64     64    512     64
dma-kmalloc-256               0      0    256     64
dma-kmalloc-128               0      0    128     64
dma-kmalloc-64                0      0     64     64
dma-kmalloc-32                0      0     32    128
dma-kmalloc-16                0      0     16    256
dma-kmalloc-8                 0      0      8    512
dma-kmalloc-192               0      0    192     42
dma-kmalloc-96                0      0     96     42
kmalloc-8192                 56     60   8192      4
kmalloc-4096                239    264   4096      8
kmalloc-2048               1070   1168   2048     16
kmalloc-1024               3160   3424   1024     32
kmalloc-512                7225   8384    512     64
kmalloc-256                9928  10112    256     64
kmalloc-192                9392   9576    192     42
kmalloc-128                2898   3328    128     64
kmalloc-96                 6468   6468     96     42
kmalloc-64               252672 252672     64     64
kmalloc-32               129920 129920     32    128
kmalloc-16                67840  67840     16    256
kmalloc-8                 88576  88576      8    512
kmem_cache_node             192    192     64     64
kmem_cache                  192    192    256     64
```

这组信息来自于/proc/slabinfo。

​	slab:由于内核会有许多小对象，这些对象构造销毁十分频繁，比如i-node，dentry，这些对象如果每次构建的时候就向内存要一个页(4kb)，而其实只有几个字节，这样就会非常浪费，为了解决这个问题，就引入了一种新的机制来处理在同一个页框中如何分配小存储区，而slab可以对小对象进行分配,这样就不用为每一个对象分配页框，从而节省了空间，内核对一些小对象创建析构很频繁，slab对这些小对象进行缓冲,可以重复利用,减少内存分配次数。



#### 1.6.4 iostat

​	iostat是I/O statistics（输入/输出统计）的缩写，iostat工具将对系统的磁盘操作活动进行监视。它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。同vmstat一样，iostat也有一个弱点，就是它不能对某个进程进行深入分析，仅对系统的整体情况进行分析。iostat属于sysstat软件包。可以用yum install sysstat 直接安装。 

> 1、命令格式

```
iostat [参数] [时间] [次数]
```

> 2、命令功能

​	通过iostat方便查看CPU、网卡、tty设备、磁盘、CD-ROM 等等设备的活动情况,	负载信息。 

> 3、命令参数

​	-c 显示CPU使用情况

​	-d 显示磁盘使用情况

​	-k 以 KB 为单位显示

​	-m 以 M 为单位显示

​	-N 显示磁盘阵列(LVM) 信息

​	-n 显示NFS 使用情况

​	-p[磁盘] 显示磁盘和分区的情况

​	-t 显示终端和CPU的信息

​	-x 显示详细信息

​	-V 显示版本信息

> 4、使用例子

1）、显示所有设备负载情况 

2）、定时显示所有信息 

3）、显示指定磁盘信息 

4）、显示tty和Cpu信息 

5）、以M为单位显示所有信息 

6）、查看TPS和吞吐量信息 

7）、查看设备使用率（%util）、响应时间（await） 

8）、查看cpu状态 

