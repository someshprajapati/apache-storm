# Apache Storm Installation and Configurations


### Zookeeper Installation and Configurations
1-zookeeper/
1. zookeeper-standalone
2. zookeeper-multinode

### Storm Installation and Configurations
2-storm/
1. strom-standalone
2. storm-multinode


### Summary of the Apache Storm:

1. Storm is used for processing streams of data.
2. Storm follows a **Nimbus(Master Node)** and **Supervisor(Worker Node)** architecture.
3. Storm data model consists of **tuples**(an ordered list of named values) and **streams**(unbounded sequence of tuples).
4. Storm consists of spouts and bolts.
    1. **Spouts** is a component of Storm that ingests the data and creates the stream of tuples for processing by the bolts.
    2. **Bolts** processes the tuples from spouts and outputs to external systems or other bolts. The processing logic in a bolt can include filters, joins, and aggregation.
5. Spouts create tuples that are processed by bolts.
6. Spouts and bolts together form the Storm topology.
7. Data processing is done by workers that are monitored by supervisors.
8. Serialization is the process to convert data structures or objects into a platform-independent stream of bytes. To read the data, programs have to reverse the serialization process. This is called deserialization.


### Types of Topologies

1. **Simple topology:** It consists of simple spouts and bolts. The output of spouts is sent to bolts.

2. **Transactional topology:** It guarantees processing of tuples only once. This is an abstraction of the above simple topology. The Storm libraries ensure that a tuple is processed exactly once even if the system goes down in the middle. This topology has become deprecated and replaced by trident topology in the current version of Storm.

3. **Distributed RPC:** Distributed RPC is also called DRPC. This topology provides libraries to parallelize any computation using Storm.

4. **Trident topology:** It is a higher level abstraction over Storm. That means it has libraries that run on top of Storm libraries to support additional functionality. Trident provides spouts and bolts that provide much higher functionality such as transaction processing and batch processing.
