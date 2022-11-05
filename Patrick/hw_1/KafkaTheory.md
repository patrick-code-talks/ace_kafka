# Kafka Theory

### Topics
- A stream of data
- Split in partitions
  - One topic can have as many partitions as you want. 
  - immutable.
  - kept limited time (default is 1 week, configurable).
  - order is guaranteed within each partition, not across partitions.
  - Data is randomly sent to partition unless key is provided.
- Offset is incremental id for each single partition

### Producers
- Produce data to topics.
- Know which partition to write to.
  - if key == null, message will be sent to topics round robin.
  - if key != null, message with same key will be sent to same partitions.

### Consumers
- Consum data from a topics(identified by name) - pull model
- Automatically knows which partition to read from.
  - If broker failures, consumer know how to recovery.
- Consumer groups
  - All consumers in an application read data as a consumer group.
  - No any two consumers in an consumer group could read data from same partition.
  - Different consumer groups could consume the same topic.
- Consumer offset
  - consumer offset is committed in kafka topic, named `__consumer_offsets__`
  - Delivery semantics
    - At least once
      - offsets are committed after message is processed.
    - At most once
      - offsets are committed once message is received.
    - Exactly once

### Brokers
- kafka server, unit of kafka servers.
- ID will be used as identification.
- Connecting one kafka broker will be connecting to entire cluster.

### Replication
- At any time, one broker will be leader for any given partition.

### Acknowledge & Durability
- Producer acknowledge
  Producer can chooose to receive acknowledge of data write to partition.
  - acks = 0: producer won't wait for ack.(possible data loss)
  - acks = 1: producer wait for leader ack.(limisted data loss)
  - acks = all: leader + replica acks.(no data loss)
- Topic durability

### Zookeeper
- skip for now

## Summary
###  Definition categories
- Logical definition
  - Topic
  - Producer
  - Consumer
- Physical definition
  - broker(bootstrap server)
  - cluster