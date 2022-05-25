## 
```bash
#curl -fsSL https://raw.githubusercontent.com/wang-q/App-Anchr/master/share/install_dep.sh | bash
```
### curl -fsSL

curl是一种数据传输程序，使用其中一种支持的协议（DICT，FILE，FTP，FTPS，GOPHER，HTTP，HTTPS，IMAP，IMAPS，LDAP，LDAPS，POP3，POP3S，RTMP，RTSP，SCP，SFTP，SMTP，SMTPS，TELNET和TFTP）从服务器或服务器传输数据的工具。该命令旨在无需用户交互即可工作。

参数：
-f/--fail                      连接失败时不显示http错误
-s/--silent                    静音模式。不输出任何东西
-S/--show-error                显示错误(该参数是否与-s重复)
-L/--location                  跟踪重定向

## 安装生信softwares
```bash
echo "==> Install bioinformatics softwares"
brew tap brewsci/bio
brew tap brewsci/science

brew install clustal-w mafft muscle trimal
brew install lastz diamond paml fasttree # raxml
brew install raxml --without-open-mpi
brew install --force-bottle newick-utils
brew install bowtie bowtie2 bwa samtools
brew install stringtie hisat2 # tophat cufflinks
brew install seqtk minimap2 minigraph # gfatools
brew install genometools # igvtools
brew install canu fastqc picard-tools kat
brew install --build-from-source snp-sites # macOS bottles broken
brew install bcftools

brew install edirect
brew install sratoolkit
```

# less used
brew install augustus prodigal
brew install megahit spades sga
brew install quast
brew install ntcard
brew install gatk freebayes

# brew unlink proj
brew install --force-bottle blast

echo "==> Custom tap"
brew tap wang-q/tap
brew install faops multiz sparsemem intspan
# brew install jrunlist jrange

echo "==> circos"
brew install wang-q/tap/circos@0.69.9

echo "==> RepeatMasker"
brew install hmmer easel

brew install wang-q/tap/rmblast@2.10.0
# brew link rmblast@2.10.0 --overwrite

brew install wang-q/tap/trf@4

brew install wang-q/tap/repeatmasker@4.1.1

# Config repeatmasker
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple h5py

cd $(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec
perl configure \
    -hmmer_dir=$(brew --prefix)/bin \
    -rmblast_dir=$(brew --prefix)/Cellar/rmblast@2.10.0/2.10.0/bin \
    -libdir=$(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec/Libraries \
    -trf_prgm=$(brew --prefix)/bin/trf \
    -default_search_engine=rmblast

cd -

#echo "==> Config repeatmasker"
#wget -N -P /tmp https://github.com/egateam/egavm/releases/download/20170907/repeatmaskerlibraries-20140131.tar.gz
#
#pushd $(brew --prefix)/opt/repeatmasker/libexec
#rm -fr lib/perl5/x86_64-linux-thread-multi/
#rm Libraries/RepeatMasker.lib*
#rm Libraries/DfamConsensus.embl
#tar zxvf /tmp/repeatmaskerlibraries-20140131.tar.gz
#sed -i".bak" 's/\/usr\/bin\/perl/env/' configure.input
#./configure < configure.input
#popd
