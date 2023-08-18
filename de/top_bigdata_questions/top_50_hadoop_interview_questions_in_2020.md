# Top 50 Hadoop Interview Questions In 2020

follow this [link](https://www.edureka.co/blog/interview-questions/top-50-hadoop-interview-questions-2016/) for details.

----
### 1. What are the basic differences between relational database and HDFS?

|                         | RDBMS   | Hadoop  |
| :---                    | :---    | :---    |
|Data Types               |RDBMS relies on the structured data and the schema of the data is always known.|Any kind of data can be stored into Hadoop i.e. Be it structured, unstructured or semi-structured.|
|Processing               |RDBMS provides limited or no processing capabilities.|Hadoop allows us to process the data which is distributed across the cluster in a parallel fashion.|
|Schema on Read Vs. Write |RDBMS is based on ‘schema on write’ where schema validation is done before loading the data.|On the contrary, Hadoop follows the schema on read policy.|
|Read/Write Speed         |In RDBMS, reads are fast because the schema of the data is already known.|The writes are fast in HDFS because no schema validation happens during HDFS write.|

----
### 3. What is Hadoop and its components. 
- Storage unit : <b>HDFS</b> (NameNode, DataNode)
- Processing framework : <b>YARN</b> (ResourceManager, NodeManager)

----
### 4. What are HDFS and YARN?

<b>HDFS</b> (Hadoop Distributed File System) is the storage unit of Hadoop. 
It is responsible for storing different kinds of data as blocks in a distributed environment. 
It follows <b>master and slave topology</b>.

- <b>NameNode</b>: NameNode is the master node in the distributed environment and it maintains the metadata information for the blocks of data stored in HDFS like block location, replication factors etc.
- <b>DataNode</b>: DataNodes are the slave nodes, which are responsible for storing data in the HDFS. NameNode manages all the DataNodes.

<b>YARN</b> (Yet Another Resource Negotiator) is the processing framework in Hadoop, 
which manages resources and provides an execution environment to the processes.

- <b>ResourceManager</b>: It receives the processing requests, and then passes the parts of requests to corresponding NodeManagers accordingly, where the actual processing takes place. It allocates resources to applications based on the needs.
- <b>NodeManager</b>: NodeManager is installed on every DataNode and it is responsible for the execution of the task on every single DataNode.

----
### 5. Tell me about the various Hadoop daemons and their roles in a Hadoop cluster.
- <b>NameNode</b>: It is the master node which is responsible for storing the metadata of all the files and directories. It has information about blocks, that make a file, and where those blocks are located in the cluster.
- <b>Datanode</b>: It is the slave node that contains the actual data.
- <b>Secondary NameNode</b>: It periodically merges the changes (edit log) with the FsImage (Filesystem Image), present in the NameNode. It stores the modified FsImage into persistent storage, which can be used in case of failure of NameNode.
- <b>ResourceManager</b>: It is the central authority that manages resources and schedule applications running on top of YARN.
- <b>NodeManager</b>: It runs on slave machines, and is responsible for launching the application's containers (where applications execute their part), monitoring their resource usage (CPU, memory, disk, network) and reporting these to the ResourceManager.
- <b>JobHistoryServer</b>: It maintains information about MapReduce jobs after the Application Master terminates.

----
### 7. List the difference between Hadoop 1 and Hadoop 2.

|                   | Hadoop 1.x                                  | Hadoop 2.x                                      |
| :---              | :---                                        | :---                                            |
| Passive NameNode  |NameNode is a <b>Single Point of Failure</b> |Active & Passive NameNode                        |
| Processing        |MRV1 (Job Tracker & Task Tracker)            |MRV2/<b>YARN</b> (ResourceManager & NodeManager) |

----
### 9. Why does one remove or add nodes in a Hadoop cluster frequently?

Attractive features of the Hadoop framework <b>leads to frequent DataNode crashes</b> in a Hadoop cluster.
- <b>utilization of commodity hardware</b>: 상용 제품 활용성이 높음
- <b>ease of scale in accordance with the rapid growth in data volume</b>: 데이터양 급증에 따른 스케일 조정이 용이함

Because of these two reasons, one of the most common task of a Hadoop administrator is 
to <b>commission (Add)</b> and <b>decommission (Remove)</b> "Data Nodes" in a Hadoop Cluster.

----
### 10. What happens when two clients try to access the same file in the HDFS?

HDFS supports <b>exclusive writes only</b>.

When the first client contacts the "NameNode" to open the file for writing, 
the "NameNode" grants a lease to the client to create this file. 
When the second client tries to open the same file for writing, 
the "NameNode" will notice that the lease for the file is already granted to another client, 
and will reject the open request for the second client.

----
### 11. How does NameNode tackle DataNode failures?
NameNode periodically receives a <b>Heartbeat</b> (signal) from each of the DataNode in the cluster, 
which implies DataNode is functioning properly.

A block report contains a list of all the blocks on a DataNode. 
If a DataNode fails to send a heartbeat message, after a specific period of time it is marked dead.

The NameNode replicates the blocks of dead node to another DataNode using the replicas created earlier.


----
### 12. What will you do when NameNode is down?
The NameNode recovery process:
- Use the file system metadata replica (FsImage) to start a new NameNode. 
- Then, configure the DataNodes and clients so that they can acknowledge this new NameNode, that is started.
- Now the new NameNode will start serving the client after it has completed loading the last checkpoint FsImage 
(for metadata information) and received enough block reports from the DataNodes. 

Whereas, on large Hadoop clusters this NameNode recovery process may consume a lot of time 
and this becomes even a greater challenge in the case of the routine maintenance. 
Therefore, we have HDFS High Availability Architecture.


----
### 13. What is a checkpoint?
<b>Checkpointing</b> is a process that takes an FsImage, edit log and compacts them into a new FsImage. 
Thus, instead of replaying an edit log, the NameNode can <b>load the final in-memory state directly from the FsImage</b>. 
This is a far more efficient operation and reduces NameNode startup time. 

Checkpointing is performed by <b>Secondary NameNode</b>.


----
### 14. How is HDFS fault tolerant? 
When data is stored over HDFS, NameNode <b>replicates</b> the data to several DataNode. 
The default replication factor is 3. You can change the configuration factor as per your need. 
If a DataNode goes down, the NameNode will automatically copy the data to another node from the replicas 
and make the data available. 

This provides <b>fault tolerance</b> in HDFS.


----
### 16. Why do we use HDFS for applications having large data sets and not when there are a lot of small files? 
HDFS is more suitable for <b>large amounts of data sets in a single file</b> as compared to 
<b>small amount of data spread across multiple files</b>. 
As you know, the NameNode stores the metadata information regarding the file system in the RAM. 
Therefore, the amount of memory produces a limit to the number of files in my HDFS file system. 
In other words, too many files will lead to the generation of too much metadata. 
And, <b>storing these metadata in the RAM will become a challenge</b>. 
As a thumb rule, metadata for a file, block or directory takes 150 bytes. 


----
### 17. How do you define “block” in HDFS? What is the default block size in Hadoop 1 and in Hadoop 2? Can it be changed? 
Blocks are the nothing but the smallest continuous location on your hard drive where data is stored. 
HDFS stores each as blocks, and distribute it across the Hadoop cluster. 
Files in HDFS are broken down into block-sized chunks, which are stored as independent units.
- Hadoop 1 default block size: 64 MB
- Hadoop 2 default block size: 128 MB

Yes, blocks can be configured. 
The dfs.block.size parameter can be used in the hdfs-site.xml file to set the size of a block in a Hadoop environment.


---- 
### 18. What does ‘jps’ command do? 
The 'jps' command helps us to <b>check if the Hadoop daemons are running or not</b>. 
It shows all the Hadoop daemons i.e namenode, datanode, resourcemanager, nodemanager etc. that are running on the machine.


----
### 19. How do you define “Rack Awareness” in Hadoop?
<b>Rack Awareness</b> is the algorithm in which the "NameNode" decides how blocks and their replicas are placed, 
based on rack definitions to minimize network traffic between "DataNodes" within the same rack. 

For Example, consider replication factor 3 (default), 
"for every block of data, two copies will exist in one rack, third copy in a different rack"

- <b>To improve the network performance</b>: greater network bandwidth between machines in the same rack than the machines residing in different rack.
- <b>To prevent loss of data</b>: even if an entire rack fails because of the switch failure or power failure.


----
### 20. What is "speculative execution" in Hadoop? 
<b>speculative execution</b>: If a node appears to be executing a task slower, 
the master node can redundantly execute another instance of the same task on another node. 
Then, the task which finishes first will be accepted and the other one is killed. 


----
### 28. Explain “Distributed Cache” in a “MapReduce Framework”.
<b>Distributed Cache</b> is a facility provided by the MapReduce framework to cache files needed by applications. 
Once you have cached a file for your job, Hadoop framework will make it available on each and every data nodes 
where you map/reduce tasks are running. 
Then you can access the cache file as a local file in your Mapper or Reducer job.

If the file is present on "hdfs://" or "http://" urls, the user mentions it to be a cache file to the distributed cache. 
<b>MapReduce job will copy the cache file on all the nodes before starting of tasks on those nodes</b>.


----
### 30. What does a “MapReduce Partitioner” do?
A "<b>MapReduce Partitioner</b>" makes sure that all the values of a single key go to <b>the same "reducer"</b>, 
thus allowing even distribution of the map output over the "reducers". 
It redirects the "mapper output" to the "reducer" by determining which "reducer" is responsible for the particular key.

즉, partitioner는 같은 키를 갖는 값들이 같은 reducer로 가도록 해준다.


----
### 32. What is a “Combiner”? 
A "Combiner" is a mini "reducer" that performs the local "reduce" task. 
It receives the input from the "mapper" on a particular "node" and sends the output to the "reducer".

모든 mapper output이 네트워크로 shuffle되면 과다 트래픽이 발생하므로,
로컬 노드에서 conbiner가 mini-reduce를 수행한 뒤 reducer로 보내는 것이다.


---
### 33. What is "Hive"?
하이브는 HDFS 등 하둡 호환 저장시스템에 저장된 데이터를 처리하고 분석하기 위해 SQL-like language를 제공하는 데이터 웨어하우스 소프트웨어다.
SQL은 가장 널리 사용되는 데이터 처리 언어이므로 맵리듀스 등 하둡 데이터 처리 프로그램에 비해 접근성이 높다.
즉, 하이브는 하둡을 위해 SQL의 단순성을 차용하여 넓은 사용자 층이 접근할 수 있도록 해준다.

하이브는 HiveQL이라는 SQL-like 언어를 제공하여 쿼리를 수행할 수 있다.
HiveQL 쿼리는 내부적으로 맵리듀스 잡으로 변환된다.


----
### 38. What is "SerDe" in "Hive"?
Apache Hive is a <b>data warehouse system built on top of Hadoop</b> 
and is used for analyzing structured and semi-structured data developed by Facebook. 
Hive abstracts the complexity of Hadoop MapReduce.

The "<b>SerDe</b>" interface allows you to instruct "Hive" about how a record should be processed. 
A "SerDe" is a combination of a "Serializer" and a "Deserializer". 
<b>"Hive" uses "SerDe" (and "FileFormat") to read and write the table's row</b>.


----
### 39. Can the default "Hive Metastore" be used by multiple users (processes) at the same time?
"Derby database" is the default "Hive Metastore". 
Multiple users (processes) cannot access it at the same time. 
It is mainly used to perform unit tests.

Upgrade your hive metastore to either MySQL, PostgreSQL to support multiple concurrent connections to Hive.


----
### 41. What is Apache HBase?
- HBase is an open source, multidimensional, distributed, scalable and a <b>NoSQL database</b> written in Java. 
- HBase runs on top of HDFS and provides BigTable (Google) like capabilities to Hadoop. 
- Designed to provide a <b>fault-tolerant</b> way of storing the large collection of sparse data sets. 
- HBase achieves <b>high throughput and low latency</b> by providing faster Read/Write Access on huge datasets.


----
### 42. What are the components of Apache HBase?
- <b>Region Server</b>: A table can be divided into several regions. A group of regions is served to the clients by a Region Server.
- <b>HMaster</b>: It coordinates and manages the Region Server (similar as NameNode manages DataNode in HDFS).
- <b>ZooKeeper</b>: Zookeeper acts like as a coordinator inside HBase distributed environment. It helps in maintaining server state inside the cluster by communicating through sessions.


----
### 43. What are the components of Region Server?
- <b>WAL</b>: Write Ahead Log (WAL) is a file attached to every Region Server inside the distributed environment. The WAL stores the new data that hasn't been persisted or committed to the permanent storage. It is used in case of failure to recover the data sets
- <b>Block Cache</b>: Block Cache resides in the top of Region Server. It stores the frequently read data in the memory.
- <b>MemStore</b>: It is the write cache. It stores all the incoming data before committing it to the disk or permanent memory. There is <b>one MemStore for each column family in a region</b>.
- <b>HFile</b>: HFile is stored in HDFS. It stores the actual cells on the disk.


----
### 45. Mention the differences between "HBase" and "Relational Databases"?
|HBase                                    |Relational Database                                              |
|:---                                     |:---                                                             |
|It is schema-less                        | It is schema-based database                                     |
|It is column-oriented data store         | It is row-oriented data store                                   |
|It is used to store de-normalized data   |It is used to store normalized data                              |
|It contains sparsely populated tables    |It contains thin tables                                          |
|Automated partitioning is done in HBase  |There is no such provision or built-in support for partitioning  |


----
### 46. What is Apache Spark?
Apache Spark is a framework for <b>real-time</b> data analytics in a <b>distributed</b> computing environment. 
It executes <b>in-memory</b> computations to increase the speed of data processing.

----
### 47. Features of Apache Spark
- <b>in-memory</b>: Hadoop MapReduce는 iteration마다 filesystem을 쓰는 반면, Spark은 in-memory 기반으로 10~100배 빠르다.
- <b>persist/cache</b>: persist(), cache()를 사용하면 failure가 발생해도 lineage의 cache 지점부터 수행 가능하다.
- <b>immutable</b>: 데이터가 메모리에 immutable로 저장되므로 failure가 생겨도 functional transformation을 재수행하기만 하면 <b>fault tolerance</b>가 확보된다.
- <b>Lazy execution</b>: Eager execution은 바로 실행(빈번한 반복연산)되는데 반해, Lazy execution은 Action을 호출하기 전까지는 Transformation의 계산을 실제로 실행하지 않으므로 IO를 줄이고 네트워크 병목현상을 피할 수 있다.


----
### 48. Define RDD.
RDD(Resilient Distribution Datasets) is a <b>fault-tolerant</b> collection of operational elements that <b>run parallel</b>. 
The partitioned data in RDD are <b>immutable</b> and <b>distributed</b>, which is a key component of Apache Spark.


----
### 49. What is Apache ZooKeeper and Apache Oozie?
<b>ZooKeeper</b> coordinates with various services in a distributed environment. 
It saves a lot of time by performing synchronization, configuration maintenance, grouping and naming.

<b>Oozie</b> is a scheduler which schedules Hadoop jobs and binds them together as one logical work. 
There are two kinds of Oozie jobs:
- Oozie Workflow: These are the sequential set of actions to be executed. You can assume it as a relay race. Where each athlete waits for the last one to complete his part.
- Oozie Coordinator: These are the Oozie jobs which are triggered when the data is made available to it. Think of this as the response-stimuli system in our body. In the same manner, as we respond to an external stimulus, an Oozie coordinator responds to the availability of data and it rests otherwise.


----
### Reference
- https://www.edureka.co/blog/interview-questions/top-50-hadoop-interview-questions-2016/
