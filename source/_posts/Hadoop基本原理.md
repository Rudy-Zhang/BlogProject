title: "Hadoop基本原理"
date: 2015-5-23 10:38
category: [Hadoop]
tags: [Hadoop]
---

###了解大数据

首先，搞清楚hadoop在处理大数据的定位在哪里

####什么是大数据？为什么要处理大数据？

数据量大（Volume） 数据类别复杂（Variety） 数据处理速度快（Velocity） 数据真实性高（Veracity） 合起来被称为4V。

处理大数据是为了挖掘数据中的隐含价值

####如何处理大数据？

集中式计算VS分布式计算

集中式计算：通过不断增加处理器的个数来增强耽搁计算机的计算能力，从而提高处理的速度。需要的内存很大，计算的速度很快。

分布式计算：一组通过网络连接的计算机，形成一个分散的系统。将需要处理的大量数据分散成多个部分，交由系统中的耽搁计算机分别处理，最后将这些计算结果合并得到最终结果。（MapReduce的核心思想）

###Hadoop是怎么产生的

####技术基础

google三驾马车：GFS、MapReduce和BigTable。Hadoop是在google三驾马车基础上的开源实现。

1. GFS（Google File System）分布式文件系统，对应Hadoop当中的HDFS。
2. MapReduce分布式计算框架，也是Hadoop处理大数据的核心思想。
3. BigTable是基于GFS的数据存储系统，对应Hadoop的HBase。

####三大分布式计算系统

Hadoop，Spark，Storm是主流的三大分布式计算系统

Spark VS Hadoop

Hadoop使用硬盘来存储数据，而Spark是将数据存在内存中的，因此Spark何以提供超过Hadoop 100倍的计算速度。内存断电后会丢失，所以Spark不
适用于需要长期保存的数据。

Storm VS Hadoop

Storm在Hadoop基础上提供了实时运算的特性，可以实时处理大数据流。不同于Hadoop和Spark，Storm不尽兴数据的手机和存储工作，直接通过网络接受并实时处理数据，然后直接通过网络实时传回结果。

所以三者适用于的应用场景分别为：

1. Hadoop常用于离线的复杂的大数据处理
2. Spark常用于离线的快速的大数据处理
3. Storm常用于在线实时的大数据处理

###Hadoop定义

####Hadoop是什么

Hadoop是一个能够对大量数据进行分布式处理的软件框架

####Hadoop特点

1. 可靠。Hadoop假设计算元素和存储会失败，所以会维护多个工作数据的副本，对失败的节点会重新处理
2. 高效。通过并行方式工作，加快处理速度。
3. 可伸缩。可以处理PB级的数据。
4. 高扩展。可以方便地扩展到数以千计的节点。
5. 低成本。Hadoop是开源的，Hadoop节点可以是很便宜的机器。

####应用场景

Hadoop适用于：海量数据，离线数据，复杂数据

场景1：数据分析，如海量日志分析，商品推荐，用户行为分析

场景2：离线计算，（异构计算+分布式计算）天文计算

场景3：海量数据存储，如Facebook的存储集群。

[更多应用场景](http://cloud.zol.com.cn/441/4415033_all.html)

###Hadoop原理

####HDFS

HDFS（Hadoop File System），是Hadoop的分布式文件存储系统

1. 将大文件分解为多个Block，每个Block保存多个副本。提供容错机制，副本丢失或者宕机时自动恢复。
2. 默认每个Block保存3个副本，64M为1个Block。
3. 将Block按照key-value映射到内存当中。

HDFS架构图如下：

![](http://i.imgur.com/ZliSEXb.png)

#####NameNode

HDFS使用主从结构，NameNode是Master节点，是领导。所有的客户端的读写请求，都需要首先请求NameNode。

NameNode存储

1. fsimage：元数据镜像文件（文件系统的目录树，文件的**元数据**信息）。元数据信息包括文件的信息，文件对应的block信息（版本信息，类型信息，和checksum），以及每一个block所在的DataNode的信息。
2. edits：元数据的操作日志

#####DataNode

DataNode是Slave，负责真正存储所有的block内容，以及数据块的读写操作

NameNode，DataNode，rack只是一些逻辑上的概念。NameNode和DataNode可能是一台机器也可能是，相邻的一台机器，很多DataNode可能处于同一台机器。rack是逻辑上比DataNode更大的概念，可能是一台机器，一台机柜，也可能是一个机房。通过使文件的备份更广泛地分布到不同的rack，DataNode上可以保证数据的可靠性。

#####HDFS写入数据

1. Client拆分文件为64M一块。
2. Client向NameNode发送写数据请求。
3. NameNode节点，记录block信息。并返回可用的DataNode。
4. Client向DataNode发送block1,2,3....；发送过程是以流式写入。流式写入，数据流向为DataNode1->DataNode2->DataNode3(1,2,3为通过规则选出来的可用的DataNode)
5. 发送完毕后告知NameNode
6. NameNode告知Client发送完成

在写数据的时候：

- 写1T文件，我们需要3T的存储，3T的网络流量贷款。
- 在执行读或写的过程中，NameNode和DataNode通过HeartBeat进行保存通信，确定DataNode活着。如果发现DataNode死掉了，就将死掉的DataNode上的数据，放到其他节点去。读取时，要读其他节点去。
- 挂掉一个节点，没关系，还有其他节点可以备份；甚至，挂掉某一个机架，也没关系；其他机架上，也有备份。

#####HDFS读取数据

1. Client向NameNode发送读请求
2. NameNode查看MetaData信息，返回文件的block位置
3. 根据一定规则（优先选择附近的数据），按顺序读取block

[更多内容](http://www.weixuehao.com/archives/596)

####MapReduce

Map是把一组数据一对一的**映射**为另外的一组数据，其映射的规则由一个**map函数**来指定。Reduce是对一组数据进行**归约**，这个归约的规则由一个**reduce函数**指定。

整个的MapReduce执行过程可以表示为：

`(input)<k1, v1> => map => <k2, v2> => combine => <k2, v2’> => reduce => <k3, v3>(output)`

也可以表示为流程图：

![](http://i.imgur.com/yRsLgoK.png)

1. **分割**：把输入数据分割成不相关的若干键/值对（key1/value1）集合，作为input
2. **映射**：这些键/值对会由多个map任务来**并行地处理**。输出一些中间键/值对key2/value2集合
3. **排序**：MapReduce会对map的输出（key2/value2）按照key2进行排序（便于归并）
4. **conbine**：属于同一个key2的所有value2组合在一起作为reduce任务的输入（相当于提前reduce，减小key2的数量，减小reduce的负担）
5. **Partition**：将mapper的输出分配到reducer；（Map的中间结果通常用”hash(key) mod R”这个结果作为标准）
5. **规约**：由reduce任务计算出最终结果并输出key3/value3。

####程序员需要做的

- 单机程序需要处理数据读取和写入、数据处理
- Hadoop程序需要实现map和reduce函数
- map和reduce之间的数据传输、排序，容错处理等由Hadoop MapReduce和HDFS自动完成。
