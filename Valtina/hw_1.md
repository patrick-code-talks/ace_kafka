## Topic
- A topic is like a table in a database
- No limit amount
- Define by name
- Support any kind of message format
- Can NOT query topic

## Partition
- No limit amount
- The topic is split into partitions
- Messages in each partition are ordered
- `Offset` -> mark the sequence of messages, like id
- Data is assigned randomly to partitions unless a key is provided
- Once the data is written to a partition, it cannot be changed
>Data can be kept by default for one week

## Producer
- Write data to topics
- Producers know to which partition to write to
- If the Kafka broker fails, producers will automatically recover
- `Key` is optional to be set; 
>if the key is not null, all messages for that key will always go to the same partition 
>(if you need message ordering for a specific field, you should set a key)

#### Kafka message is consisted of
- Key-value pair (both can be null) in binary format (serialization and deserialization)
- Compression type
- headers(optional)
- Partition + offset
- Timestamp 

## Consumers
- Read data from a topic (pull model)
- Consumers know automatically which broker to read from
- If the Kafka broker fails, consumers will automatically recover
- Data is read in order from low to high offset within each partition

#### Consumer group
- Each consumer within a group reads from exclusive partitions
- If you have more consumers than partitions, some consumers will be inactive
- The same topic can have multiple consumer groups
- Consumer offsets
  - __consumer_offsets
  - Define where the consumer is currently at
  - Consumers will automatically commit offsets (at least once)
  - 3 delivery semantics if you choose to commit manually
    - At least once (preferred) -> after messages are processed
    - At most once -> as soon as messages are processed
    - Exactly once -> use Transactional API

## Kafka Brokers
- A cluster is composed of multiple brokers
- Broker is identified with its ID (integer)
- Broker contains certain topic partitions
- If you connect to any broker, you are connected to the entire cluster
- Usually 3 brokers
- A broker does NOT necessarily has to have all the topic partitions
- Kafka broker discovery
  - Every Kafka broker is also called a `bootstrap server`
  - Once you connect to one bootstrap you are connected to the entire cluster


## Topic replication factor
- Topics should have a replication factor more than 1
  - For a replication factor of N, you can lose up to `N-1` 
- If one broker is down, another broker can serve the data
- There is only one broker can be a leader for any partition
- Producers can only send data to the leaders and consumers will only read from the leader broker for a partition, while other brokers replicate data


## Producer acknowledgments(acks)
- Acks = 0: producer will NOT wait for acknowledgment
- Acks = 1: producer will wait for leader acknowledgment
- Acks = all: leader replicas acknowledgment


## Zookeeper
- Manages brokers (keep a list)
- Select leader for partitions
- Send notifications to Kafka in case of changes
- Zookeeper is necessary now but not in the future (optional in 3.x and no need in 4.x) because Zookeeper has a scalability issue
- KRaft will replace Zookeeper to improve performance
- Zookeeper does `NOT` store consumer offsets with Kafka > v0.10 (**store in topic**)
- Do not use Zookeeper as a configuration on your Kafka client
