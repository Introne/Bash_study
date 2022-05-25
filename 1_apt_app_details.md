## linuxbrew依赖项

### build-essential
Ubuntu默认情况下，并没有提供C/C++的编译环境，因此还需要手动安装。但是如果单独安装gcc以及g++比较麻烦，幸运的是，Ubuntu提供了一个build-essential软件包。

查看该软件包的依赖关系：  
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

### curl 
curl：文件传输工具 
curl是一个利用URL语法在命令行下工作的文件传输工具。 它支持文件上传和下载，是综合传输工具。

curl支持的通信协议有FTP、FTPS、HTTP、HTTPS、TFTP、SFTP、Gopher、SCP、Telnet、DICT、FILE、LDAP、LDAPS、IMAP、POP3、SMTP和RTSP。  
curl还支持SSL认证、HTTP POST、HTTP PUT、FTP上传, HTTP form based upload、proxies、HTTP/2、cookies、用户名+密码认证(Basic, Plain, Digest, CRAM-MD5, NTLM, Negotiate and Kerberos)、file transfer resume、proxy tunneling。


### git
git是一种版本控制系统，主要用于管理源代码。

解释：
1、编写源代码时需要备份，本地机器和远程服务器都需要存放一份代码，这个时候git可以提供一套机制使其本地和远程同步；  
2、对同一份代码更改，可以互不影响，又可以同步别人的代码；  
3、版本更新，历史代码更改记录，可以查询很详细的信息。


## 其他工具

### aptitude 
Aptitude，一个基于Ncurses的APT文字介面前端程序，是Debian及其衍生系统中功能极其强大的软件包管理系统。以交互形式运行时，它可以显示出所有可用的软件包，并允许用户选择包来安装或卸载。它还有一个强大的搜索系统，可通过多种搜索模式进行搜索。 

### parallel ：并行执行脚本
GNU parallel是用于Linux和其他类Unix操作系统的命令行驱动的实用工具，它允许用户并行的执行shell脚本。

### vim：文本编辑器
Vim 是从vi 发展出来的一个文本编辑器。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。 

### screen：命令行终端工具 
screen 是一款由GNU 开发的命令行终端工具，它提供了从多个终端窗口连接到同一个shell 会话（会话共享）。 当网络中断，或终端窗口意外关闭时，screen 中运行的程序任然可以运行（注：系统自带的终端窗口，当窗口意外关闭时，在该终端窗口中运行的程序也会终止。）。 

### xsltproc 
xsltproc是由DanielVeillard用来C语言编写的是一个快速XSLT引擎,它可以将通过XSL层叠样式表把XML转换为相应格式的文件，比如：HTML,XHTML,PDF...

### numactl
numactl工具可用于查看当前服务器的NUMA节点配置、状态，可通过该工具将进程绑定到指定CPU core，由指定CPU core来运行对应进程。 


## 安装gtk3

### gtk：图型界面管理程序库
gtk(gimp toolkit)是GIMP 开发过程中用来管理图型界面的一套工具程序库，是完全使用C语言开发的。

### xvfb：显示服务协议的显示服务器
Xvfb是一个实现了X11显示服务协议的显示服务器。 不同于其他显示服务器，Xvfb在内存中执行所有的图形操作，不需要借助任何显示设备。

### glade：图形用户界面产生器
Glade 是GTK+ 图形用户界面产生器。 也就是说，Glade 是个Visual Programming Tool，和Microsoft Windows 平台的Visual Tools 类似，只要用鼠标拉一拉，它就会自动帮你产生C source code。

## 画图工具

### gnuplot：交互式绘图工具 
Gnuplot是一个命令行的交互式绘图工具（command-driven interactive function plotting program）。用户通过输入命令，可以逐步设置或修改绘图环境，并以图形描述数据或函数，使我们可以借由图形做更进一步的分析。

### graphviz
Graphviz （英文：Graph Visualization Software的缩写）是一个由AT&T实验室启动的开源工具包，可以用于绘制DOT语言脚本描述的图形。 

### imagemagick：图片处理工具
ImageMagick是一个免费的创建、编辑、合成图片的软件。它可以读取、转换、写入多种格式的图片。实现图片切割、颜色替换、各种效果的应用，图片的旋转、组合，文本，直线， ...

### nautilus-open-terminal 
可随处打开终端的Nautilus 插件

### nautilus-actions
Nautilus Actions是一个文件管理器扩展，允许通过所选文件的上下文菜单添加任意程序。 
