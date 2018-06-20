# Hive

hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行。 其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。

## Hive定义

Hive是建立在 Hadoop 上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载（ETL），这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制。Hive 定义了简单的类 SQL 查询语言，称为 HQL，它允许熟悉 SQL 的用户查询数据。同时，这个语言也允许熟悉 MapReduce 开发者的开发自定义的 mapper 和 reducer 来处理内建的 mapper 和 reducer 无法完成的复杂的分析工作。

Hive 没有专门的数据格式。 Hive 可以很好的工作在 Thrift 之上，控制分隔符，也允许用户指定数据格式。

## 适用场景

Hive 构建在基于静态批处理的Hadoop 之上，Hadoop 通常都有较高的延迟并且在作业提交和调度的时候需要大量的开销。因此，Hive 并不能够在大规模数据集上实现低延迟快速的查询，例如，Hive 在几百MB 的数据集上执行查询一般有分钟级的时间延迟。

因此，  
Hive 并不适合那些需要低延迟的应用，例如，联机事务处理（OLTP）。Hive 查询操作过程严格遵守Hadoop MapReduce 的作业执行模型，Hive 将用户的HiveQL 语句通过解释器转换为MapReduce 作业提交到Hadoop 集群上，Hadoop 监控作业执行过程，然后返回作业执行结果给用户。Hive 并非为联机事务处理而设计，Hive 并不提供实时的查询和基于行级的数据更新操作。Hive 的最佳使用场合是大数据集的批处理作业，例如，网络日志分析。

## 设计特征

Hive 是一种底层封装了Hadoop 的数据仓库处理工具，使用类SQL 的HiveQL 语言实现数据查询，所有Hive 的数据都存储在Hadoop 兼容的文件系统（例如，Amazon S3、HDFS）中。Hive 在加载数据过程中不会对数据进行任何的修改，只是将数据移动到HDFS 中Hive 设定的目录下，因此，Hive 不支持对数据的改写和添加，所有的数据都是在加载的时候确定的。Hive 的设计特点如下。

● 支持索引，加快数据查询。

● 不同的存储类型，例如，纯文本文件、HBase 中的文件。

● 将元数据保存在关系数据库中，大大减少了在查询过程中执行语义检查的时间。

● 可以直接使用存储在Hadoop 文件系统中的数据。

● 内置大量用户函数UDF 来操作时间、字符串和其他的数据挖掘工具，支持用户扩展UDF 函数来完成内置函数无法实现的操作。

● 类SQL 的查询方式，将SQL 查询转换为MapReduce 的job 在Hadoop集群上执行。

## Hive 体系结构

主要分为以下几个部分：

**用户接口**

用户接口主要有三个：CLI，Client 和 WUI。其中最常用的是 CLI，Cli 启动的时候，会同时启动一个 Hive 副本。Client 是 Hive 的客户端，用户连接至 Hive Server。在启动 Client 模式的时候，需要指出 Hive Server 所在节点，并且在该节点启动 Hive Server。 WUI 是通过浏览器访问 Hive。

**元数据存储**

Hive 将元数据存储在数据库中，如 mysql、derby。Hive 中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等。

**解释器、编译器、优化器、执行器  
**

解释器、编译器、优化器完成 HQL 查询语句从词法分析、语法分析、编译、优化以及查询计划的生成。生成的查询计划存储在 HDFS 中，并在随后由 MapReduce 调用执行。

**Hadoop**

Hive 的数据存储在 HDFS 中，大部分的查询由 MapReduce 完成（包含 \* 的查询，比如 select \* from tbl 不会生成 MapReduce 任务）。

## 数据存储

首先，Hive 没有专门的数据存储格式，也没有为数据建立索引，用户可以非常自由的组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符和行分隔符，Hive 就可以解析数据。

其次，Hive 中所有的数据都存储在 HDFS 中，Hive 中包含以下数据模型：表\(Table\)，外部表\(External Table\)，分区\(Partition\)，桶\(Bucket\)。

Hive 中的 Table 和数据库中的 Table 在概念上是类似的，每一个 Table 在 Hive 中都有一个相应的目录存储数据。例如，一个表 pvs，它在 HDFS 中的路径为：/wh/pvs，其中，wh 是在 hive-site.xml 中由 ${hive.metastore.warehouse.dir} 指定的数据仓库的目录，所有的 Table 数据（不包括 External Table）都保存在这个目录中。

