# Kafka
Apache Kafka is an open-source stream-processing software platform developed by the Apache Software Foundation, written in Scala and Java. The project aims to provide a unified, high-throughput, low-latency platform for handling real-time data feeds. Kafka can connect to external systems (for data import/export) via Kafka Connect and provides Kafka Streams, a Java stream processing library. Kafka uses a binary TCP-based protocol that is optimized for efficiency and relies on a "message set" abstraction that naturally groups messages together to reduce the overhead of the network roundtrip. This "leads to larger network packets, larger sequential disk operations, contiguous memory blocks which allows Kafka to turn a bursty stream of random message writes into linear writes.

Kafka was developed by LinkedIn in 2010, and it has been a top-level Apache project since 2012. It is a highly scalable, durable, robust, and fault-tolerant publish-subscribe event streaming platform.

## Let’s explore what some of the common use cases of Kafka are:
- Real-time processing of application activity tracking, like searches.
- Stream processing
- Log aggregation, where Kafka consolidates logs from multiple services (producers) and standardises the format for consumers.
- An interesting use case that has emerged is the microservices architecture. Kafka can be a suitable choice for event sourcing microservices where a lot of events are generated and we want to keep track of the sequence of events (i.e. what has happened).

## Basic Components
Let’s talk a little about the basic components that Kafka uses for its publish-subscribe messaging system. A producer is an entity/application that publishes data to a Kafka cluster, which is made up of brokers. A broker is responsible for receiving and storing the data when a producer publishes. A consumer then consumes data from a broker at a specified offset, i.e. position.

That is, it’s a multi-producer, multi-consumer structure, and it looks something like this:
![image](https://user-images.githubusercontent.com/6689256/108030650-93ae1300-6ffd-11eb-990b-07209b7fe224.png)

What does a basic unit of data look like in Kafka? This is generally called a message or a record (interchangeably). A message contains the data and also the metadata. The metadata contains information such as the offset, a timestamp, compression type, and etc.

These messages are organised into logical groupings or categories which are called a topic, to which producers publish data. Typically, messages in a topic are spread across different partitions in different brokers. A broker manages many partitions.

A producer can publish to multiple topics. You can define what your topics are and which topics a producer publishes to. In a similar vein, consumers can choose which topics they want to subscribe to as well. In some ways, this is similar to reading and writing to database tables.

A topic is then divided into partitions, where each contains a subset of a topic’s messages. A broker can have multiple partitions. Why are there multiple partitions for a topic? Primarily it is to increase throughput; parallel access to the topic can occur.

Further, the Kafka brokers also give us reliability and data protection using replication. If a broker fails, then all the partitions assigned to that broker would become unavailable.

To resolve this issue, there is the concept of a replica, i.e. a duplicate of each partition. You can specify the number of replicas a partition has. At a given point in time, all replicas are identical to the original partition — i.e. “leader” — unless it hasn’t caught up to the most recent data in the leader.
![image](https://user-images.githubusercontent.com/6689256/108030805-cd7f1980-6ffd-11eb-9325-344a9195398e.png)

What is unique about Kafka is that it keeps all the messages for a set amount of time (this can be indefinitely). Each message has an offset, or position, in this message log. Instead of Kafka managing which message a consumer is up to, Kafka delegates this responsibility entirely to the consumer itself. By doing this, Kafka is able to support many more consumers.

## Kafka - Sumamry
![image](https://user-images.githubusercontent.com/6689256/108030350-1a162500-6ffd-11eb-9d5d-940f5994b654.png)

## a) Installing Kafka
1. Install the Java Development Kit (JDK): http://www.oracle.com/technetwork/java/javase/downloads/index.html

2. Download latest version from https://kafka.apache.org/downloads

3. Unpack latest version
> tar -xzf <kafka tarball>
> cd <new kafka directory>

4. Start Zookeeper
> bin/zookeeper-server-start.sh config/zookeeper.properties

5. Start Kafka Server
> bin/kafka-server-start.sh config/server.properties

6. For Windows
> Open Kafka version folder you downloaded and extracted and create a new folder called - data. Add Zookeeper and Kafka folder.

> Copy the path for zookeeper (C:\kafka_version\data\zookeeper) and put it in temp variable inside zookeeper.properties inside config folder (use notepad ++ to open it) and change forward slash to backward slash.

> Then in terminal type: C:\Kafka_version id > zookeeper-server-start.bat config\zookeeper.properties

> If all goes well then you see binding port 0.0.0.0:2181

> Repeat it for kafka now by opening the xfile server.properties from config and change the path variable log.dirs to (C:\kafka_version\data\kafka)

> Then in terminal type: C:\Kafka_version id > kafka-server-start.bat config\server.properties

## b) Creating a topic
This code is made for Unix-based systems such as Linux and Mac OS. For Windows use bin\windows\ instead of bin/, and change the script extension to .bat

1. Run built-in script to create new topic named "test" with 1 partition on 1 node
> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

2. See the topic
> bin/kafka-topics.sh --list --zookeeper localhost:2181

## c) Sending and receiving messages
1. Run the producer in terminal and enter some messages
> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```
message 1
message 2
message 3
```

2. In a new terminal window read the messages
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```
message 1
message 2
message 3
```
If you go side-by-side with the terminal windows you can type in the producer window and see the results appear in the consumer

## References
1. https://medium.com/better-programming/an-introduction-to-apache-kafka-95a82260c1c3
2. https://towardsdatascience.com/kafka-python-explained-in-10-lines-of-code-800e3e07dad1
3. https://medium.com/swlh/apache-kafka-in-a-nutshell-5782b01d9ffb (Detailed Read)
4. https://medium.com/hacking-talent/kafka-all-you-need-to-know-8c7251b49ad0
