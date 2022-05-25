# 新的学习又开始啦

## 设置Bash解释器运行环境
```Bash
#!/usr/bin/env bash  
```


```
echo "====> Download Genomics related tools <===="

mkdir -p $HOME/bin
mkdir -p $HOME/.local/bin
mkdir -p $HOME/share
mkdir -p $HOME/Scripts 
```
在家目录下创建一些文件夹

## 添加环境变量--$HOME_PATH
```Bash
# make sure $HOME/bin in your $PATH
if grep -q -i homebin $HOME/.bashrc; then
    echo "==> .bashrc already contains homebin"
else
    echo "==> Update .bashrc"

    HOME_PATH='export PATH="$HOME/bin:$HOME/.local/bin:$PATH"'
    echo '# Homebin' >> $HOME/.bashrc
    echo $HOME_PATH >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    eval $HOME_PATH
fi 
```


### 背景
#### grep语句
Linux grep 命令用于查找文件里符合条件的字符串。grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。
若不指定任何文件名称，或是所给予的文件名为-，则 grep 指令会从标准输入设备读取数据。

grep来自于英文词组“global search regular expression and print out the line”的缩写，意思是用于全面搜索的正则表达式，并将结果输出。
人们通常会将grep命令与正则表达式搭配使用，参数作为搜索过程中的补充或对输出结果的筛选，命令模式十分灵活。

相关参数：
-i 或 --ignore-case : 忽略字符大小写的差别。
-q 或 --quiet或--silent : 不显示任何信息。

#### 环境变量
什么是环境变量？
简单地说，环境变量就是当前环境下的参数或者变量。专业的说，指在操作系统中用来指定操作系统的一些参数。如最常见的环境变量 —— PATH，PATH 是指系统会去哪些目录中寻找可执行的程序的环境变量。当用户要求系统运行一个程序而没有告诉它程序所在的完整路径时，系统除了在当前目录下寻找此程序外，还要到PATH变量中指定的路径去寻找。  
比如说你想执行一条命令ls。如果不设置这个环境变量呢，除非你知道ls放在/bin下，告诉系统去执行/bin/ls，否则系统会告诉你我不知道ls在哪啊。。。“command not found” 
现在有了$PATH这个变量，系统会优先去这个变量的值里指定的目录去找ls，如果都找不到，才会告诉你“command not found”。

linux中变量调用的时候会在变量名前加一个\$符号。一般作为规范，环境变量是全部大写的。例如，$HOME是一个环境变量


#### 添加PATH环境变量

（1）PATH环境变量的格式

编辑 PATH 声明，其格式为：

　　PATH=$PATH:\<PATH1>:\<PATH2>:\<PATH3>:------:\<PATHn>

　　可以自己加上指定的路径，中间用冒号隔开。环境变量更改后，在用户下次登陆时生效，如果想立刻生效，则可执行下面的语句：$source .bash_profile

（2）添加PATH环境变量

export PATH=路径:$PATH

查看命令：echo $PATH， 可判断是否添加PATH成功。

注：该方法的PATH 在终端关闭后就会消失。所以还是建议通过编辑/etc/profile来改PATH，也可以改 家目录下的.bashrc(即：~/.bashrc)。

#### .bashrc 文件
.bashrc 文件主要保存着个人的一些个性化设置，如：命令别名、环境变量等。

修改.bashrc文件，它可以把使用这些环境变量的权限控制到用户级别，只是针对某一个特定的用户。

而修改/etc/profile 文件，它是针对于所有的用户，使所有用户都有权使用这些环境变量。相比较起来，第一种方法更加安全。

## 仓库克隆
```bash
# Clone or pull other repos
for OP in dotfiles withncbi; do
    if [[ ! -d "$HOME/Scripts/$OP/.git" ]]; then
        if [[ ! -d "$HOME/Scripts/$OP" ]]; then
            echo "==> Clone $OP"
            git clone https://github.com/wang-q/${OP}.git "$HOME/Scripts/$OP"
        else
            echo "==> $OP exists"
        fi
    else
        echo "==> Pull $OP"
        pushd "$HOME/Scripts/$OP" > /dev/null
        git pull
        popd > /dev/null
    fi
done
```
###背景
####  for 循环
标准 Bash for 循环
for 循环遍历项列表并执行给定的命令集。

