# brew.sh命令解释by sn

## 1. 禁用Homebrew自动更新

```bash
export HOMEBREW_NO_AUTO_UPDATE=1
# export ALL_PROXY=socks5h://localhost:1080
```

### export命令

在shell中执行程序时，shell会提供一组环境变量。export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅及于该此登陆操作。
参数：
　-f 　代表[变量名称]中为函数名称。
　-n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
　-p 　列出所有的shell赋予程序的环境变量123
 
延伸
export设置环境变量是暂时的，只在本次登录中有效，可修改如下文件来使命令长久有效。

1. 修改profile文件
2. 修改本id根路径下的.bashrc或.bash_profile文件
   
### 代理设置
同时设置http、https以及ftp代理：export ALL_PROXY=
设置http代理：export http_proxy=
设置https代理：export HTTPS_PROXY=
设置ftp代理：export FTP_PROXY=

### SOCKS5协议
SOCKS5 是一个代理协议，它在使用TCP/IP协议通讯的前端机器和服务器机器之间扮演一个中介角色，使得内部网中的前端机器变得能够访问Internet网中的服务器，或者使通讯更加安全。SOCKS5 服务器通过将前端发来的请求转发给真正的目标服务器， 模拟了一个前端的行为。在这里，前端和SOCKS5之间也是通过TCP/IP协议进行通讯，前端将原本要发送给真正服务器的请求发送给SOCKS5服务器，然后SOCKS5服务器将请求转发给真正的服务器。

## 2. 清除brew下载中断的文件

```bash
# Clear caches
rm -f $(brew --cache)/*.incomplete
```

## rm -f
rm（remove）： rm[选项] 文件或目录
用法：rm [选项] 文件或目录
选项：
-f：强制删除（force），和 -i 选项相反，使用 -f，系统将不再询问，而是直接删除目标文件或目录。
-i：和 -f 正好相反，在删除文件或目录之前，系统会给出提示信息，使用 -i 可以有效防止不小心删除有用的文件或目录。
-r：递归删除，主要用于删除目录，可删除指定目录及包含的所有内容，包括所有的子目录和文件。

### Cache
cache叫做高速缓冲存储器，是介于中央处理器和主存储器之间的高速小容量存储器。

cache作用：CPU的速度远高于内存，当CPU直接从内存中存取数据时要等待一定时间周期，而Cache则可以保存CPU刚用过或循环使用的一部分数据，如果CPU需要再次使用该部分数据时可从Cache中直接调用，这样就避免了重复存取数据，减少了CPU的等待时间，因而提高了系统的效率。

### brew --cache

```bash
snwang@DESKTOP-T89G9DA:~$ brew --cache
/home/snwang/.cache/Homebrew
```

显示cache的目录地址（找到linuxbrew的缓存目录）  

### $()命令替换
在Bash中，$( )与` `（反引号）都是用来作命令替换的。
命令替换与变量替换差不多，都是用来重组命令行的，先完成引号里的命令行，然后将其结果替换出来，再重组成新的命令行。

### /*.incomplete
*通配符: 作为匹配文件名扩展的一个通配符，能自动匹配给定目录下的每一个文件；
.incomplete: 未完全下载文件的后缀名

## 配置PERL环境变量

```bash
if grep -q -i PERL_534_PATH $HOME/.bashrc; then
    echo "==> .bashrc already contains PERL_534_PATH"
else
    echo "==> Updating .bashrc with PERL_534_PATH..."
    PERL_534_BREW=$(brew --prefix)/Cellar/$(brew list --versions perl | sed 's/ /\//' | head -n 1)
    PERL_534_PATH="export PATH=\"$PERL_534_BREW/bin:\$PATH\""
    echo '# PERL_534_PATH' >> $HOME/.bashrc
    echo $PERL_534_PATH    >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    # make the above environment variables available for the rest of this script
    eval $PERL_534_PATH
fi
```

