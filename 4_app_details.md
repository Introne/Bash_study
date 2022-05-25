## 编译

在Linux下进行嵌入式开发，我们需要使用gcc、make、cmake等工具来编译。

    源文件较少时可以使用直接调用gcc工具进行编译。

    源码文件较多时，可借用make工具。make通过解析Makefile文件来执行一些gcc命令进行编译。

    简单的项目，Makefile还可以自己写一写。但实际项目中，我们很少直接编写Makefile，而是借助cmake工具来生成。cmake根据一个CMakeLists.txt文件来生成不同平台的Makefile文件，达到跨平台的作用。


## gcc
GCC编译器：GCC（GNU Compiler Collection）是由 GNU 开发的编程语言编译器。 GCC最初代表“GNU C Compiler”，当时只支持C语言。 后来又扩展能够支持更多编程语言，包括 C++、Fortran 和 Java 等。

## make工具

### autoconf：从configure.ac 生成 configure
GNU Autoconf是一个在Bourne shell下制作供编译、安装和打包软件的配置脚本（Configure_script (computing)）的工具。  
Autoconf并不受程式语言限制，常用于C、C++、Erlang和Objective-C。  
配置脚本控制了一个软件包在特定系统上的安装。在进行一系列测试后，配置脚本从模板中生成makefile与头文件进而调整软件包，使之适应某一种系统。  
Autoconf与Automake、Libtool等软件组成了GNU构建系统。
由Autoconf生成的配置脚本在运行的时候与Autoconf是无关的，就是说配置脚本的用户并不需要拥有Autoconf。

### libtool：平台支持
提供一种标准的方法来创建静态库或共享库。它把特定于平台的库的产生过程的复杂性隐藏在一个统一的接口后面，这个接口通过libtool被所有的平台支持。
过程：Makefile.in、config.h.in-->configure-->config.status、Makefile、config.h 

### autogen：生成Makefile
AutoGen是一种用于生成包含具有不同替换的重复文本的程序文件的工具。它的目标是简化包含大量重复文本的程序的维护。如果这种文本的几个块必须在并行表中保持同步，那么这一点尤其有价值。

### automake：用Makefile.am(s) 和configure.ac 生成 Makefile.in(s)
Automake是一个从文件 Makefile.am 自动生成 Makefile.in 的工具。  
每个 Makefile.am 基本上是一系列 make 的宏定义 （make规则也会偶尔出现）。生成的 Makefile.in 服从 GNU Makefile 标准。  
GNU Makefile 标准文档长、复杂，而且会发生改变。  
Automake 的目的就是解除个人GNU维护者维护 Makefile 的负担 （并且让Automake的维护者来承担这个负担）。

### GUN构建系统：autoconf，automake、libtool

用户通过configure->make->make install基于源码安装软件。然而大部分用户可能并不知道这个过程究竟做了些什么。

configure脚本是由软件开发者维护并发布给用户使用的shell脚本。这个脚本的作用是检测系统环境，最终目的是生成Makefile和config.h。

make通过读取Makefile文件，开始构建软件。而make install可以将软件安装到需要安装的位置。

#### makefile

makefile用来定义整个工程的编译规则。一个工程中的源文件计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为 makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。

makefile带来的好处就是——“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率。make是一个命令工具，是一个解释makefile中指令的命令工具，一般来说，大多数的IDE都有这个命令，比如：Delphi的make，Visual C++的nmake，Linux下GNU的make。可见，makefile都成为了一种在工程方面的编译方法。

## cmake

CMake是一种跨平台的构建系统， 类似于Makefile, 是专为C/C++开发的一套构建系统。

它使用自己定义的一套脚本语言来描述构建关系，最终也是生成Makefile。

## Homebrew Formulae

### gpatch：Apply a diff file to an original
单词“Patch”是补丁的意思，因此PATCH文件通常是软件补丁、内存补丁、文件补丁等，通常用于更新软件或者修复软件问题。

具体来说：
使用diff命令，可以在原始文件的基础上进行修改后，根据所做的修改生成补丁文件。
使用patch命令，可以将该补丁打到原始文件上，使之成为修改后的文件。

### pkg-config：Manage compile and link flags for libraries
pkg-config 是一个常用的库信息提取工具。能根据软件安装时软件的.pc配置文件路径找到相应的头文件路径和库文件路径

## 语法分析器

