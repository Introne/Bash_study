# check gnu stow is installed
```bash
hash stow 2>/dev/null || {
    echo >&2 "GNU stow is required but it's not installed.";
    echo >&2 "Install with homebrew: brew install stow";
    exit 1;
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
 -p /usr/bin/openssl aliesopenssl 往hash表中添加一条，执行aliesopenssl即 执行openssl命令（起别名）

### stow管理软件包
是用于 Linux 的软件安装管理的实用程序，它许多地方都优于“久经考验”的 Red Hat 和 Debian 软件包管理系统。通过使用 Stow，可以将应用程序打包成标准的 tar 文件，并按照逻辑安排应用程序二进制文件，以易于访问。

软件安装管理在总体上描述了在系统上安装、卸载、更新和组织软件应用程序（或称为软件包）的活动。在这些活动中，组织应用程序是尤其重要的活动。如果应用程序组织得井井有条，那么在 Linux 机器上安装、升级和卸载应用程序会变得更加容易且更方便。

Stow 提供了一个特殊命令 stow，该命令可以与各种选项一起执行，以调用 Stow 进行软件安装管理。Stow 命令的常规语法如下：
$ stow [options] application-name

### 2> /dev/null
代表忽略掉错误提示信息

### exit 1

exit（0）：正常运行程序并退出程序；

exit（1）：非正常运行导致退出程序；

exit 0 可以告知你的程序的使用者：你的程序是正常结束的。如果 exit 非 0 值，那么你的程序的使用者通常会认为你的程序产生了一个错误。
在 shell 中调用完你的程序之后，用 echo $? 命令就可以看到你的程序的 exit 值。在 shell 脚本中，通常会根据上一个命令的 $? 值来进行一些流程控制。

# colors in term
# http://stackoverflow.com/questions/5947742/how-to-change-the-output-color-of-echo-in-linux
```bash
GREEN=
RED=
NC=
if tty -s < /dev/fd/1 2> /dev/null; then
  GREEN='\033[0;32m'
  RED='\033[0;31m'
  NC='\033[0m' # No Color
fi

log_warn () {
  echo -e "==> ${RED}$@${NC} <=="
}

log_info () {
  echo -e "==> ${GREEN}$@${NC}"
}

log_debug () {
  echo -e "==> $@"
}
```
### TTY
TTY 一词源自电传打字机的缩写

在 Linux 或 UNIX 中，TTY 变为了一个抽象设备。有时它指的是一个物理输入设备，例如串口，有时它指的是一个允许用户和系统交互的虚拟 TTY（参考此处）。

TTY 是 Linux 或 UNIX 的一个子系统，它通过 TTY 驱动程序在内核级别实现进程管理、行编辑和会话管理。

在大多数 发行版 中，可以使用以下键盘快捷键来得到 TTY 屏幕：

CTRL + ALT + F1 – 锁屏
CTRL + ALT + F2 – 桌面环境
CTRL + ALT + F3 – TTY3
CTRL + ALT + F4 – TTY4
CTRL + ALT + F5 – TTY5
CTRL + ALT + F6 – TTY6
你最多可以访问六个 TTY。但是，前两个快捷方式指向发行版的锁定屏幕和桌面环境。

### tty命令
功能：返回连接到当前标准输入的终端设备文件名。
参数：
‘-s’‘--silent’‘--quiet’           Print nothing; only return an exit status.

```bash
 snwang@DESKTOP-T89G9DA:~$ tty
/dev/pts/2
```
tty[1-6]就是用CTRL + ALT + F[1-6]所看到的那个终端; 即虚拟控制台。其他的是外部终端和网络终端。

pts/*为伪(虚拟)终端, 其中pts/0,1,2在桌面Linux中是标准输入，标准输出，标准出错。


# enter BASE_DIR
BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )
cd "${BASE_DIR}" || exit


### &&
当测试条件两个都为真时返回真(0)，有假为假。

# stow configurations
mkdir -p ~/.config

log_warn "Restow dotfiles"
DIRS=( stow-git stow-htop stow-latexmk stow-perltidy stow-screen stow-wget stow-vim stow-proxychains )

for d in ${DIRS[@]}; do
    log_info "${d}"
    for f in $(find ${d} -maxdepth 1 | cut -sd / -f 2- | grep .); do
        homef="${HOME}/${f}"
        if [ -h "${homef}" ] ; then
            log_debug "Symlink exists: [${homef}]"
        elif [ -f "${homef}" ] ; then
            log_debug "Delete file: [${homef}]"
            rm "${homef}"
        elif [ -d "${homef}" ] ; then
            log_debug "Delete dir: [${homef}]"
            rm -fr "${homef}"
        fi
    done
	stow -t "${HOME}" "${d}" -v 2
done

# don't ruin Ubuntu
log_info "stwo-bash"
stow -t "${HOME}" stow-bash -v 2

# don't ruin Ubuntu
log_info "stow-htop2 to .config"
stow -t "${HOME}"/.config/htop stow-htop2 -v 2


Reference
exit 1：https://blog.csdn.net/super_gnu/article/details/77099395
