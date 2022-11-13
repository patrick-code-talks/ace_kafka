### Topics

1. Identified by its name, like: logs, users, user_r2p10
2. Like a database table but can not query
3. Producers to send data and consumers to read data

### Producers

1. Create new message send to topic

### Consumers

1. Read data from topic
2. Consumer from same Consumers groups only can read data from one partition each time
3. The offset is used by Kafka to maintain the current position of a consumer

### Broker

1. A single Kafka server is called a broker
2. Receives messages from producers
3. Are desigined to operate as part of a cluster

### Zookeeper

1. maintain the list of members of a cluster
2. Helps to keep track of the Kafka cluster nodes status, partiotions.

### Replica

1. It guarantees availability and durability when individual nodes fail