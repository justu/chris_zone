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
<h4>2.Mapreduce介绍：</h4>
<p>Hadoop Map/Reduce是一个使用简易的软件框架，基于它写出来的应用程序能够运行在由上千个商用机器组成的大型集群上，并以一种可靠容错的方式并行处理上T级别的数据集。
  
一个Map/Reduce 作业（job） 通常会把输入的数据集切分为若干独立的数据块，由 map任务（task）以完全并行的方式处理它们。框架会对map的输出先进行排序， 然后把结果输入给reduce任务。通常作业的输入和输出都会被存储在文件系统中。 整个框架负责任务的调度和监控，以及重新执行已经失败的任务。

通常，Map/Reduce框架和分布式文件系统是运行在一组相同的节点上的，也就是说，计算节点和存储节点通常在一起。这种配置允许框架在那些已经存好数据的节点上高效地调度任务，这可以使整个集群的网络带宽被非常高效地利用。

Map/Reduce框架由一个单独的master JobTracker 和每个集群节点一个slave TaskTracker共同组成。master负责调度构成一个作业的所有任务，这些任务分布在不同的slave上，master监控它们的执行，重新执行已经失败的任务。而slave仅负责执行由master指派的任务。

应用程序至少应该指明输入/输出的位置（路径），并通过实现合适的接口或抽象类提供map和reduce函数。再加上其他作业的参数，就构成了作业配置（job configuration）。然后，Hadoop的 job client提交作业（jar包/可执行程序等）和配置信息给JobTracker，后者负责分发这些软件和配置信息给slave、调度任务并监控它们的执行，同时提供状态和诊断信息给job-client。

虽然Hadoop框架是用JavaTM实现的，但Map/Reduce应用程序则不一定要用 Java来写 。</p>
<h5>Mapreduce运行机制</h5>
<img src="http://images.cnitblog.com/blog/306623/201306/23175200-0ccb72e4bd7b4e48a2eea8673f361741.x-png"></img>
<img src="http://images.cnitblog.com/blog/306623/201306/23175247-1cff38de2f154503bccd89a5d057f696.x-png"></img>
<img src="http://images.cnitblog.com/blog/306623/201306/23175340-b918c709af954125962ac14125ad4823.jpg"></img>
<table>
  <tr>
    <td>输入分片（input split）</td>
    <td>在进行map计算之前，mapreduce会根据输入文件计算输入分片（input split），每个输入分片（input split）针对一个map任务，输入分片（input split）存储的并非数据本身，而是一个分片长度和一个记录数据的位置的数组，输入分片（input split）往往和hdfs的block（块）关系很密切，假如我们设定hdfs的块的大小是64mb，如果我们输入有三个文件，大小分别是3mb、65mb和127mb，那么mapreduce会把3mb文件分为一个输入分片（input split），65mb则是两个输入分片（input split）而127mb也是两个输入分片（input split），换句话说我们如果在map计算前做输入分片调整，例如合并小文件，那么就会有5个map任务将执行，而且每个map执行的数据大小不均，这个也是mapreduce优化计算的一个关键点。</td>
  </tr>
  <tr>
    <td>map阶段</td>
    <td>就是程序员编写好的map函数了，因此map函数效率相对好控制，而且一般map操作都是本地化操作也就是在数据存储节点上进行。</td>
  </tr>
  <tr>
    <td>combiner阶段</td>
    <td>combiner阶段是程序员可以选择的，combiner其实也是一种reduce操作，因此我们看见WordCount类里是用reduce进行加载的。Combiner是一个本地化的reduce操作，它是map运算的后续操作，主要是在map计算出中间文件前做一个简单的合并重复key值的操作，例如我们对文件里的单词频率做统计，map计算时候如果碰到一个hadoop的单词就会记录为1，但是这篇文章里hadoop可能会出现n多次，那么map输出文件冗余就会很多，因此在reduce计算前对相同的key做一个合并操作，那么文件会变小，这样就提高了宽带的传输效率，毕竟hadoop计算力宽带资源往往是计算的瓶颈也是最为宝贵的资源，但是combiner操作是有风险的，使用它的原则是combiner的输入不会影响到reduce计算的最终输入，例如：如果计算只是求总数，最大值，最小值可以使用combiner，但是做平均值计算使用combiner的话，最终的reduce计算结果就会出错。</td>
  </tr>
  <tr>
    <td>shuffle阶段</td>
    <td>将map的输出作为reduce的输入的过程就是shuffle了，这个是mapreduce优化的重点地方。这里我不讲怎么优化shuffle阶段，讲讲shuffle阶段的原理，因为大部分的书籍里都没讲清楚shuffle阶段。Shuffle一开始就是map阶段做输出操作，一般mapreduce计算的都是海量数据，map输出时候不可能把所有文件都放到内存操作，因此map写入磁盘的过程十分的复杂，更何况map输出时候要对结果进行排序，内存开销是很大的，map在做输出时候会在内存里开启一个环形内存缓冲区，这个缓冲区专门用来输出的，默认大小是100mb，并且在配置文件里为这个缓冲区设定了一个阀值，默认是0.80（这个大小和阀值都是可以在配置文件里进行配置的），同时map还会为输出操作启动一个守护线程，如果缓冲区的内存达到了阀值的80%时候，这个守护线程就会把内容写到磁盘上，这个过程叫spill，另外的20%内存可以继续写入要写进磁盘的数据，写入磁盘和写入内存操作是互不干扰的，如果缓存区被撑满了，那么map就会阻塞写入内存的操作，让写入磁盘操作完成后再继续执行写入内存操作，前面我讲到写入磁盘前会有个排序操作，这个是在写入磁盘操作时候进行，不是在写入内存时候进行的，如果我们定义了combiner函数，那么排序前还会执行combiner操作。每次spill操作也就是写入磁盘操作时候就会写一个溢出文件，也就是说在做map输出有几次spill就会产生多少个溢出文件，等map输出全部做完后，map会合并这些输出文件。这个过程里还会有一个Partitioner操作，对于这个操作很多人都很迷糊，其实Partitioner操作和map阶段的输入分片（Input split）很像，一个Partitioner对应一个reduce作业，如果我们mapreduce操作只有一个reduce操作，那么Partitioner就只有一个，如果我们有多个reduce操作，那么Partitioner对应的就会有多个，Partitioner因此就是reduce的输入分片，这个程序员可以编程控制，主要是根据实际key和value的值，根据实际业务类型或者为了更好的reduce负载均衡要求进行，这是提高reduce效率的一个关键所在。到了reduce阶段就是合并map输出文件了，Partitioner会找到对应的map输出文件，然后进行复制操作，复制操作时reduce会开启几个复制线程，这些线程默认个数是5个，程序员也可以在配置文件更改复制线程的个数，这个复制过程和map写入磁盘过程类似，也有阀值和内存大小，阀值一样可以在配置文件里配置，而内存大小是直接使用reduce的tasktracker的内存大小，复制时候reduce还会进行排序操作和合并文件操作，这些操作完了就会进行reduce计算了。</td>
  </tr>
  <tr>
    <td>reduce阶段</td>    
    <td>和map函数一样也是程序员编写的，最终结果是存储在hdfs上的。</td>
  </tr>
</table>
