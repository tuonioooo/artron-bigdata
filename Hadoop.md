## 什么是Hadoop

**Hadoop**，是一个由Apache基金会所开发的分布式系统基础架构。 用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。

Hadoop实现了一个分布式文件系统（Hadoop Distributed File System），简称HDFS。HDFS有高容错性的特点，并且设计用来部署在低廉的（low-cost）硬件上；而且它提供高吞吐量（high throughput）来访问应用程序的数据，适合那些有着超大数据集（large data set）的应用程序。HDFS放宽了（relax）POSIX的要求，可以以流的形式访问（streaming access）文件系统中的数据。

Hadoop的框架最核心的设计就是：HDFS和MapReduce。HDFS为海量的数据提供了存储，则MapReduce为海量的数据提供了计算。

## Hadoop起源

### 项目起源

Hadoop由 Apache Software Foundation 公司于 2005 年秋天作为Lucene的子项目Nutch的一部分正式引入。它受到最先由 Google Lab 开发的 Map/Reduce 和 Google File System(GFS) 的启发。

2006 年 3 月份，Map/Reduce 和 Nutch Distributed File System (NDFS) 分别被纳入称为 Hadoop 的项目中。

Hadoop 是最受欢迎的在 Internet 上对搜索关键字进行内容分类的工具，但它也可以解决许多要求极大伸缩性的问题。例如，如果您要 grep 一个 10TB 的巨型文件，会出现什么情况？在传统的系统上，这将需要很长的时间。但是 Hadoop 在设计时就考虑到这些问题，采用并行执行机制，因此能大大提高效率。

### 发展历程

Hadoop原本来自于谷歌一款名为MapReduce的编程模型包。谷歌的MapReduce框架可以把一个应用程序分解为许多并行计算指令，跨大量的计算节点运行非常巨大的数据集。使用该框架的一个典型例子就是在网络数据上运行的搜索算法。Hadoop[3] 最初只与网页索引有关，迅速发展成为分析大数据的领先平台。

目前有很多公司开始提供基于Hadoop的商业软件、支持、服务以及培训。Cloudera是一家美国的企业软件公司，该公司在2008年开始提供基于Hadoop的软件和服务。GoGrid是一家云计算基础设施公司，在2012年，该公司与Cloudera合作加速了企业采纳基于Hadoop应用的步伐。Dataguise公司是一家数据安全公司，同样在2012年该公司推出了一款针对Hadoop的数据保护和风险评估。

### 名字起源

Hadoop这个名字不是一个缩写，而是一个虚构的名字。该项目的创建者，Doug Cutting解释Hadoop的得名 ：“这个名字是我孩子给一个棕黄色的大象玩具命名的。我的命名标准就是简短，容易发音和拼写，没有太多的意义，并且不会被用于别处。小孩子恰恰是这方面的高手。”

## Hadoop优点

Hadoop是一个能够对大量数据进行分布式处理的软件框架。 Hadoop 以一种可靠、高效、可伸缩的方式进行数据处理。

Hadoop 是可靠的，因为它假设计算元素和存储会失败，因此它维护多个工作数据副本，确保能够针对失败的节点重新分布处理。

Hadoop 是高效的，因为它以并行的方式工作，通过并行处理加快处理速度。

Hadoop 还是可伸缩的，能够处理 PB 级数据。

此外，Hadoop 依赖于社区服务，因此它的成本比较低，任何人都可以使用。

Hadoop是一个能够让用户轻松架构和使用的分布式计算平台。用户可以轻松地在Hadoop上开发和运行处理海量数据的应用程序。它主要有以下几个优点：

高可靠性。Hadoop按位存储和处理数据的能力值得人们信赖。

高扩展性。Hadoop是在可用的计算机集簇间分配数据并完成计算任务的，这些集簇可以方便地扩展到数以千计的节点中。

高效性。Hadoop能够在节点之间动态地移动数据，并保证各个节点的动态平衡，因此处理速度非常快。

高容错性。Hadoop能够自动保存数据的多个副本，并且能够自动将失败的任务重新分配。

低成本。与一体机、商用数据仓库以及QlikView、Yonghong Z-Suite等数据集市相比，hadoop是开源的，项目的软件成本因此会大大降低。

Hadoop带有用Java语言编写的框架，因此运行在 Linux 生产平台上是非常理想的。Hadoop 上的应用程序也可以使用其他语言编写，比如 C++。

## Hadoop大数据处理的意义