### bison
bison 是一个语法分析器的生成器。

它读取用户提供的语法的产生式，生成一个 C 语言格式的 LALR(1) 动作表

### flex
Flex是一个生成词法分析器的工具，它可以利用正则表达式来生成匹配相应字符串的C语言代码，其语法格式基本同Lex相同。

## 其他

###  libs：静态链接库
lib是库（Library）的英文缩写，它主要存放系统的链接库文件，没有该目录则系统就无法正常运行。 

### gd：图片生成
gd 库是PHP处理图形的扩展库，它提供了一系列用来处理图片的API（应用程序编程接口），使用gd 库可以处理图片或者生成图片

### gsl：数值计算 
GSL是一个用于数值计算的C库。 例如，您可以使用GSL来求解线性方程组、将曲线拟合到一组点、进行数值积分、统计计算等。

### jemalloc：内存分配器 
JeMalloc 是一款内存分配器，与其它内存分配器相比，它最大的优势在于多线程情况下的高性能以及内存碎片的减少。

### boost：C++程序库
Boost是为C++语言标准库提供扩展的一些C++程序库的总称。 Boost库是一个可移植、提供源代码的C++库，作为标准库的后备，是C++标准化进程的开发引擎之一，是为C++语言标准库提供扩展的一些C++程序库的总称。

### fftw：C语言程序集
FFTW ( the Faster Fourier Transform in the West) 是一个快速计算离散傅里叶变换的标准C语言程序集，其由MIT的M.Frigo 和S. Johnson 开发。 可计算一维或多维实和复数据以及任意规模的DFT。

### libffi：编译器
libffi的作用就相当于编译器，它为多种调用规则提供了一系列高级语言编程接口，然后通过相应接口完成函数调用，底层会根据对应的规则，完成数据准备，生成相应的汇编指令代码。

### libgit2: Git 核心开发包 
libgit2 是一个可移植、纯 C 语言实现的 Git 核心开发包，你可以使用它来编写自定义的 Git 应用。

libgit2 已被广泛应用在许多应用程序上，包括 GitHub 网站，还被应用在 Plastic SCM 和强大的微软 Visual Studio 工具箱。

### libxml2：XML解析的开发工具包
libxml2是一个用于XML解析的开发工具包，提供C语言接口。

### libgcrypt: 加密算法库
libgcrypt是一个非常成熟的加密算法库，也是著名的开源加密软件GnuPG的底层库，支持多种对称、非对称加密算法，以及多种Hash算法。

### libxslt:XSL解析
libxslt为后端的Python,PHP,PERL,RUBY及前端的safari,opera,chrome提供XSL解析。

### pcre: Perl函数库 
PCRE(Perl Compatible Regular Expressions)是一个轻量级的Perl函数库，包括perl 兼容的正则表达式库。 

### libedit: 命令行编辑器库 
libedit命令行编辑器库: 提供了通用的行编辑、历史记录和标记化功能.

### readline: 行编辑库 
readline 从字面上来理解，就是从“行”上面读取。 实际上就是一个行编辑库，bash 在用，mysql 也在用，mutt 也在用。 通过readline，可以方便的在命令行上面移动，增删，复制，粘贴，搜索。

### sqlite: 数据库、程序库、命令行工具、学习关系型数据库的工具
SQLite（/ˌɛskjuːɛlˈlaɪt/或/ˈsiːkwəl.laɪt/）是遵守ACID的关系数据库管理系统，它包含在一个相对小的C程式庫中。 与许多其它数据库管理系统不同，SQLite不是一个客户端/服务器结构的数据库引擎，而是被集成在用户程序中

### nasm: 汇编工具 
Netwide Assembler （简称NASM）是一款基于x86架构的汇编与反汇编软件。 它可以用来编写16位（8086、80286等）、32位（IA-32）和64位（x86_64）的程序。 NASM被认为是Linux平台上最受欢迎的汇编工具之一。

### bzip2: 解压 
bzip2 是一个基于Burrows-Wheeler 变换的无损压缩软件，压缩效果比传统的LZ77/LZ78压缩演算法来得好。 它免费提供，具有高质量的数据压缩能力。 bzip2 利用先进的压缩技术，能够把文件压缩到10%至15%，压缩的速度和解压的效率都非常高！ 支持大多数压缩格式，包括tar、gzip 等等。

