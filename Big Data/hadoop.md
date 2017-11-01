<h2>Apache Hadoop开发可靠、可扩展、分布式计算的开源软件</h2>

<strong>Apache Hadoop软件库是一个框架，允许在使用简单编程模型的计算机集群上对大型数据集进行分布式处理。 它旨在从单个服务器扩展到数千台机器，每台机器都提供本地计算和存储</strong>

<h4>主要包括如下模块：</h4>
<table>
  <tr>
    <td>Hadoop Common</td>
    <td><strong>支持其他Hadoop模块的通用工具</strong></td>
  </tr>
  <tr>
    <td>HDFS（Hadoop分布式文件系统）</td>
    <td><strong>提供对应用程序数据的高吞吐量访问的分布式文件系统</strong></td>
  </tr>
  <tr>
    <td>Hadoop YARN</td>
    <td><strong>作业调度和集群资源管理的框架</strong></td>
  </tr>
  <tr>
    <td>Hadoop MapReduce</td>
    <td><strong>用于并行处理大型数据集的基于YARN的系统</strong></td>
  </tr>
</table>
<h4>Apache其他hadoop相关项目包括：</h4>
<ul>
  <li>Ambari™：用于配置，管理和监控Apache Hadoop集群的基于Web的工具，其中包括支持Hadoop HDFS，Hadoop MapReduce，Hive，HCatalog，HBase，ZooKeeper，Oozie，Pig和Sqoop。 Ambari还提供了一个用于查看集群健康状况的仪表板，如热图和可视化查看MapReduce，Pig和Hive应用程序以及以用户友好的方式诊断其性能特征的功能。</li>
  <li>Avro™：数据序列化系统。</li>
  <li>Cassandra™：可扩展的多主数据库，无单点故障。</li>
  <li>Chukwa™：用于管理大型分布式系统的数据收集系统。</li>
  <li>HBase™：可扩展的分布式数据库，支持大型表格的结构化数据存储。</li>
  <li>Hive™：提供数据摘要和即席查询的数据仓库基础设施。</li>
  <li>Mahout™：可扩展的机器学习和数据挖掘库。</li>
  <li>Pig™：用于并行计算的高级数据流语言和执行框架。</li>
  <li>Spark™：一种用于Hadoop数据的快速且通用的计算引擎。 Spark提供了一个简单而富有表现力的编程模型，支持各种应用，包括ETL，机器学习，流处理和图形计算。</li>
  <li>Tez™：一种基于Hadoop YARN的通用数据流编程框架，它提供了强大且灵活的引擎来执行任意DAG的任务来处理批量和交互式用例的数据。 Tez被Hado™，Pig™和Hadoop生态系统中的其他框架以及其他商业软件（例如ETL工具）采用，以替代Hadoop™MapReduce作为底层执行引擎。</li>
  <li>ZooKeeper™：分布式应用程序的高性能协调服务。</li>
</ul>
<h4>1.HDFS的分布式存储实现：</h4>
<p>HDFS是一个分布式文件系统，去掉分布式，其实就是一个文件系统，通过对元数据进行管理，对上传的数据文件进行层层拆分，实现物理上的分布式存储和逻辑上的统一调度管理</p>
<ul>
  <li><strong>主从结构：</strong>主节点——namenode，从节点，有很多个datanode</li>
  <li><strong>namenode：</strong>接收用户操作请求，维护文件目录结构，管理文件与block之间关系，block与datanode之间关系</li>
  <li><strong>datanode：</strong>存储文件，拆分文件为一个个block存储在磁盘上，为保证数据安全，文件会有多个副本</li>    
</ul>
<p>HDFS采用master/slave架构。一个HDFS集群是由一个Namenode和一定数目的Datanodes组成。Namenode是一个中心服务器，负责管理文件系统的名字空间(namespace)以及客户端对文件的访问。集群中的Datanode一般是一个节点一个，负责管理它所在节点上的存储。HDFS暴露了文件系统的名字空间，用户能够以文件的形式在上面存储数据。从内部看，一个文件其实被分成一个或多个数据块，这些块存储在一组Datanode上。Namenode执行文件系统的名字空间操作，比如打开、关闭、重命名文件或目录。它也负责确定数据块到具体Datanode节点的映射。Datanode负责处理文件系统客户端的读写请求。在Namenode的统一调度下进行数据块的创建、删除和复制。</p>
<img src="http://hadoop.apache.org/docs/r1.0.4/cn/images/hdfsarchitecture.gif"></img>