### grep语句
Linux grep 命令用于查找并显示文件里符合条件的字符串。
可与正则表达式搭配使用，参数作为搜索过程中的补充或对输出结果的筛选。
相关参数：
-i 或 --ignore-case : 忽略字符大小写的差别。
-q 或 --quiet或--silent : 不显示任何信息

### sed 's/ /\//'
将上个管道的输出：perl 5.34.0中的空格替换为/，即输出perl/5.34.0

sed：Stream Editor文本流编辑，能配合正则表达式使用。
's/代表替换：'s/查找内容/替换为的字串/'
\表示转义

### head -n 1（<font color=skyBlue>不太确定该命令在此处的意义</font>）
将上个管道的输出perl/5.34.0，首行输出

head命令
1．head命令格式：head [Option] [File]

2．命令功能：head 用来显示档案的开头至标准输出中，默认head命令打印其相应文件的开头10行。 

3．Option：
-q 隐藏文件名
-v 显示文件名
-c<字节> 显示字节数
-n<行数> 显示的行数

### 双引号与单引号的区别
双引号内其可以包含特殊字符(单引号直接输出内部字符串，不解析特殊字符；双引号内则会解析特殊字符)，包括', ", $, \,如果要忽略特殊字符，就可以利用\来转义，忽略特殊字符，作为普通字符输出

### eval命令
eval（evaluation）
Double check!!

### 一些命令输出
```bash
snwang@DESKTOP-T89G9DA:~$ echo $HOME
/home/snwang
```

```bash
snwang@DESKTOP-T89G9DA:~$ brew list --versions perl
perl 5.34.0
```

```bash
snwang@DESKTOP-T89G9DA:~$ brew --prefix
/home/linuxbrew/.linuxbrew
```

## 安装cpanm
```bash
hash cpanm 2>/dev/null || {
    curl -L https://cpanmin.us |
        perl - -v --mirror-only --mirror http://mirrors.ustc.edu.cn/CPAN/ App::cpanminus
}
```

### hash
1.什么是hash？
linux系统下会有一个hash表，刚开机时会显示hash为空，当你执行过一次或多次命令，hash就会记录下执行过的命令的路径第一次执行命令shell解释器默认的会从PATH路径下寻找该命令的路径，当你第二次使用该命令时，shell解释器首先会查看hash表，没有该命令才会去PATH路径下寻找
2.hash表的作用是什么?
hash大大提高命令的调用速率。
3.参数
	-l  显示hash表内容	
	-r 清除hash表
	-d openssl 删除表中某一条（删除openssl）
	-t openssl 查看openssl命令路径（hash表中没有的话，可以调用which命令）
	-p /usr/bin/openssl aliesopenssl 往hash表中添加一条，执行aliesopenssl即	执行openssl命令（起别名）

### cpanm（<font color=VioletRed>不太理解hash cpanm的意思，是判断是否有安装后续命令的意思么？</font>）
cpanm是安装 Perl模块的最方便的方法。

### 双竖线‘||’
用双竖线‘||’可以分割多条命令，执行的时候遵循如下规则，如果前一条命令为真，则后面的命令不会执行，如果前一条命令为假，则继续执行后面的命令。

### 2>/dev/null
把错误输出到 “黑洞”

2是标准错误的文件描述符
>是重定向符号，该重定向会覆盖原有内容
>/dev/null 是一个特殊的设备文件，这个文件接收到任何数据都会被丢弃。因此，null 这个设备通常也被称为位桶（bit bucket）或黑洞。

综上，2>/dev/null 的意思就是将标准错误 stderr 删掉。

### curl -L
curl是一种数据传输程序，使用其中一种支持的协议（DICT，FILE，FTP，FTPS，GOPHER，HTTP，HTTPS，IMAP，IMAPS，LDAP，LDAPS，POP3，POP3S，RTMP，RTSP，SCP，SFTP，SMTP，SMTPS，TELNET和TFTP）从服务器或服务器传输数据的工具。该命令旨在无需用户交互即可工作。

参数：
-L/--location	跟踪重定向



#### link
cache： https://blog.csdn.net/boyaaboy/article/details/102539578
