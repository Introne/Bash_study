## bash基本命令解释（1-apt）

### 学习第一弹：一帆风顺
```
#!/usr/bin/env bash  
```
设置Bash解释器运行环境

### 学习第二弹：两全其美
```
echo "====> Install softwares via apt-get <===="
```
输出""中的内容：提示以下的命令是通过apt-get安装一些app

### 学习第三弹：三神俱喜
```
echo "==> Disabling the release upgrader"
sudo sed -i.bak 's/^Prompt=.*$/Prompt=never/' /etc/update-manager/release-upgrades
```

设置ubuntu的系统升级Prompt=never

将etc下配置文件release-upgrades中Prompt=以外的字符串替换为Prompt=never，并保存

同时会以release-upgrades.bak文件备份原来未修改文件内容，以确保原始文件内容安全性，防止错误操作而无法恢复原来内容。

#### 背景
sudo是linux系统管理指令，是允许系统管理员让普通用户执行一些或者全部的root命令的一个工具.

sed：Stream Editor文本流编辑，sed命令是把文件一行行的读到内存中当成一行处理,是一个“非交互式的”面向字符流的编辑器,它也被称为流编辑器。跟车间中的流水线一样，sed能一行一行的逐个处理，获取到需要的内容后显示到屏幕上。sed能配合正则表达式使用。

sed命令的语法格式：sed [选项] [指令] [输入文件]

选项：
-n 取消默认输出
-r 支持正则表达式
-p 打印
-e 多项编辑
-i.bak  修改后备份 
s 搜索一次
sg 搜索全局
\#\#\#： s#替换前#替换后#g
/ / / : 与###一样

指令：
's/代表替换：注's/A/B' 即将文件内所有A替换为B

### 学习第四弹：四季发财
```bash
echo "==> Switch to an adjacent mirror"
# https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu
cat <<EOF > list.tmp
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

## Not recommended
# deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse

EOF
```
/etc/apt/sources.list是一个普通可编辑的文本文件，保存了ubuntu软件更新的源服务器的地址。

该命令行通过生成 /etc/apt/sources.list临时文件，结合下一个条件判断命令行，将 /etc/apt/sources.list 文件中 Ubuntu 默认的源地址 http://archive.ubuntu.com/ 替换为 http://mirrors.ustc.edu.cn/ 。

##### why替换地址？
ubuntu 图形安装器会根据用户设定的时区推断 locale，这导致默认的源地址通常不是 http://archive.ubuntu.com/， 而是 http://<country-code>.archive.ubuntu.com/ubuntu/ 。因此ubuntu默认的软件更新源也是国外的，速度超级慢，用"apt install"安装软件时各种网络问题也是层出不穷。这时我们可以为 apt-get 设置成国内镜像代理。


修改源之前需要先查看版本名，ubuntu20.04对应的是“focal”，也就是以上镜像源中展示的。“eoan”代表ubuntu19.10，“xenial”代表ubuntu16.04，“bionic”代表ubuntu18.04，“disco”代表ubuntu19.04。
用以下命令查看版本名：
```bash
lsb_release -c
```

##### 背景
###### cat <<EOF 
使用cat <<EOF时，我们输入完成后，需要在一个新的一行输入EOF结束stdin的输入。EOF必须顶行写，前面不能用制表符或者空格。
<<-可以避免EOF误被stdin的情况。

###### 重定向 
\> 重定向，覆盖原内容
\>> 重定向，在原内容后续上

###### deb和deb-src
deb 或是 deb-src 表明了所获取的软件包档案类型。
deb：档案类型为二进制预编译软件包，一般我们所用的档案类型。相对于apt
deb-src：档案类型为用于编译二进制软件包的源代码。相对于源代码包（apt-get source $package）

###### https防劫持
使用 HTTPS 可以有效避免国内运营商的缓存劫持。

##### 镜像
镜像，原意是光学里指的物体在镜面中所成之像。引用到电脑网络上，一个网站的镜像是指对一个网站内容的拷贝。镜像通常用于为相同信息内容提供不同的源，特别是在下载量大的时候提供了一种可靠的网络连接。制作镜像是一种文件同步的过程。
有了镜像网站的好处是：如果不能对主站作正常访问（如某个服务器死掉或出了意外），但仍能通过其它服务器正常浏览。
镜像可以提高用户在某个地区的下载速度。譬如一个美国网站的中国镜像可以使来自中国的用户直接从这个中国的镜像访问，从而加快了速度。这可以看作是一种全球范围的缓存。