Bash for 循环采用以下形式：

1. for 变量 in 常量列表; do  \n 一些命令; \\n done  
其中，列表可以是由空格分隔的一系列字符串，一系列数字，命令输出，数组等。

2. for ((变量初始化;条件判断;变量自变)); do  \n 一些命令; \n done;

#### popd > /dev/null
确保 popd 保持静默(不产生任何输出)

popd命令用于删除目录栈中的记录；如果popd目录不加如何参数则会先删除最上面的记录，然后切换到删除过后目录栈中的最上面的目录。

语法
popd (选项）

选项
+N：将第N个目录删除（从左边数起，数字从0开始）

## alignDB
```bash
# alignDB
# chmod +x $HOME/Scripts/alignDB/alignDB.pl
# ln -fs $HOME/Scripts/alignDB/alignDB.pl $HOME/bin/alignDB.pl
```

### 背景

### chmod +x 命令: 给执行权限

chmod命令用于改变linux系统文件或目录的访问权限。用它控制文件或目录的访问权限。该命令有两种用法。一种是包含字母和操作符表达式的文字设定法；另一种是包含数字的数字设定法。

命令格式: chmod [-cfvR] [—help] [—version] mode file 

命令功能用于改变文件或目录的访问权限，用它控制文件或目录的访问权限。
   
命令参数必要参数：
-c 当发生改变时，报告处理信息   
-f 错误信息不输出  
-R 处理指定目录以及其子目录下的所有文件  
-v 运行时显示详细处理信息


选择参数：
--reference=<目录或者文件> 设置成具有指定目录或者文件具有相同的权限  
--version 显示版本信息

<权限范围>+<权限设置> 使权限范围内的目录或者文件具有指定的权限 
<权限范围>-<权限设置> 删除权限范围的目录或者文件的指定权限  
<权限范围>=<权限设置> 设置权限范围内的目录或者文件的权限为指定的值权限范围

u ：目录或者文件的当前的用户  
g ：目录或者文件的当前的群组  
o ：除了目录或者文件的当前用户或群组之外的用户或者群组  
a ：所有的用户及群组  


权限代号：
r ：读权限，用数字4表示  
w ：写权限，用数字2表示  
x ：执行权限，用数字1表示  
\- ：删除权限，用数字0表示  
s ：特殊权限

### ln命令
ln（link files）的功能是为某一个文件在另外一个位置建立一个同步的链接。

当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后在 其它的目录下用ln命令链接（link）它就可以，不必重复的占用磁盘空间。

语法: ln [参数][源文件或目录][目标文件或目录]

参数
 -f 强制执行
 -s 软链接(符号链接)

#### alignDB
用于分析基因组序列中indels和snp之间关系的计算协议

## Jim Kent bin
```bash

echo "==> Jim Kent bin"
cd $HOME/bin/
RELEASE=$( ( lsb_release -ds || cat /etc/*release || uname -om ) 2>/dev/null | head -n1 )
if [[ $(uname) == 'Darwin' ]]; then
    curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-darwin-2011.tar.gz
else
    if echo ${RELEASE} | grep CentOS > /dev/null ; then
        curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-centos-7-2011.tar.gz
    else
        curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-ubuntu-1404-2011.tar.gz
    fi
fi \
    > jkbin.tar.gz

echo "==> untar from jkbin.tar.gz"
tar xvfz jkbin.tar.gz x86_64/axtChain
tar xvfz jkbin.tar.gz x86_64/axtSort
tar xvfz jkbin.tar.gz x86_64/axtToMaf
tar xvfz jkbin.tar.gz x86_64/chainAntiRepeat
tar xvfz jkbin.tar.gz x86_64/chainMergeSort
tar xvfz jkbin.tar.gz x86_64/chainNet
tar xvfz jkbin.tar.gz x86_64/chainPreNet
tar xvfz jkbin.tar.gz x86_64/chainSplit
tar xvfz jkbin.tar.gz x86_64/chainStitchId
tar xvfz jkbin.tar.gz x86_64/faToTwoBit
tar xvfz jkbin.tar.gz x86_64/lavToPsl
tar xvfz jkbin.tar.gz x86_64/netChainSubset
tar xvfz jkbin.tar.gz x86_64/netFilter
tar xvfz jkbin.tar.gz x86_64/netSplit
tar xvfz jkbin.tar.gz x86_64/netSyntenic
tar xvfz jkbin.tar.gz x86_64/netToAxt

mv $HOME/bin/x86_64/* $HOME/bin/
rm jkbin.tar.gz
```

#### lsb_release命令
LSB是Linux Standard Base的缩写，lsb_release命令用来显示LSB和特定版本的相关信息。如果使用该命令时不带参数，则默认加上-v参数。

语法格式： lsb_release [参数]

常用参数：

-i	显示系统名称简写
-d	显示系统全称和版本号
-r	显示版本号
-a	显示LSB所有信息

#### 竖线‘|’ 和双竖线‘||’
竖线‘|’，在linux中是作为管道符的，将‘|’前面命令的输出作为'|'后面的输入。

用双竖线‘||’分割的多条命令，执行的时候遵循如下规则，如果前一条命令为真，则后面的命令不会执行，如果前一条命令为假，则继续执行后面的命令。

#### *（星号）
\*（星号）是linux中的通配符，代表一个或一个以上的所有字符。linux的隐藏文件和隐藏文件夹都是以.（点号）开头，所以.*应该是代表当前目录下的所有隐藏目录和隐藏文件夹。
如果是./*则表示当前目录下的所有文件和所有目录，因为.（点号）还有代表当前目录的意思。

#### uname 命令查看Linux内核版本
uname（Unix name），功能是用于查看系统主机名、内核及硬件架构等信息。如果不加任何参数，默认仅显示系统内核名称，相当于-s参数。

语法格式：uname [option]

常用参数：

-a	显示系统所有相关信息
-m	显示计算机硬件架构
-n	显示主机名称
-r	显示内核发行版本号
-s	显示内核名称
-v	显示内核版本
-p	显示主机处理器类型
-o	显示操作系统名称
-i	显示硬件平台

#### head -n 1: 显示首行
head命令
1．head命令格式：head [Option] [File]

2．命令功能：head 用来显示档案的开头至标准输出中，默认head命令打印其相应文件的开头10行。

3．Option：
-q 隐藏文件名
-v 显示文件名
-c<字节> 显示字节数
-n<行数> 显示的行数

#### Linux内核( Linux kernel )

内核是操作系统的核心 ，其主要功能有：

　　1.响应中断，执行中断服务程序 　　2.管理多个进程，调度和分享处理器的时间 　　3.管理进程地址空间的内存管理 　　4.网络和进程间通信等系统服务程序

内核的活动范围：

　　1.运行于用户空间，执行用户进程
　　2.运行于内核空间，处于进程上下文，代表某个特定进程的执行
　　3.运行于内核空间，处于中断上下文，与任何进程无关，处理某个特定的中断

Linux 的版本号分为两部分，即内核版本与发行版本。


## faToTwoBit
```bash
if [[ $(uname) == 'Darwin' ]]; then
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/macOSX.x86_64/faToTwoBit
else
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/faToTwoBit
fi \
    > faToTwoBit
mv faToTwoBit $HOME/bin/
chmod +x $HOME/bin/faToTwoBit
```

#### curl -L

curl是一种数据传输程序，使用其中一种支持的协议（DICT，FILE，FTP，FTPS，GOPHER，HTTP，HTTPS，IMAP，IMAPS，LDAP，LDAPS，POP3，POP3S，RTMP，RTSP，SCP，SFTP，SMTP，SMTPS，TELNET和TFTP）从服务器或服务器传输数据的工具。该命令旨在无需用户交互即可工作。

参数：
-L/--location 跟踪重定向


#### faToTwoBit: 将fasta格式改为 .2bit格式
.2bit:二进制文件


Reference
环境变量：https://blog.csdn.net/qingkongyeyue/article/  
chomd:https://www.yiibai.com/linux/chmod.html