Partition 对应于数据库中的 Partition 列的密集索引，但是 Hive 中 Partition 的组织方式和数据库中的很不相同。在 Hive 中，表中的一个 Partition 对应于表下的一个目录，所有的 Partition 的数据都存储在对应的目录中。例如：pvs 表中包含 ds 和 city 两个 Partition，则对应于 ds = 20090801, ctry = US 的 HDFS 子目录为：/wh/pvs/ds=20090801/ctry=US；对应于 ds = 20090801, ctry = CA 的 HDFS 子目录为；/wh/pvs/ds=20090801/ctry=CA

Buckets 对指定列计算 hash，根据 hash 值切分数据，目的是为了并行，每一个 Bucket 对应一个文件。将 user 列分散至 32 个 bucket，首先对 user 列的值计算 hash，对应 hash 值为 0 的 HDFS 目录为：/wh/pvs/ds=20090801/ctry=US/part-00000；hash 值为 20 的 HDFS 目录为：/wh/pvs/ds=20090801/ctry=US/part-00020

External Table 指向已经在 HDFS 中存在的数据，可以创建 Partition。它和 Table 在元数据的组织上是相同的，而实际数据的存储则有较大的差异。

Table 的创建过程和数据加载过程（这两个过程可以在同一个语句中完成），在加载数据的过程中，实际数据会被移动到数据仓库目录中；之后对数据对访问将会直接在数据仓库目录中完成。删除表时，表中的数据和元数据将会被同时删除。

* External Table 只有一个过程，加载数据和创建表同时完成（CREATE EXTERNAL TABLE ……LOCATION），实际数据是存储在 LOCATION 后面指定的 HDFS 路径中，并不会移动到数据仓库目录中。当删除一个 External Table 时，仅删除元数据，表中的数据不会真正被删除。

## 安装配置

你可以下载一个已打包好的hive稳定版，也可以下载源码自己build一个版本。

**安装需要**

1. java 1.6，java 1.7或更高版本。
2. Hadoop 2.x或更高, 1.x. Hive 0.13 版本也支持 0.20.x, 0.23.x
3. Linux,mac,windows操作系统。以下内容适用于linux系统。

**安装打包好的hive**

需要先到apache下载已打包好的hive镜像，然后解压开该文件

| 1 | `$ tar-xzvf hive-x.y.z.tar.gz` |
| :--- | :--- |


设置hive环境变量

| 1 | `$ cdhive-x.y.z$ exportHIVE_HOME={{pwd}}` |
| :--- | :--- |


设置hive运行路径

| 1 | `$ exportPATH=$HIVE_HOME/bin:$PATH` |
| :--- | :--- |


**编译Hive源码**

下载hive源码

此处使用maven编译，需要下载安装maven。