### 学习第五弹：五星高照 
```bash
if [ ! -e /etc/apt/sources.list.bak ]; then
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
fi
sudo mv list.tmp /etc/apt/sources.list
```
在替换国内镜像软件源之前，备份原来的软件源并另存。
由一个条件判断语句执行此功能：如果list.bak备份文件不存在，就通过复制进行备份，然后将上一个命令中生成的带有国内镜像源的文件覆盖sources.list文件。
#### 背景：
##### 文件比较运算符
-e filename：如果 filename存在，则为真  [ -e /var/log/syslog ]
-d filename：如果 filename为目录，则为真  [ -d /tmp/mydir ]
-f filename：如果 filename为常规文件，则为真  [ -f /usr/bin/grep ]
-L filename：如果 filename为符号链接，则为真  [ -L /usr/bin/grep ]
-r filename：如果 filename可读，则为真  [ -r /var/log/syslog ]
-w filename：如果 filename可写，则为真  [ -w /var/mytmp.txt ]
-x filename：如果 filename可执行，则为真  [ -L /usr/bin/grep ]

##### 字符串比较运算符 （请注意引号的使用，这是防止空格扰乱代码的好方法）
-z string 如果 string长度为零，则为真  [ -z “$myvar” ]
-n string 如果 string长度非零，则为真  [ -n “$myvar” ]
 
##### 算术比较运算符
num1-eq num2 等于 [ 3 -eq $mynum ]
 num1-ne num2 不等于 [ 3 -ne $mynum ]
 num1-lt num2 小于 [ 3 -lt $mynum ]
 num1-le num2 小于或等于 [ 3 -le $mynum ]
 num1-gt num2 大于 [ 3 -gt $mynum ]
 num1-ge num2 大于或等于 [ 3 -ge $mynum ]

 ##### 逻辑符
条件表达式的相反： 逻辑非 !  
条件表达式的并列：逻辑与 –a                   
条件表达式的或：逻辑或 -o                   


### 学习第六弹：六六大顺 
```bash
\# Virtual machines needn't this and I want life easier.
\# https://help.ubuntu.com/lts/serverguide/apparmor.html
if [ "$(whoami)" == 'vagrant' ]; then
    echo "==> Disable AppArmor"
    sudo service apparmor stop
    sudo update-rc.d -f apparmor remove
fi
```
AppArmor限制太多，关掉它！
使用sudo service apparmor stop禁用Apparmor，并使用sudo update-rc.d -f apparmor defaults删除内核模块

#### 背景
##### Whoami
whoami命令用于显示当前系统的有效用户名称。
登陆到虚拟机里面，默认的用户叫做 vagrant，可以用通过 whoami 命令查看
##### Vagrant
Vagrant 通过一个简单的配置文件为各种虚拟化提供程序提供了一个抽象层，可让开发人员和操作人员快速启动运行 Linux 或任何其他操作系统的虚拟机 (VM)。   
通过Vagrant创建虚机需要先导入镜像文件，也就是box，它们默认存储的位置在用户目录下的 .vagrant.d 目录下
##### $()
$() 执行命令代换, 即取所包含的命令的输出作为文本值参与运行, 文本值甚至可以是命令, 如直接运行$(echo pwd) 则相当于直接运行pwd.
##### AppArmor
AppArmor是一款与SeLinux类似的安全框架/工具，其主要作用是控制应用程序的各种权限，例如对某个目录/文件的读/写，对网络端口的打开/读/写等等。
##### sudo服务（借权服务）
作用：将特定命令的执行权限赋予指定用户，以保证普通用户可以完成原本root管理员才能完成的任务。
配置原则：在保证普通用户完成相应工作的前提下，尽可能少地赋予额外的权限
功能：
    限制用户执行指定的命令
    记录用户执行的每一条命令
    验证后的5分钟内无需再次验证
