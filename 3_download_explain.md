## 新的学习又开始啦

```
#!/usr/bin/env bash  
```
设置Bash解释器运行环境

```
echo "====> Download Genomics related tools <===="

mkdir -p $HOME/bin
mkdir -p $HOME/.local/bin
mkdir -p $HOME/share
mkdir -p $HOME/Scripts 
```
在家目录下创建一些文件夹

```
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
确保$HOME/bin已经添加进环境变量的设置

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
1，$HOME这个代码是一个环境变量，它代表的是当前登录的用户的主文件夹的意思。（就是家目录的那个）

2，$HOME/bin这个代码指的就是主文件夹下的bin子目录，代表的是文件夹的内部子目录。（注意不是根目录的那个）

3，PATH=PATH:HOME/bin这个代码是设置PATH环境变量，就是设置环境变量用等号，首先:冒号是分割符。记得Windows上面也有PATH环境变量，Windows的路径之间的分隔符是;分号。

4，PATH:HOME/bin表示在保留原来的PATH环境变量的基础上，再增加HOME/bin这个路径作为新的$PATH环境变量。计算机中的变量有许多，主要应用于系统文件的管理方面。

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

# alignDB
# chmod +x $HOME/Scripts/alignDB/alignDB.pl
# ln -fs $HOME/Scripts/alignDB/alignDB.pl $HOME/bin/alignDB.pl

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

if [[ $(uname) == 'Darwin' ]]; then
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/macOSX.x86_64/faToTwoBit
else
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/faToTwoBit
fi \
    > faToTwoBit
mv faToTwoBit $HOME/bin/
chmod +x $HOME/bin/faToTwoBit