Hadoop得以在大数据处理应用中广泛应用得益于其自身在数据提取、变形和加载(ETL)方面上的天然优势。Hadoop的分布式架构，将大数据处理引擎尽可能的靠近存储，对例如像ETL这样的批处理操作相对合适，因为类似这样操作的批处理结果可以直接走向存储。Hadoop的MapReduce功能实现了将单个任务打碎，并将碎片任务(Map)发送到多个节点上，之后再以单个数据集的形式加载(Reduce)到数据仓库里

## Hadoop核心架构

Hadoop 由许多元素构成。其最底部是 Hadoop Distributed File System（HDFS），它存储 Hadoop 集群中所有存储节点上的文件。HDFS（对于本文）的上一层是MapReduce 引擎，该引擎由 JobTrackers 和 TaskTrackers 组成。通过对Hadoop分布式计算平台最核心的分布式文件系统HDFS、MapReduce处理过程，以及数据仓库工具Hive和分布式数据库Hbase的介绍，基本涵盖了Hadoop分布式平台的所有技术核心。

　　HDFS

　　对外部客户机而言，HDFS就像一个传统的分级文件系统。可以创建、删除、移动或重命名文件，等等。但是 HDFS 的架构是基于一组特定的节点构建的（参见图 1），这是由它自身的特点决定的。这些节点包括 NameNode（仅一个），它在 HDFS 内部提供元数据服务；DataNode，它为 HDFS 提供存储块。由于仅存在一个 NameNode，因此这是 HDFS 的一个缺点（单点失败）。

　　存储在 HDFS 中的文件被分成块，然后将这些块复制到多个计算机中（DataNode）。这与传统的 RAID 架构大不相同。块的大小（通常为 64MB）和复制的块数量在创建文件时由客户机决定。NameNode 可以控制所有文件操作。HDFS 内部的所有通信都基于标准的 TCP/IP 协议。

　　NameNode

　　NameNode 是一个通常在 HDFS 实例中的单独机器上运行的软件。它负责管理文件系统名称空间和控制外部客户机的访问。NameNode 决定是否将文件映射到 DataNode 上的复制块上。对于最常见的 3 个复制块，第一个复制块存储在同一机架的不同节点上，最后一个复制块存储在不同机架的某个节点上。注意，这里需要您了解集群架构。

　　实际的 I/O事务并没有经过 NameNode，只有表示 DataNode 和块的文件映射的元数据经过 NameNode。当外部客户机发送请求要求创建文件时，NameNode 会以块标识和该块的第一个副本的 DataNode IP 地址作为响应。这个 NameNode 还会通知其他将要接收该块的副本的 DataNode。

　　NameNode 在一个称为 FsImage 的文件中存储所有关于文件系统名称空间的信息。这个文件和一个包含所有事务的记录文件（这里是 EditLog）将存储在 NameNode 的本地文件系统上。FsImage 和 EditLog 文件也需要复制副本，以防文件损坏或 NameNode 系统丢失。

　　NameNode本身不可避免地具有SPOF（Single Point Of Failure）单点失效的风险，主备模式并不能解决这个问题，通过Hadoop Non-stop namenode才能实现100% uptime可用时间。

　　DataNode

　　DataNode 也是一个通常在 HDFS实例中的单独机器上运行的软件。Hadoop 集群包含一个 NameNode 和大量 DataNode。DataNode 通常以机架的形式组织，机架通过一个交换机将所有系统连接起来。Hadoop 的一个假设是：机架内部节点之间的传输速度快于机架间节点的传输速度。

　　DataNode 响应来自 HDFS 客户机的读写请求。它们还响应来自 NameNode 的创建、删除和复制块的命令。NameNode 依赖来自每个 DataNode 的定期心跳（heartbeat）消息。每条消息都包含一个块报告，NameNode 可以根据这个报告验证块映射和其他文件系统元数据。如果 DataNode 不能发送心跳消息，NameNode 将采取修复措施，重新复制在该节点上丢失的块。

　　文件操作

　　可见，HDFS 并不是一个万能的文件系统。它的主要目的是支持以流的形式访问写入的大型文件。

　　如果客户机想将文件写到 HDFS 上，首先需要将该文件缓存到本地的临时存储。如果缓存的数据大于所需的 HDFS 块大小，创建文件的请求将发送给 NameNode。NameNode 将以 DataNode 标识和目标块响应客户机。

　　同时也通知将要保存文件块副本的 DataNode。当客户机开始将临时文件发送给第一个 DataNode 时，将立即通过管道方式将块内容转发给副本 DataNode。客户机也负责创建保存在相同 HDFS名称空间中的校验和（checksum）文件。