##### service 命令
service命令是用来控制系统服务的实用工具，它以启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态。
语法： service < option > | --status-all | [ service_name [ command | –full-restart ] ]
option的值如下：
-h：显示 service 的帮助信息
-status：显示所服务的状态
--status-all：查看所有服务的状态
service_name：服务名，即 /etc/init.d 目录下的脚本文件名
command：系统服务脚本支持的控制命令，如：start、stop 和 restart
--full-restart：重启所有服务
##### update-rc.d命令
update-rc.d命令用来更新系统启动项的脚本。这些启动项脚本的链接位于/etc/rcN.d/目录（N代表0～6），对应脚本位于/etc/init.d/目录。
Ubuntu中的运行级别:
    0（关闭系统）
    1（单用户模式，只允许root用户对系统进行维护。）
    2 到 5（多用户模式，其中3为字符界面，5为图形界面。）
    6（重启系统）

删除一个服务: sudo update-rc.d ServiceName remove

### 学习第七弹：七星高照 
```bash
echo "==> Disable whoopsie"
sudo sed -i 's/report_crashes=true/report_crashes=false/' /etc/default/whoopsie
sudo service whoopsie stop
```
禁用whoopsie
将etc下whoopsie配置文件中的report_crashes=true参数更改为report_crashes=false，并保存；然后利用service命令禁用whoopsie服务。

#### 背景
##### 什么是whoopsie？
Whoopsie 是Ubuntu 错误跟踪器的一部分。它获取应用程序失败时apport创建和呈现的崩溃报告，并将它们发送到 Canonical 服务器进行进一步处理。
Apport是一个错误收集系统，会收集软件崩溃、未处理异常和其他，包括程序bug，并为调试目的生成崩溃报告。 当一个应用程序崩溃或者出现Bug时候，Apport就会通过弹窗警告用户并且询问用户是否提交崩溃报告。

### 学习第八弹：八面春风 
```bash
echo "==> Install linuxbrew dependences"
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y install build-essential curl file git
sudo apt-get -y install libbz2-dev zlib1g-dev libzstd-dev
# sudo apt-get -y install libcurl4-openssl-dev libexpat-dev libncurses-dev
```

进行软件更新，并安装linuxbrew依赖项
要设置Brew，我们需要在系统上安装一些依赖项，如GCC、Glibc 和 64 位 x86_64 CPU等

#### 背景
##### why need linuxbrew
普通用户想安装应用往往比较麻烦，他们没有写入 /etc/,/bin/,/sbin/ 等重要目录的权限，只能在configure时通过 --prefix=$HOME 来将应用安装在HOME目录下。
linuxbrew是著名MacOS包管理器homebrew的linux版，它可以很方便地安装应用到HOME目录下。 

##### 关于APT，apt-和apt
APT(Advanced Packaging Tool)：Debian 系 Linux 特有的软件包管理体系，相较于先前的从源码开始编译软件，极大地方便了在 Linux 下对软件进行管理
apt-：以 apt-get 和 apt-cache 为代表，它们是 Debian 软件包管理体系中各种功能对应的命令行工具
apt：在原有 apt-get 和 apt-cache 基础上，对基础软件包管理操作进行简化和优化，专为新手终端用户设计的命令行工具

###### apt-详解
同步远程仓库中的记录表：sudo apt-get update
将本地所有软件包更新至远程仓库最新版本：sudo apt-get upgrade
在软件仓库中搜索某一软件包：apt-cache search <package>
查看软件包具体信息：apt-cache show <package>
安装软件包：sudo apt-get install <package>
卸载软件包：sudo apt-get remove <package>


##### sudo apt-get update 与upgrade
在windows下安装软件，我们只需要有.exe文件，然后双击，直接安装就可以了。但Linux下，不是这样的。每个Linux的发行版，比如Ubuntu，都会维护一个自己的软件仓库，我们常用的几乎所有软件都在这里面。这里面的软件绝对安全，而且绝对的能正常安装。
那我们要怎么安装呢？在Ubuntu下，我们维护一个源列表，源列表里面都是一些网址信息，这每一条网址就是一个源，这个地址指向的数据标识着这台源服务器上有哪些软件可以安装使用。
编辑源命令：
```bash
sudo gedit /etc/apt/sources.list
```

在这个文件里加入或者注释（加#）掉一些源后，保存。这时候，我们的源列表里指向的软件就会增加或减少一部分。
接一下要做的就是：
```bash
sudo apt-get update
```
这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。我们在新立得软件包管理器里看到的软件列表，都是通过update命令更新的。

update后，可能需要upgrade一下。
```bash
sudo apt-get upgrade
```
这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。如果你的软件都是最新版本，会提示：
升级了 0 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。

