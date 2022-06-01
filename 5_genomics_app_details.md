# Some command explains

## bash install_dep.sh
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
```
### brew tap命令

Taps(third-party-repositories)

brew tap可以为brew软件的跟踪,更新和安装添加更多的的tap formulae

如果你在核心仓库没有找到你需要的软件,那么你就需要安装第三方的仓库去安装你需要的软件

tap命令的仓库源默认来至于Github，但是这个命令也不限制于这一个地方
```bash

# brew unlink proj
brew install --force-bottle blast

echo "==> Custom tap"
brew tap wang-q/tap
brew install faops multiz sparsemem intspan
```
### brew install --force
'--force' 选项，移除之前缓存的版本，重新获取最新版本。
'--force-bottle' 选项，如果已经存在当前版本，仍旧下载。即使在安装过程中，还未使用。

# Config repeatmasker
```bash
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple h5py

cd $(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec
perl configure \
    -hmmer_dir=$(brew --prefix)/bin \
    -rmblast_dir=$(brew --prefix)/Cellar/rmblast@2.10.0/2.10.0/bin \
    -libdir=$(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec/Libraries \
    -trf_prgm=$(brew --prefix)/bin/trf \
    -default_search_engine=rmblast

cd -
```
### pip install
pip3 install -i <源地址> <包名>

源地址
阿里源：https://mirrors.aliyun.com/pypi/simple/ 
科大源：https://pypi.mirrors.ustc.edu.cn/simple/ 
清华源：https://pypi.tuna.tsinghua.edu.cn/simple 
官方源：https://pypi.python.org/

### brew --prefix
```bash
snwang@DESKTOP-T89G9DA:~$ brew --prefix
/home/linuxbrew/.linuxbrew
```
1、源码安装一般包括几个步骤：配置（configure），编译（make），安装（make install）。

2、其中configure是一个可执行脚本，在源码目录中执行可以完成自动的配置工作，即./configure。它有很多选项，在待安装的源码路径下使用命令./configure–help输出详细的选项列表。  
其中--prefix选项是配置安装的路径，如果不配置该选项，安装后可执行文件默认放在/usr/local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc，其它的资源文件放在/usr/local/share，比较凌乱。

3、在实际的安装过程中，我们可以增加--prefix参数，这样可以将要安装的应用安装到指定的目录中，如  
./configure --prefix=/usr/local/openssl  
可以把所有资源文件放在/usr/local/test的路径中，不会杂乱。

用了—prefix选项的另一个好处是卸载软件或移植软件。当某个安装的软件不再需要时，只须简单的删除该安装目录，就可以把软件卸载得干干净净；移植软件只需拷贝整个目录到另外一个机器即可（相同的操作系统）。

### 特殊符号（\）
放在一行指令的最末端，表示紧接着的回车无效（其实也就是转义了Enter），后继新行的输入仍然作为当前指令的一部分。

### cd -
回到上一个目录

```bash
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
```
### wget -N -P 
网页文件下载工具，可以将互联网上的数据保存到一个文件或展示在终端上。

参数：  
-N, –timestamping 不要重新下载文件除非比本地文件新  
-P, –directory-prefix=PREFIX 将文件保存到目录 PREFIX/

### pushd命令：cd命令增强版，实现多个目录之间切换
cd -命令能回到前一个目录，但无法实现多个目录之间的切换

pushd：切换到作为参数的目录，并把原目录和当前目录压入到一个虚拟的堆栈中；如果不指定参数，则会回到前一个目录，并把堆栈中最近的两个目录作交换
popd： 弹出堆栈中最近的目录
dirs: 列出当前堆栈中保存的目录列表

### MPI＝message passing interface：

在分布式内存（distributed-memory）之间实现信息通讯的一种 规范/标准/协议（standard）。它是一个库，不是一门语言。可以被fortran，c，c++等调用。

# bioinformatics softwares"

### clustal-w：多序列比对工具
### mafft：多序列比对工具
### muscle：多序列比对工具，其速度和准确度优于clustal
### trimal：比对序列修剪软件
trimAl输入文件必须是比对完成的序列文件,同时此软件支持多种输入文件格式，如clustal, fasta, NBRF/PIR, nexus, phylip3.2, phylip等。

修剪对齐完的比对序列构建的进化树更加精确。

### lastz: 两序列比对
### diamond: 蛋白数据库的比对
DIAMOND能够高效将翻译后的DNA序列或protein序列比对到参考蛋白质数据库。它基于C++语言编写，可实现多线程并行处理，这使得比对效率远远高于blast软件。在确定测序的reads来源于哪些微生物时，可通过将reads或reads组装后的contigs翻译后核苷酸序列或蛋白质序列比对到蛋白质参考数据库，如常用的NCBI的NR库（非冗余蛋白质数据库），进而找到数据中的微生物基因从而确定微生物。它有以下特点：

1. 比BLAST快100 - 20000倍（Pairwise alignment）
2. long reads的Frameshift alignments移位分析
3. 资源消耗小，服务器和普通电脑均可运行
4. 输出格式多样，包括BLAST比对格式

### paml: 系统发育分析
PAML（将最大似然法用于系统发育分析）是一个用最大似然法来对DNA和蛋白质序列进行系统发育分析的软件包。
### fasttree: 速度最快的最大似然法进化树构建软件
基于最大似然法构建进化树的软件，它最大的特点就是运行速度快，支持几百万条序列的建树任务。

### raxml:使用多线程或并行化使用最大似然法构建进化树。 
### newick-utils: 用于处理系统发育树的unix shell工具
其功能包括重置根（re-root）、提取子树、修剪、合并枝以及可视化。
### bowtie:将测序reads与长参考序列比对工具。
适用于将长度大约为50到100或1000字符的reads与相对较长的基因组（如哺乳动物）进行比对。
### bwa: mapping到参考基因组上的软件
WA 全名 Burrow-Wheeler Aligner

BWA是一款将DNA序列mapping到参考基因组上的软件，例如比对到人类基因组。其由三个算法组成BWA-backtrack，BWA-SW和BWA-MEM。在该软件作者的github上可以看到对三个算法的不同用处的解释https://github.com/lh3/bwa 
### samtools: 用于操作sam和bam文件的工具合集
能够实现二进制查看、格式转换、排序及合并等功能，结合sam格式中的flag、tag等信息，还可以完成比对结果的统计汇总。
### stringtie: 转录本组装和定量工具.  
它的输入不仅包括其他转录本汇编程序也可以使用短读序列的对比。为了在实验之间鉴定差异表达的基因，可以使用Ballgown，Cuffdiff或其他（DESeq，edgeR等）专用软件来处理StringTie的输出。
### hisat2: 基因组比对工具
由于测序仪机器读长的限制，在构建文库的过程中首先需要将DNA片段化，测序得到的序列只是基因组上的部分序列。为了确定测序reads在基因组上的位置，需要将reads比对回参考基因组上，这个步骤叫做mapping。
### tophat: 转录组比对软件
将RNA-Seq数据允许gap的回贴回参考基因组上 
### cufflinks: Python可视化工具包

### seqtk：序列处理工具
处理FASTA或FASTQ格式的序列。它可以无缝解析FASTQ和FASTA文件，也可以选择使用gzip对其进行压缩。

faops参考了该工具
### minimap2：长reads比对工具

### minigraph：泛基因组构建工具
基于图的数据模型和相关格式来表示多个基因组，同时保留线性参考基因组的坐标。

可以有效地构建泛基因组图并紧凑地编码当前参考基因组中缺失的数万个结构变体。  
### gfatools：处理GFA和rGFA格式的序列图的工具

### genometools：分析基因组数据的Python类，函数和脚本的集合

### igvtools：NGS数据可视化工具
可以展示序列比对，拷贝数变异，突变位点等多种数据的分布
### canu：三代测序组装工具 
### fastqc：高通量测序数据的高级质控工具
输入FastQ，SAM，BAM文件，输出对测序数据评估的网页报告 
### picard-tools：高通量测序数据的(Java) 工具箱
用于处理高通量排序（HTS）数据和格式，例如SAM / BAM / CRAM和VCF
### kat：测序数据简单质控
包含多个工具来帮助用户通过使用k-mer对测序数据进行简单分析，如组装完整性、测序错误、是否有污染等。

k-mer是一段长度为k的序列，而后面的mer即为monomeric unit(单体单元)，也就是每个碱基。通过去掉低频率的k-mer能够使得组装结果更加准确
### snp-sites：FASTA格式转换
将FASTA全基因组比对文件转换为VCF格式，用于后续建树 
### bcftools：操作vcf和BCF文件的工具合集
用于处理vcf(variant call format)文件和bcf(binary call format)文件。前者为文本文件，后者为其二进制文件。最主要的命令是view命令来进行SNP和Indel calling。

### edirect：基因序列挖掘工具
不仅可以根据基因ID或者名称挖掘基因序列，而且也可以通过PMID号批量挖掘文献。  
另外SRA高通量测序数据的注释信息也可以通过这个软件挖掘
### sratoolkit
sratoolkit 是NCBI提供的用于处理来自SRA数据库测序数据的一个工具包。

sratoolkit可以将.sra文件转换为 .fstaq.gz文件的工具。

注：sra是二进制文件，在Linux下如果用less去查看，它会显示这是个二进制文件，你是否确定打开它。一般我们分析测序数据，是用fastq文件打开分析，所以就需要转格式。

此外，可以利用prefetch命令从NCBI网站下载SRA accession no.的列表文件

注：SRA是NIH的高通量测序数据的主要档案，存档来自各种高通量测序平台的原始测序数据和比对信息，比如Illumina。

# less used
### augustus：真核生物基因结构预测工具
Augustus是一个基因预测工具，基于HMM和Bayes，通过已有物种的注释信息对软件进行训练，也可以整合EST, cDNA, RNA-seq数据作为先验模型进行预测。 
### prodigal：原核基因预测工具

### megahit：多快好省的宏基因组装工具
①MEGAHIT是超快的宏基因组序列组装工具，尤其适合组装超大规模数据；
②与SPAdes和IDBA-UD相比，计算时间和内存消耗方面优势巨大；
③在同类软件评估中，MEGAHIT通常有着最少的计算时间和N50，同时也拥有最低的嵌合体比例；
④软件安装方便，参数简单，可通过调整k-mer范围和步长控制分析质量和计算时间的不同要求；
⑤尤其在土壤等复杂环境样本组装、大量样本混合组装方面优势明显，成为行业的主流组装软件。

### spades：一般用于小基因组组装
一款de novo基因组组装软件， 适用于细菌/真菌等小型基因组的组装，不推荐用于动植物基因组的
### sga：基因组的组装工具
SGA (String Graph Assembler), 采用 String Grapher 的方法来进行基因组的组装。软件通过创建 FM-index/Burrows-Wheeler 索引来进行查找 short reads 之间的 overlaps， 从而进行基因组组装。
### quast：评估基因组组装效果
QUAST代表“质量评估工具”。 它通过计算各种指标来评估基因组/元基因组装配。 当前的QUAST工具包包括用于基因组装配的通用QUAST工具，MetaQUAST（用于宏基因组数据集的扩展），QUAST-LG（用于大型基因组（例如哺乳动物）的扩展）以及Icarus（用于这些工具的交互式可视化工具）。
QUAST软件包在有或没有参考基因组的情况下均可工作。 但是，如果至少有一个紧密的参考基因组与装配体一起提供，则将提供更多信息。 该工具可以接
### ntcard：基因组数据k-mer估算工具
### gatk：二代重测序数据分析软件
是基因分析的工具集。主要用于去除重复序列、重新校正碱基质量值、变异检查等。
### freebayes：snp calling 软件
使用freebayes软件很简单，输入原始的DNA序列和排序后的bam文件，即可得到检测结果。

### circos：关系可视化圈图表达软件
常被用来绘制环状的细菌染色体或质粒，或作为可视化比较基因组学研究多个细菌基因组之间差异的工具。
### hmmer：blast的替代品
也是一款用于寻找同源序列的软件
### rmblast：基因组重复序列检测
### trf: 是一款串联重复序列查找工具
Tandem Repeats Finder, 简称TRF
### repeatmasker：重复序列预测工具

# Config repeatmasker
### h5py：Python 中操作和使用HDF5 数据的工具库
一个HDF5文件是一种存放两类对象的容器：dataset和group. Dataset是类似于数组的数据集，而group是类似文件夹一样的容器，存放dataset和其他group。在使用h5py的时候需要牢记一句话：groups类比词典，dataset类比Numpy中的数组。 HDF5的dataset虽然与Numpy的数组在接口上很相近，但是支持更多对外透明的存储特征，如数据压缩，误差检测，分块传输。

## Reference:
configure --prefix: https://blog.csdn.net/qq_39715000/article/details/125022415
MPI: https://www.jianshu.com/p/a806022fbd6b
diamond: https://zouhua.top/archives/504431cd.html
h5py：https://blog.csdn.net/weixin_36670529/article/details/103789245。
