## bash基本命令解释（1-apt）
   
```
#!/usr/bin/env bash  
```
设置Bash解释器运行环境

```
echo "====> Install softwares via apt-get <===="
```
输出""中的内容

```
echo "==> Disabling the release upgrader"
sudo sed -i.bak 's/^Prompt=.*$/Prompt=never/' /etc/update-manager/release-upgrades
```

设置ubuntu的系统升级Prompt=never

将etc下配置文件release-upgrades中Prompt=以外的字符串替换为Prompt=never，并保存

同时会以release-upgrades.bak文件备份原来未修改文件内容，以确保原始文件内容安全性，防止错误操作而无法恢复原来内容。

###### 背景
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


's/代表是替换


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

if [ ! -e /etc/apt/sources.list.bak ]; then
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
fi
sudo mv list.tmp /etc/apt/sources.list

# Virtual machines needn't this and I want life easier.
# https://help.ubuntu.com/lts/serverguide/apparmor.html
if [ "$(whoami)" == 'vagrant' ]; then
    echo "==> Disable AppArmor"
    sudo service apparmor stop
    sudo update-rc.d -f apparmor remove
fi

echo "==> Disable whoopsie"
sudo sed -i 's/report_crashes=true/report_crashes=false/' /etc/default/whoopsie
sudo service whoopsie stop

echo "==> Install linuxbrew dependences"
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y install build-essential curl file git
sudo apt-get -y install libbz2-dev zlib1g-dev libzstd-dev
# sudo apt-get -y install libcurl4-openssl-dev libexpat-dev libncurses-dev

echo "==> Install other software"
sudo apt-get -y install aptitude parallel vim screen xsltproc numactl

echo "==> Install develop libraries"
# sudo apt-get -y install libreadline-dev libedit-dev
sudo apt-get -y install libdb-dev libxml2-dev libssl-dev libncurses5-dev # libgd-dev
# sudo apt-get -y install gdal-bin gdal-data libgdal-dev # /usr/lib/libgdal.so: undefined reference to `TIFFReadDirectory@LIBTIFF_4.0'
# sudo apt-get -y install libgsl0ldbl libgsl0-dev

# Gtk stuff, Need by alignDB
# install them in a fresh machine to avoid problems
echo "==> Install gtk3"
sudo apt-get -y install libcairo2-dev libglib2.0-0 libglib2.0-dev libgtk-3-dev libgirepository1.0-dev
sudo apt-get -y install gir1.2-glib-2.0 gir1.2-gtk-3.0 gir1.2-webkit-3.0

echo "==> Install gtk3 related tools"
# sudo apt-get -y install xvfb glade

echo "==> Install graphics tools"
sudo apt-get -y install gnuplot graphviz imagemagick

#echo "==> Install nautilus plugins"
#sudo apt-get -y install nautilus-open-terminal nautilus-actions

# Mysql will be installed separately.
# Remove system provided mysql package to avoid confusing linuxbrew.
echo "==> Remove system provided mysql"
sudo apt-get -y purge mysql-common

echo "==> Restore original sources.list"
if [ -e /etc/apt/sources.list.bak ]; then
    sudo rm /etc/apt/sources.list
    sudo mv /etc/apt/sources.list.bak /etc/apt/sources.list
fi

echo "====> Basic software installation complete! <===="