总而言之，update是更新软件列表，upgrade是更新软件。

##### apt-get -y 中的-y
apt-get -y中的-y是同意的意思。指跳过系统提示，直接运行指令。

##### build-essential软件包
Ubuntu缺省情况下，并没有提供C/C++的编译环境，因此还需要手动安装。但是如果单独安装gcc以及g++比较麻烦，幸运的是，Ubuntu提供了一个build-essential软件包。查看该软件包的依赖关系：
```bash
snwang@DESKTOP-T89G9DA:~$ apt-cache depends build-essential
build-essential
 |Depends: libc6-dev
  Depends: <libc-dev>
    libc6-dev
  Depends: gcc
  Depends: g++
  Depends: make
    make-guile
  Depends: dpkg-dev
```
也就是说，安装了该软件包，编译c/c++所需要的软件包也都会被安装。因此如果想在Ubuntu中编译c/c++程序,只需要安装该软件包就可以了。

##### curl
curl是一种数据传输程序，使用其中一种支持的协议（DICT，FILE，FTP，FTPS，GOPHER，HTTP，HTTPS，IMAP，IMAPS，LDAP，LDAPS，POP3，POP3S，RTMP，RTSP，SCP，SFTP，SMTP，SMTPS，TELNET和TFTP）从服务器或服务器传输数据的工具。该命令旨在无需用户交互即可工作。

### 学习第九弹：九久康泰 
```bash
echo "==> Install other software"
sudo apt-get -y install aptitude parallel vim screen xsltproc numactl
```
安装一些其他的工具


```bash
echo "==> Install develop libraries"
# sudo apt-get -y install libreadline-dev libedit-dev
sudo apt-get -y install libdb-dev libxml2-dev libssl-dev libncurses5-dev # libgd-dev
# sudo apt-get -y install gdal-bin gdal-data libgdal-dev # /usr/lib/libgdal.so: undefined reference to `TIFFReadDirectory@LIBTIFF_4.0'
# sudo apt-get -y install libgsl0ldbl libgsl0-dev

# Gtk stuff, Need by alignDB
# install them in a fresh machine to avoid problems
```
安装一些开发库

```bash
echo "==> Install gtk3"
sudo apt-get -y install libcairo2-dev libglib2.0-0 libglib2.0-dev libgtk-3-dev libgirepository1.0-dev
sudo apt-get -y install gir1.2-glib-2.0 gir1.2-gtk-3.0 gir1.2-webkit-3.0

echo "==> Install gtk3 related tools"
# sudo apt-get -y install xvfb glade
```
安装gtk3
gtk(gimp toolkit)是GIMP 开发过程中用来管理图型界面的一套工具程序库，是完全使用C语言开发的。



```bash
echo "==> Install graphics tools"
sudo apt-get -y install gnuplot graphviz imagemagick
#echo "==> Install nautilus plugins"
#sudo apt-get -y install nautilus-open-terminal nautilus-actions

# Mysql will be installed separately.
# Remove system provided mysql package to avoid confusing linuxbrew.
```
安装一些画图工具：gnuplot graphviz imagemagick；。

#### 背景
##### Mysql
SQL（structured query language）是一种标准的数据库语言。包含以下3个功能：
数据创建语句，能够帮助定义数据库和对象，例如表，视图，触发器，存储过程；
数据操纵语言，能够更新数据，查询数据；
数据控制语言，帮你管理数据权限。

MySQL由 My 和 SQL组成

MySQL是数据库管理系统，能够帮助我们管理关系型数据库，并且是开源的，意味着这是免费的，如果必要，我们可以修改源代码。


```bash
echo "==> Remove system provided mysql"
sudo apt-get -y purge mysql-common
```
#### 背景
##### apt-get purge 
apt-get remove 会删除软件包而保留软件的配置文件
apt-get purge 会同时清除软件包和软件的配置文件，即彻底删除软件包。

echo "==> Restore original sources.list"
if [ -e /etc/apt/sources.list.bak ]; then
    sudo rm /etc/apt/sources.list
    sudo mv /etc/apt/sources.list.bak /etc/apt/sources.list
fi
恢复原始的sources.list：用上述的sources.list.bak替代原有的sources.list

### 学习第十弹：十全十美 
```bash
echo "====> Basic software installation complete! <===="
```
Basic software 暂且备好啦，诸事大吉！！！