### gzip: 解压  
gzip是在Linux系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。gzip不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和tar命令一起构成Linux操作系统中比较流行的压缩文件格式。据统计，gzip命令对文本文件有60%～70%的压缩率。减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。

### libarchive：开源压缩库（存在代码执行漏洞！！！）  
Libarchive是一个开源压缩库，Libarchive广泛应用于Debian Linux、FreeBSD和诸多文件压缩工具。

### libzip：读取，创建和修改zip档案 
libzip是一个C库，用于读取，创建和修改zip档案。可以从数据缓冲区，文件或直接从其他zip归档文件直接复制的压缩数据中添加文件。

### xz：压缩 
xz 是一个使用LZMA压缩算法的无损数据压缩文件格式。 与gzip、bzip2相同，同样支持多文件压缩，但是约定不能将多于一个的目标文件压缩进同一个档案文件。

# download tools

### aria2：多线程下载 
Aria2是一个命令行下运行、多协议、多来源下载工具（HTTP/HTTPS、FTP、BitTorrent、Metalink），并且支持迅雷离线以及百度云等常用网盘的多线程下载（甚至可以超过专用客户端的下载速度）。

### curl：文件传输工具 
curl是一个利用URL语法在命令行下工作的文件传输工具。 它支持文件上传和下载，是综合传输工具。

curl支持的通信协议有FTP、FTPS、HTTP、HTTPS、TFTP、SFTP、Gopher、SCP、Telnet、DICT、FILE、LDAP、LDAPS、IMAP、POP3、SMTP和RTSP。  
curl还支持SSL认证、HTTP POST、HTTP PUT、FTP上传, HTTP form based upload、proxies、HTTP/2、cookies、用户名+密码认证(Basic, Plain, Digest, CRAM-MD5, NTLM, Negotiate and Kerberos)、file transfer resume、proxy tunneling。

### wget
wget 是一个从网络上自动下载文件的自由工具，支持通过HTTP、HTTPS、FTP 三个最常见的TCP/IP协议 下载，并可以使用HTTP 代理。
 "wget" 这个名称来源于“World Wide Web” 与“get” 的结合。

# gnu

### gnu-sed：流编辑器 
sed是一个流编辑器，用于处理来自文件或管道的输入流，进行基本文本的匹配及处理。虽然在某些方面类似于允许进行脚本编辑（例如ed）的编辑器，但是sed仅仅对其输入内容从头到尾逐行读入、匹配和处理，因此效率更高。sed能够对来自于管道的文本进行过滤

### gnu-tar：打包工具
tar是Unix和类Unix系统上的归档打包工具，可以将多个文件合并为一个文件，打包后的文件名亦为“tar”。

# other tools

### screen：命令行终端工具 
screen 是一款由GNU 开发的命令行终端工具，它提供了从多个终端窗口连接到同一个shell 会话（会话共享）。 当网络中断，或终端窗口意外关闭时，screen 中运行的程序任然可以运行（注：系统自带的终端窗口，当窗口意外关闭时，在该终端窗口中运行的程序也会终止。）。

### stow：软件包管理工具 
GNU Stow 是一个用来管理安装的软件包的工具，Stow 将软件安装在独立的路径，例如/usr/local/stow/emacs vs. /usr/local/stow/perl。

### htop：系统监控工具 
htop是一个进程管理工具，
Htop是一款运行于Linux系统监控与进程管理软件，用于取代Unix下传统的top。与top只提供最消耗资源的进程列表不同，htop提供所有进程的列表，并且使用彩色标识出处理器、swap和内存状态。
用户一般可以在top无法提供详尽系统信息的情况下选择安装并使用htop。比如，在查找应用程序的内存泄漏问题时。与top相比，htop提供更方便、光标控制的界面来杀死进程。
htop用C语言编写，采用了ncurses库。htop的名称源于其作者的名字。

### parallel：并行执行脚本
GNU parallel是用于Linux和其他类Unix操作系统的命令行驱动的实用工具，它允许用户并行的执行shell脚本。 

### pigz 
pigz是一个C写的打包解包开源工具。 它代表gzip的并行实现，是gzip的全功能替代品，在压缩数据时利用多个处理器和多个内核，即支持多线程并行处理，解压缩比gzip快。