[![](http://f.hiphotos.baidu.com/baike/s%3D250/sign=1c80bf9775094b36df921ce893cd7c00/0823dd54564e9258fc24c7a69f82d158ccbf4e61.jpg)](https://baike.baidu.com/pic/hive/67986/0/0823dd54564e9258fc24c7a69f82d158ccbf4e61?fr=lemma&ct=single)

以Hive 0.13版为例

1. 编译hive 0.13源码基于hadoop 0.23或更高版本
   $cdhive$mvncleaninstall-Phadoop-2,dist$cdpackaging/target/apache-hive-{version}-SNAPSHOT-bin/apache-hive-{version}-SNAPSHOT-bin$lsLICENSENOTICEREADME.txtRELEASE\_NOTES.txtbin/\(alltheshellscripts\)lib/\(requiredjarfiles\)conf/\(configurationfiles\)examples/\(sampleinputandqueryfiles\)hcatalog/\(hcataloginstallation\)scripts/\(upgradescriptsforhive-metastore\)
2. 编译hive 基于hadoop 0.20
   $cdhive$antcleanpackage$cdbuild/dist\#lsLICENSENOTICEREADME.txtRELEASE\_NOTES.txtbin/\(alltheshellscripts\)lib/\(requiredjarfiles\)conf/\(configurationfiles\)examples/\(sampleinputandqueryfiles\)hcatalog/\(hcataloginstallation\)scripts/\(upgradescriptsforhive-metastore\)

**运行hive**

Hive运行依赖于hadoop，在运行hadoop之前必需先配置好hadoopHome。

| 1 | `exportHADOOP_HOME=<hadoop-install-dir>` |
| :--- | :--- |


在hdfs上为hive创建\tmp目录和/user/hive/warehouse\(akahive.metastore.warehouse.dir\) 目录，然后你才可以运行hive。

在运行hive之前设置HiveHome。

| 1 | `$ exportHIVE_HOME=<hive-install-dir>` |
| :--- | :--- |


在命令行窗口启动hive

| 1 | `$ $HIVE_HOME/bin/hive` |
| :--- | :--- |


若执行成功，将看到类似内容如图所示



## 基本语法

基本数据类型

hive支持多种不同长度的整型和浮点型数据，支持布尔型，也支持无长度限制的字符串类型。例如：TINYINT、SMALINT、BOOLEAN、FLOAT、DOUBLE、STRING等基本数据类型。这些基本数据类型和其他sql方言一样，都是保留字。

集合数据类型

hive中的列支持使用struct、map和array集合数据类型。大多数关系型数据库中不支持这些集合数据类型，因为它们会破坏标准格式。关系型数据库中为实现集合数据类型是由多个表之间建立合适的外键关联来实现。在大数据系统中，使用集合类型的数据的好处在于提高数据的吞吐量，减少寻址次数来提高查询速度。

使用集合数据类型创建表实例：

CREATE TABLE STUDENTINFO

\(

NAME STRING,

FAVORITE ARRAY&lt;STRING&gt;,

COURSE MAP&lt;STRING,FLOAT&gt;,

ADDRESS STRUCT&lt;CITY:STRING,STREET:STRING&gt;

\)

查询语法：SELECT S.NAME,S.FAVORITE\[0\],S.COURSE\["ENGLISH"\],S.ADDRESS.CITY FROM STUDENTINFO S;

分区表

创建分区表：create table employee \(name string,age int,sex string\) partitioned by \(city string\) row format delimited fields terminated by '\t';

分区表装载数据：load data local inpath '/usr/local/lee/employee' into table employee partition \(city='hubei'\);



## Hive常用优化方法

1、join连接时的优化：当三个或多个以上的表进行join操作时，如果每个on使用相同的字段连接时只会产生一个mapreduce。

2、join连接时的优化：当多个表进行查询时，从左到右表的大小顺序应该是从小到大。原因：hive在对每行记录操作时会把其他表先缓存起来，直到扫描最后的表进行计算

3、在where字句中增加分区过滤器。

4、当可以使用left semi join 语法时不要使用inner join，前者效率更高。原因：对于左表中指定的一条记录，一旦在右表中找到立即停止扫描。

5、如果所有表中有一张表足够小，则可置于内存中，这样在和其他表进行连接的时候就能完成匹配，省略掉reduce过程。设置属性即可实现，set hive.auto.covert.join=true; 用户可以配置希望被优化的小表的大小 set hive.mapjoin.smalltable.size=2500000; 如果需要使用这两个配置可置入$HOME/.hiverc文件中。

6、同一种数据的多种处理：从一个数据源产生的多个数据聚合，无需每次聚合都需要重新扫描一次。

例如：insert overwrite table student select \*　from employee; insert overwrite table person select \* from employee;

可以优化成：from employee insert overwrite table student select \* insert overwrite table person select \*

7、limit调优：limit语句通常是执行整个语句后返回部分结果。set hive.limit.optimize.enable=true;

8、开启并发执行。某个job任务中可能包含众多的阶段，其中某些阶段没有依赖关系可以并发执行，开启并发执行后job任务可以更快的完成。设置属性：set hive.exec.parallel=true;

9、hive提供的严格模式，禁止3种情况下的查询模式。

a：当表为分区表时，where字句后没有分区字段和限制时，不允许执行。

b：当使用order by语句时，必须使用limit字段，因为order by 只会产生一个reduce任务。

c：限制笛卡尔积的查询。

10、合理的设置map和reduce数量。

11、jvm重用。可在hadoop的mapred-site.xml中设置jvm被重用的次数。



