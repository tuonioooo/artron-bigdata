# 目录

* [Introduce](README.md)
* [大数据](da-shu-ju.md)
* [Hadoop](hadoop.md)
* [Hbase](/hbase.md)
* [Hive](/hive.md)

### Hadoop {#hadoop}

Hadoop是什么？  
答：一个分布式系统基础架构。

Hadoop解决了什么问题？  
答：解决了大数据（大到一台计算机无法进行存储，一台计算机无法在要求的时间内进行处理）的可靠存储\(HDFS\)和处理\(MapReduce\)。

### Hive {#hive}

Hive是什么？  
答：Hive是建立在Hadoop之上的，使用Hadoop作为底层存储的批处理系统。（可以理解为MapReduce的一层壳）

Hive解决了什么问题？  
答：Hive是为了减少MapReduce jobs的编写工作。

### HBase {#hbase}

HBase是什么？  
答：HBase是一种Key/Value系统，它运行在HDFS之上。

HBase解决了什么问题？  
答：Hbase是为了解决Hadoop的实时性需求。

### Spark和Storm {#spark和storm}

是什么？  
答：Spark和Storm都是通用的并行计算框架。

解决了什么问题？  
答：解决Hadoop只适用于离线数据处理，而不能提供实时数据处理能力的问题。