### cloc 
Cloc是一款使用Perl语言开发的开源代码统计工具，支持多平台使用、多语言识别，能够计算指定目标文件或文件夹中的文件数（files）、空白行数（blank）、注释行数（comment）和代码行数（code）。

### tree：树状目录结构 
一款用于展示目录树状结构的命令工具

### pv：页面浏览
PV（page view）即页面浏览量，通常是衡量一个网络新闻频道或网站甚至一条网络新闻的主要指标。网页浏览数是评价网站流量最常用的指标之一，简称为PV。监测网站PV的变化趋势和分析其变化原因是很多站长定期要做的工作。 Page Views中的Page一般是指普通的html网页，也包含php、jsp等动态产生的html内容。来自浏览器的一次html内容请求会被看作一个PV，逐渐累计成为PV总数。

## 命令行工具

### jq：处理JSON 数据的工具 
jq 是一款命令行下处理JSON 数据的工具。 其可以接收标准输入，命令管道或者文件中的JSON 数据，经过一系列的过滤器(filters)和表达式的转后形成我们需要的数据结构并将结果输出到标准输出中。 

注：JSON是JavaScript Object Notation的简称，是一种轻量的数据表示方法

### pup：命令行HTML解析工具。
pup 是一个用于处理 HTML 的命令行工具。它从标准输入读取，打印到标准输出，并允许用户使用 CSS 选择器过滤页面的某些部分。受 jq 的启发，pup 旨在成为一种从终端探索 HTML 的快速灵活的方式。

### datamash: linux极简统计分析工具 
datamash 作为一个命令行程序可以对文本文件进行数字和文本相关的基本统计操作（虽说基本但是所有的操作都足够常用高频）。 

### miller: 数据文件调整工具 
Miller 是一个命令行工具，用于查询、调整和重新格式化各种格式的数据文件，包括CSV、TSV、JSON 和JSON Lines。

### tsv-utils：大型表格数据文件处理工具 
tsv-utils：eBay的TSV实用程序：用于大型表格数据文件的命令行工具。 过滤，统计，抽样，联接等

### bat：cat命令升级版 
bat 是命令行下一款用来显示文件内容的工具，bat 命令功能跟常用命令 cat 类似。只是 bat 功能上更加强大一些，bat 在cat命令的基础上加入了行号显示、代码高亮和 Git 集成。

### exa：ls命令升级版
 exa 是 ls 命令的一个可替代方案。

它色彩艳丽，还可以显示 git 状态等其他信息，自动将文件大小转换为方便人们阅读的单位，并且所有这些都保持与 ls 几乎相同的执行速度。

### hyperfine：命令行基准测试工具 
特性：
 
支持统计跨测试运行结果
支持任意 shell 命令
持续反馈基准进度和估计进度
支持在做正式基准之前执行预测试运行
每次运行前设置缓存清除命令
异常值统计
支持多种格式导出结果：CSV、JSON、Markdown。
参数化基准测试
跨平台

https://github.com/chinanf-boy/hyperfine-zh

### ripgrep：grep命令升级版   
Ripgrep 是命令行下一个基于行的搜索工具，RipGrep 使用Rust 开发，可以在多平台下运行，支持Mac、Linux 和Windows 等平台。 

### tealdeer：man命令简化版  
Too long, Don't read, 简化版的man pages查看工具。 

### librsvg：SVG渲染库 
一个SVG文件可以被转换为PNG

注：可缩放矢量图形（英语：Scalable Vector Graphics，缩写：SVG）是一种基于可扩展标记语言（XML），用于描述二维矢量图形的图形格式。

### udunits 
UDUNITS 包支持物理量单位。它的 C 库提供了单位的算术操作和兼容单位之间的数值转换。该软件包包含一个扩展的单元数据库，它采用 XML 格式并且用户可扩展。

### proxychains-ng 
ProxyChains 是Linux 和其他unix 下的代理工具。 它可以使任何程序通过代理上网， 允许TCP 和DNS 通过代理隧道， 支持HTTP、 SOCKS4 和SOCKS5 类型的代理服务器， 并且可配置多个代理。 

### yasm

### yasm 

### yasm 

### yasm 

### yasm

### yasm 

### yasm 

### yasm 

### yasm

### yasm 

### yasm 

### yasm 

### yasm

### yasm 

### yasm 

### yasm 

### yasm

### yasm 

### yasm 

### yasm 

### yasm

### yasm 

### yasm 

### yasm 

### yasm
