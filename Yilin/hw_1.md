## Topic
- Similar to how databases have tables to organize and segment datasets, Kafka uses the concept of topics to organize related messages.
- A topic is identified by its name. For example, we may have a topic called logs that may contain log messages from our application, and another topic called purchases that may contain purchase data from our application as it happens.



## Partition
- Topics are broken down into a number of partitions. A single topic may have more than one partition, it is common to see topics with 100 partitions.
- The number of partitions of a topic is specified at the time of topic creation. Partitions are numbered starting from 0 to N-1, where N is the number of partitions. The figure below shows a topic with three partitions, with messages being appended to the end of each one.


## Producer
- Applications that send data into topics are known as Kafka producers. 
- A Kafka producer sends messages to a topic, and messages are distributed to partitions according to a mechanism such as key hashing (more on it below).



#### Kafka message is consisted of
- Each event message contains an optional key and a value.
- Kafka message keys are commonly used when there is a need for message ordering for all messages sharing the same field. For example, in the scenario of tracking trucks in a fleet, we want data from trucks to be in order at the individual truck level. In that case, we can choose the key to be truck_id. In the example shown below, the data from the truck with id truck_id_123 will always go to partition p0.



## Consumers
- Applications that read data from Kafka topics are known as consumers.
- Consumers can read from one or more partitions at a time in Apache Kafka, and data is read in order within each partition as shown below.


#### Consumer group
- Consumers that are part of the same application and therefore performing the same "logical job" can be grouped together as a Kafka consumer group.
- It is important to note that each topic partition is only assigned to one consumer within a consumer group, but a consumer from a consumer group can be assigned multiple partitions.
- Kafka brokers use an internal topic named __consumer_offsets that keeps track of what messages a given consumer group last successfully processed.
- Offsets are critical for many applications. If a Kafka client crashes, a rebalance occurs and the latest committed offset help the remaining Kafka consumers know where to restart reading and processing messages.




## Kafka Brokers
- A single Kafka server is called a Kafka Broker. That Kafka broker is a program that runs on the Java Virtual Machine (Java version 11+) and usually a server that is meant to be a Kafka broker will solely run the necessary program and nothing else.
- An ensemble of Kafka brokers working together is called a Kafka cluster. Some clusters may contain just one broker or others may contain three or potentially hundreds of brokers. Companies like Netflix and Uber run hundreds or thousands of Kafka brokers to handle their data.






## Topic replication factor
- One of the main reasons for Kafka's popularity, is the resilience it offers in the face of broker failures. Machines fail, and often we cannot predict when that is going to happen or prevent it. Kafka is designed with replication as a core feature to withstand these failures while maintaining uptime and data accuracy.
- In Kafka, replication means that data is written down not just to one broker, but many.
- A replication factor of 3 is a commonly used replication factor as it provides the right balance between broker loss and replication overhead.




## Producer acknowledgments(acks)
- Kafka producers only write data to the current leader broker for a partition.
- Kafka producers must also specify a level of acknowledgment acks to specify if the message must be written to a minimum number of replicas before being considered a successful write.
- When acks=0 producers consider messages as "written successfully" the moment the message was sent without waiting for the broker to accept it at all.
- When acks=1 , producers consider messages as "written successfully" when the message was acknowledged by only the leader.
- When acks=all, producers consider messages as "written successfully" when the message is accepted by all in-sync replicas (ISR).


## Zookeeper
- Zookeeper is used for metadata management in the Kafka world.
- Zookeeper keeps track of which brokers are part of the Kafka cluster
- Zookeeper is used by Kafka brokers to determine which broker is the leader of a given partition and topic and perform leader elections