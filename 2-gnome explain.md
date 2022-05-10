## GUI设置章节

### 第一弹
```
#!/usr/bin/env bash  
```
设置Bash解释器运行环境


### 第二弹
```
# http://superuser.com/questions/244189/bashrc-how-to-know-x-window-is-available-or-not
# https://unix.stackexchange.com/questions/313338/gnome3-how-do-i-remove-favorites-from-dash-via-terminal
if [ -n "$DISPLAY" ]; then
    # gsettings get com.canonical.Unity.Launcher favorites
    echo "==> Set favorites"
    gsettings set org.gnome.shell favorite-apps "['ubiquity.desktop', 'org.gnome.Nautilus.desktop', 'gnome-terminal.desktop', 'firefox.desktop', 'gnome-system-monitor.desktop']"
    # http://askubuntu.com/questions/177348/how-do-i-disable-the-screensaver-lock
    echo "==> Disable lock screen"
    gsettings set org.gnome.desktop.screensaver lock-enabled false
    gsettings set org.gnome.desktop.session idle-delay 0 # (0 to disable)

    # http://askubuntu.com/questions/79150/how-to-remove-bookmarks-from-the-nautilus-sidebar/152540#152540
    echo "==> Remove nautilus bookmarks"
    echo "enabled=false" > "$HOME/.config/user-dirs.conf"

#    sed -i 's/\Documents//' "$HOME/.config/user-dirs.dirs"
#    sed -i 's/\Downloads//' "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Music//'     "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Pictures//'  "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Public//'    "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Templates//' "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Videos//'    "$HOME/.config/user-dirs.dirs"

#    rm -fr "$HOME/Documents"
#    rm -fr "$HOME/Downloads"
    rm -fr "$HOME/Music"
    rm -fr "$HOME/Pictures"
    rm -fr "$HOME/Public"
    rm -fr "$HOME/Templates"
    rm -fr "$HOME/Videos"

    mkdir -p "$HOME/.config/gtk-3.0/"
    echo > "$HOME/.config/gtk-3.0/bookmarks"

else
    echo "This script should be execute inside a GUI terminal"
fi
```
if语句检查是否有图形界面，如果有就利用gsettings更改gnome相关终端配置，通过sed -i 's//'进行目录的删除或修改；如果没有就直接在终端上输出This script should be execute inside a GUI terminal

### 背景
#### DISPLAY
在Linux/Unix类操作系统上, DISPLAY用来设置将图形显示到何处.
DISPLAY 环境变量格式如下host:NumA.NumB,host指Xserver所在的主机主机名或者ip地址, 图形将显示在这一机器上, 可以是启动了图形界面的Linux/Unix机器, 也可以是安装了Exceed, X-Deep/32等Windows平台运行的Xserver的Windows机器. 如果Host为空, 则表示Xserver运行于本机, 并且图形程序(Xclient)使用unix socket方式连接到Xserver,而不是TCP方式. 使用TCP方式连接时, NumA为连接的端口减去6000的值, 如果NumA为0, 则表示连接到6000端口; 使用unix socket方式连接时则表示连接的unix socket的路径, 如果为0, 则表示连接到/tmp/.X11-unix/X0 . NumB则几乎总是0.

简要来说：如果用户有图形界面，DISPLAY将被设置为某个非零数值

#### .bashrc
如果运行一个基于 Unix 或者类 Unix 的操作系统，bash 很有可能是作为默认终端被安装的。虽然存在很多不同的 shell，bash 却是最常见或许也是最主流的。简单来说，bash 是一个能解释用户输入进终端程序的东西，并且基于你的输入来运行命令。它在一定程度上支持使用脚本来定制功能，这时候就要用到 .bashrc 了。

为了加载配置，bash 在每次启动时都会加载 .bashrc 文件的内容。每个用户的 home 目录都有这个 shell 脚本。它用来存储并加载用户的终端配置和环境变量。

终端配置可以包含很多不同的东西。最常见的，.bashrc 文件包含用户想要用的别名。别名允许用户通过更短的名字或替代的名字来指向命令，对于经常在终端下工作的人来说这可是一个省时利器。

#### if [ -n "$DISPLAY" ]
-n string：如果 string长度非零，则为真  [ -n “$myvar” ]

#### gsetting
gsettings提供了对GSetings的命令行操作。
GSetings实际上是一套高级API，用来操作dconf。
dconf存储着GNOME3的配置，是二进制格式。它做为GSettings的后端系统存在，暴露出低级API。在GNOME2时代，类似的角色是gconf，但它是以XML文本形式存储。

更接地气的说法是，dconf是GNOME3的注册表，gsettings是一个查询、读取、设置注册表键值的命令行工具。

而gsettings set "org.gnome.settings可以设置某个schema下某个key的值
比如设置shell下的favorite-apps的值等

#### rm -fr
rm （remove）：删除命令。命令语法：rm [options] name...
命令参数：-i 删除前逐一询问确认；-f 即使原档案属性设为唯读，亦直接删除，无需逐一确认；-r 将目录及以下之档案亦逐一删除

#### mkdir 
mkdir）make directories）：创建新目录，此命令所有用户都可以使用。命令语法：mkdir [options] directory_name
命令参数：
-m 选项用于手动配置所创建目录的权限，而不再使用默认权限。
-p 选项递归创建所有目录，以创建 /home/test/demo 为例，在默认情况下，你需要一层一层的创建各个目录，而使用 -p 选项，则系统会自动帮你创建 /home、/home/test 以及 /home/test/demo。

#### gtk
gtk(gimp toolkit)是GIMP 开发过程中用来管理图型界面的一套工具程序库
    

    
#### 并未安装ubuntu的图形界面，浅浅学习一下其中的bash命令