　　在最后的文件块发送之后，NameNode 将文件创建提交到它的持久化元数据存储（在 EditLog 和 FsImage 文件）。

　　Linux 集群

　　Hadoop 框架可在单一的 Linux 平台上使用（开发和调试时），官方提供MiniCluster作为单元测试使用，不过使用存放在机架上的商业服务器才能发挥它的力量。这些机架组成一个 Hadoop 集群。它通过集群拓扑知识决定如何在整个集群中分配作业和文件。Hadoop 假定节点可能失败，因此采用本机方法处理单个计算机甚至所有机架的失败。

### Hadoop和高效能计算、网格计算的区别

在Hadoop 出现之前，高性能计算和网格计算一直是处理大数据问题主要的使用方法和工具，它们主要采用消息传递接口（Message Passing Interface，MPI）提供的API 来处理大数据。高性能计算的思想是将计算作业分散到集群机器上，集群计算节点访问存储区域网络SAN 构成的共享文件系统获取数据，这种设计比较适合计算密集型作业。当需要访问像PB 级别的数据的时候，由于存储设备网络带宽的限制，很多集群计算节点只能空闲等待数据。而Hadoop却不存在这种问题，由于Hadoop 使用专门为分布式计算设计的文件系统HDFS，计算的时候只需要将计算代码推送到存储节点上，即可在存储节点上完成数据本地化计算，Hadoop 中的集群存储节点也是计算节点。在分布式编程方面，MPI 是属于比较底层的开发库，它赋予了程序员极大的控制能力，但是却要程序员自己控制程序的执行流程，容错功能，甚至底层的套接字通信、数据分析算法等底层细节都需要自己编程实现。这种要求无疑对开发分布式程序的程序员提出了较高的要求。相反，Hadoop 的MapReduce 却是一个高度抽象的并行编程模型，它将分布式并行编程抽象为两个原语操作，即map 操作和reduce 操作，开发人员只需要简单地实现相应的接口即可，完全不用考虑底层数据流、容错、程序的并行执行等细节。这种设计无疑大大降低了开发分布式并行程序的难度。

网格计算通常是指通过现有的互联网，利用大量来自不同地域、资源异构的计算机空闲的CPU 和磁盘来进行分布式存储和计算。这些参与计算的计算机具有分处不同地域、资源异构（基于不同平台，使用不同的硬件体系结构等）等特征，从而使网格计算和Hadoop 这种基于集群的计算相区别开。Hadoop 集群一般构建在通过高速网络连接的单一数据中心内，集群计算机都具有体系结构、平台一致的特点，而网格计算需要在互联网接入环境下使用，网络带宽等都没有保证。


## Hadoop发展现状

Hadoop 设计之初的目标就定位于高可靠性、高可拓展性、高容错性和高效性，正是这些设计上与生俱来的优点，才使得Hadoop 一出现就受到众多大公司的青睐，同时也引起了研究界的普遍关注。到目前为止，Hadoop 技术在互联网领域已经得到了广泛的运用，例如，Yahoo 使用4 000 个节点的Hadoop集群来支持广告系统和Web 搜索的研究；Facebook 使用1 000 个节点的集群运行Hadoop，存储日志数据，支持其上的数据分析和机器学习；百度用Hadoop处理每周200TB 的数据，从而进行搜索日志分析和网页数据挖掘工作；中国移动研究院基于Hadoop 开发了“大云”（Big Cloud）系统，不但用于相关数据分析，还对外提供服务；淘宝的Hadoop 系统用于存储并处理电子商务交易的相关数据。国内的高校和科研院所基于Hadoop 在数据存储、资源管理、作业调度、性能优化、系统高可用性和安全性方面进行研究，相关研究成果多以开源形式贡献给Hadoop 社区。

除了上述大型企业将Hadoop 技术运用在自身的服务中外，一些提供Hadoop 解决方案的商业型公司也纷纷跟进，利用自身技术对Hadoop 进行优化、改进、二次开发等，然后以公司自有产品形式对外提供Hadoop 的商业服务。比较知名的有创办于2008 年的Cloudera 公司，它是一家专业从事基于ApacheHadoop 的数据管理软件销售和服务的公司，它希望充当大数据领域中类似RedHat 在Linux 世界中的角色。该公司基于Apache Hadoop 发行了相应的商业版本Cloudera Enterprise，它还提供Hadoop 相关的支持、咨询、培训等服务。在2009 年，Cloudera 聘请了Doug Cutting（Hadoop 的创始人）担任公司的首席架构师，从而更加加强了Cloudera 公司在Hadoop 生态系统中的影响和地位。最近，Oracle 也表示已经将Cloudera 的Hadoop 发行版和Cloudera Manager 整合到Oracle Big Data Appliance 中。同样，Intel 也基于Hadoop 发行了自己的版本IDH。从这些可以看出，越来越多的企业将Hadoop 技术作为进入大数据领域的必备技术.

