---
tags: [certification, confluent, kafka, apache]
---

# Confluent Certified Developer for Apache KAFKA (CCDAK)

*Last update: 24 Ago 2024*

Apache Kafka is an open source product developed by LinkedIn and now under Apache Foundation.

Confluent is a private company that gives commercial/enterprise support and provides additional tools to the Kafka ecosystem.

Key concepts:

* events vs static data, flow ws storage. Kafka handle unlimited temporal series of events.
* real time handling
* events persistence

Kafka solution is based on several componenents:

* the `broker`, the Kafka instance in the cluster which has its own storage. Usually there are more than one broker in the cluster, for fault tolerance. 
* the `cluster manager`, that controls the brokers cluster, handling brokers failures and recoveries
* the `producers`, client applications that run outside the cluster and send events to the brokers
* the `consumers`, client applications that run outside the cluster and receive (pull) events from the brokers
* the `streams`, applications that process events, producing other events

Within Kafka, events are saved in named topics. 

## Topic and messages

A topic is a sequence (or stream) of events or messages. A broker can have an unlimited number of topics, each with a unique name.

A message has the following structure:

* timestamp, when the messages has been created or reveived (ingested) by Kafka
* key, optional, the key of the message
* value, the message body
* headers, optional message metadata

Each topic is implemented in 1 or more `partitions`. 

Messages are distributed among the partitions using the hash of the message key. Messages with the same key are sent to the same partition. If the message does not have a key, the messages is sent to a random partition using a round-robin approach. It is also possible to define a custom partitioning logic.

IMPORTANT: messages are not deleted after being consumed. They remain in the topic for 1 week (defaul value for the retention period). Business or legal considerations influence the retention period for all topics in a cluster and/or for a specific topic.

A topic can be defined as `compacted`: only the most recent message with the same key will be maintained in the topics while the less recents messages will be deleted.

## Partition

A partition is a log structure, an immutable and time ordered sequence of events; new events are always attached at the end of the partition.

Each partition can have 0..N replicas, for fault tolerance. The number of replicas is the replication factor; its typical value is 3. Replicas are distributed across the brokers, with one as leader and the others as followers.

The partitions are stored as files (`segments`) on the disk. No structured storage is required by Kafka.

The messages are guaranteed to be in temporal order at patrition level but not a topic level.

Within a partition, each message has its own position number, a unique integer assigned by Kafka and used to track until where the consumers have read the messages. This is the partition offset.


## Infrastructure

Kafka runs in a cluster of brokers. Each broker is a Kafka instance with its own storage. A broker can be a node, a virtual machine, a docker compose service o a pod in a kubernetes cluster.

Kafka cluster is managed by an external cluster manager, `ZooKeeper`, who requires its own cluster of instances. Later, another cluster manager, `KRaft`, has been introduced. KRaft runs inside the Kafka cluster, reducing the operational complexity.

Both ZooKeeper and KRaft implements a consensus protocol that establish who, among the Kafka brokers in the cluster, is the master one.


## Producer

A producer is an application that uses the Kafka Producer API to write events to Kafka topics.

Producers writes to leader partition(s).


## Consumer

A consumer is an application that uses the kafka Consumer API to wait (via polling) for new messages arrive from 1..N topics. The consumer specify which topics is interested into.

Consumers work in groups and consumer groups are handled by Kafka. Each consumer group has a unique ID (`group id`). Every consumer with the same group id belong to the same group.

During the configuratiob phase, the Kafka consumer groups manager assign one or more partitions to each consumer in the same consumer group.

Consumers never stop; they run in a infinite loop waiting for the arrival if new messages.

By default, consumers read from the leader partition but, in multi region/AZ/data center, it can be configured to read from partition followers.

Consumers have their own topic offset.

## Security

Kafka default security level is very low.

* Encryption in transit is optional and off in the default configuration. Here transit means from producer to broker, between brokers, from broker to consumer)
* Authentication and authoriation are options and off in the default configuration
* Encryption at reset (partition segments) is now available in the standard Apache Kafka (available in the Confluent version). Workaround 1: encrypt the disk. Workaround 2: encrypt/decrypt the messages using the producers/consumers
  