需要说明的是，Hadoop 技术虽然已经被广泛应用，但是该技术无论在功能上还是在稳定性等方面还有待进一步完善，所以还在不断开发和不断升级维护的过程中，新的功能也在不断地被添加和引入，读者可以关注Apache Hadoop的官方网站了解最新的信息。得益于如此多厂商和开源社区的大力支持，相信在不久的将来，Hadoop 也会像当年的Linux 一样被广泛应用于越来越多的领域，从而风靡全球。

**MapReduce与Hadoop之比较**
   
   　　Hadoop是Apache软件基金会发起的一个项目，在大数据分析以及非结构化数据蔓延的背景下，Hadoop受到了前所未有的关注。
   
   　　Hadoop是一种分布式数据和计算的框架。它很擅长存储大量的半结构化的数据集。数据可以随机存放，所以一个磁盘的失败并不会带来数据丢失。Hadoop也非常擅长分布式计算——快速地跨多台机器处理大型数据集合。
   
   　　MapReduce是处理大量半结构化数据集合的编程模型。编程模型是一种处理并结构化特定问题的方式。例如，在一个关系数据库中，使用一种集合语言执行查询，如SQL。告诉语言想要的结果，并将它提交给系统来计算出如何产生计算。还可以用更传统的语言(C++，Java)，一步步地来解决问题。这是两种不同的编程模型，MapReduce就是另外一种。
   
   　　MapReduce和Hadoop是相互独立的，实际上又能相互配合工作得很好。

## Hadoop集群系统

Google的数据中心使用廉价的Linux PC机组成集群，在上面运行各种应用。即使是分布式开发的新手也可以迅速使用Google的基础设施。核心组件是3个：

　　1.GFS（Google File System）。一个分布式文件系统，隐藏下层负载均衡，冗余复制等细节，对上层程序提供一个统一的文件系统API接口。Google根据自己的需求对它进行了特别优化，包括：超大文件的访问，读操作比例远超过写操作，PC机极易发生故障造成节点失效等。GFS把文件分成64MB的块，分布在集群的机器上，使用Linux的文件系统存放。同时每块文件至少有3份以上的冗余。中心是一个Master节点，根据文件索引，找寻文件块。详见Google的工程师发布的GFS论文。

　　2.MapReduce。Google发现大多数分布式运算可以抽象为MapReduce操作。Map是把输入Input分解成中间的Key/Value对，Reduce把Key/Value合成最终输出Output。这两个函数由程序员提供给系统，下层设施把Map和Reduce操作分布在集群上运行，并把结果存储在GFS上。

　　3.BigTable。一个大型的分布式数据库，这个数据库不是关系式的数据库。像它的名字一样，就是一个巨大的表格，用来存储结构化的数据。

## Hadoop信息安全

通过Hadoop安全部署经验总结，开发出以下十大建议，以确保大型和复杂多样环境下的数据信息安全。

　　1、先下手为强!在规划部署阶段就确定数据的隐私保护策略，最好是在将数据放入到Hadoop之前就确定好保护策略。

　　2、确定哪些数据属于企业的敏感数据。根据公司的隐私保护政策，以及相关的行业法规和政府规章来综合确定。

　　3、及时发现敏感数据是否暴露在外，或者是否导入到Hadoop中。

　　4、搜集信息并决定是否暴露出安全风险。

　　5、确定商业分析是否需要访问真实数据，或者确定是否可以使用这些敏感数据。然后，选择合适的加密技术。如果有任何疑问，对其进行加密隐藏处理，同时提供最安全的加密技术和灵活的应对策略，以适应未来需求的发展。

　　6、确保数据保护方案同时采用了隐藏和加密技术，尤其是如果我们需要将敏感数据在Hadoop中保持独立的话。

　　7、确保数据保护方案适用于所有的数据文件，以保存在数据汇总中实现数据分析的准确性。

　　8、确定是否需要为特定的数据集量身定制保护方案，并考虑将Hadoop的目录分成较小的更为安全的组。

　　9、确保选择的加密解决方案可与公司的访问控制技术互操作，允许不同用户可以有选择性地访问Hadoop集群中的数据。

　　10、确保需要加密的时候有合适的技术(比如Java、Pig等)可被部署并支持无缝解密和快速访问数据。