---
tags: [certification, confluent, kafka, apache]
---

# Confluent Certified Developer for Apache KAFKA (CCDAK)

*Last update: 09 Oct 2024*

## References

* [Apache Kafka documentation](https://kafka.apache.org/documentation/)
* [Confluent documentation](https://docs.confluent.io/index.html)
* [Confluent certification training](https://training.confluent.io/learningpath/confluent-certified-bootcamp-developer)
* [CloudGuru/Pluralsight training](https://www.pluralsight.com/cloud-guru/courses/confluent-certified-developer-for-apache-kafka-ccdak)
* [Spring Kafka](https://spring.io/projects/spring-kafka)
* [Baeldung Kafka series](https://www.baeldung.com/apache-kafka-series)

## Training

On the Confluence Training platform, suggested (free) courses are:

* Apache Kafka Fundamentals
* Confluent Fundamentals Accreditation
* Confluent Certified Bootcamp: Developer

## Introduction

Apache Kafka is an open source product developed by LinkedIn and now under Apache Foundation.

Confluent is a private company that gives commercial/enterprise support and provides additional tools to the Kafka ecosystem.

Key concepts:

* Events vs static data, flow vs storage. Kafka handle unlimited temporal series of events.
* Real time handling
* Events persistence

An event streaming platform has 2 primary uses:

* Stream processing: continuous real time data processing. Evolution of the Enterprise Messaging.
* Data integration: flow of data changes to align systems. A sort of streaming ETL.

Kafka solution is based on several components:

* the `broker`, the Kafka instance in the cluster which has its own storage. Usually there are more than one broker in the cluster, for fault tolerance.
* the `cluster manager`, that controls the brokers cluster, handling brokers failures and recoveries
* the `producers`, client applications that run outside the cluster and send events to the brokers
* the `consumers`, client applications that run outside the cluster and receive (pull) events from the brokers
* the `streams`, applications that process events, producing other events

Within Kafka, events are saved in named topics.

## Brokers

A broker is a Kafka instance running in a cluster.

The broker configuration is stored in the `server.properties` file, and it can be modified by command line arguments and the `AdminClient` API.

Some configuration parameters are `read-only` because they require a broker restart.

Configurations are "per-broker" o "cluster-wide".

## Topics and messages

A topic is a sequence (or stream) of events or messages. A broker can have an unlimited number of topics, each with a unique name.

A message has the following structure:

* timestamp, when the messages have been created or received (ingested) by Kafka
* key, optional, the key of the message
* value, the message body
* headers, optional message metadata

Each topic is implemented in 1 or more `partitions`.

Messages are distributed among the partitions using the hash of the message key. Messages with the same key are sent to the same partition. If the message does not have a key, the messages are sent to a random partition using a round-robin approach. It is also possible to define a custom partitioning logic.

IMPORTANT: messages are not deleted after being consumed. They remain in the topic for 1 week (default value for the retention period). Business or legal considerations influence the retention period for all topics in a cluster and/or for a specific topic.

A topic can be defined as `compacted`: only the most recent message with the same key will be maintained in the topics while the less recent messages will be deleted.

The topic configuration has broker-level defaults. It can be changed programmatically or via command line arguments.

## Partition

A partition is a log structure, an immutable and time ordered sequence of events; new events are always attached at the end of the partition.

Each partition can have 0 to N replicas, for fault tolerance. The number of replicas is the replication factor; its typical value is 3 because 3 is the typical number of brokers in the cluster. Replicas are distributed across the brokers, with one as leader and the others as followers.

The partitions are stored as files (`segments`) on the disk. No structured storage is required by Kafka.

The messages are guaranteed to be in temporal order at partition level but not a topic level.

Within a partition, each message has its own position number, a unique integer assigned by Kafka and used to track until where the consumers have read the messages. This is the partition offset.

## Infrastructure

Kafka runs in a cluster of brokers. Each broker is a Kafka instance with its own storage. A broker can be a node, a virtual machine, a docker compose service o a pod in a Kubernetes cluster.

Kafka cluster is managed by an external cluster manager, `ZooKeeper`, who requires its own cluster of instances. Later, another cluster manager, `KRaft`, has been introduced. KRaft runs inside the Kafka cluster, reducing the operational complexity.

Both ZooKeeper and KRaft implements a consensus protocol that establish who, among the Kafka brokers in the cluster, is the master one.

## Producer

A producer is an application that uses the Kafka Producer API to write events to Kafka topics.

Producers write to leader partition(s).

Producer properties:

* bootstrap.servers: the brokers
* key.serializer: how to serialize the message key
* value.serializer: how to serialize the message value
* acks: "all" (wait all in-sync replicas)

## Consumer

A consumer is an application that uses the Kafka Consumer API to wait (via polling) for new messages arrive from 1 to N topics. The consumer specify which topics are interested into.

Consumers work in groups and consumer groups are handled by Kafka. Each consumer group has a unique ID (`group id`). Every consumer with the same group ID belong to the same group.

During the configuration phase, the Kafka consumer groups manager assign one or more partitions to each consumer in the same consumer group.

Consumers never stop; they run in an infinite loop waiting for the arrival of new messages.

By default, consumers read from the leader partition but, in multi region/AZ/data center, it can be configured to read from partition followers.

Consumers have their own topic offset.

Consumer properties:

* bootstrap.servers: the brokers
* group.id: the ID of the consumer group
* key.serializer: how to serialize the message key
* value.serializer: how to serialize the message value
* enable.auto.commit: true or false

## Security

Kafka default security level is very low.

* Encryption in transit is optional and off in the default configuration. Here transit means from producer to broker, between brokers, from broker to consumer.
* Authentication and authorization are optional and off in the default configuration
* Encryption at rest (partition segments) is not available in the standard Apache Kafka, but it is available in the Confluent version. Workaround 1: encrypt the disk. Workaround 2: encrypt/decrypt the messages using the producers/consumers.

## Kafka Streams

Java API included in Apache Kafka.

It allows creating applications that run inside the Kafka cluster with an "exactly-once" semantic. Streams can be stateful with the state managed by the Kafka cluster.

## Kafka Connect and Connectors

It is a separate system with its own cluster. Its purpose is to:

1. Read data from "sources", writing messages to Kafka topics
2. Write data to "sinks", reading messages from Kafka topics.

The Connect cluster has several workers that use Kafka REST API to integrate with Kafka. Each worker handle 1 to N connections. The state of the Connect cluster is stored in a Kafka topic.

Each connector has its own configuration file and can have 1 to N tasks.

Structure: 1 connect cluster --> 1 to N workers --> 1 to N tasks

Connectors can be single instance/thread or multi-instance distributed among the workers.

Kafka Connectors are a set of ready-to-use components, maintained by Confluent or by the community.

## Confluent REST Proxy

REST API to produce and consume messages.

It is developed and maintained by Confluent, and it is free to use.

## Confluent Schema Registry

It is a configuration server dedicated to message schemas stored in a dedicated Kafka topic.

Producers mark the messages with a specific schema ID and the consumers check the message against the schema from the registry before processing it.

It is developed and maintained by Confluent, and it is free to use.

## Confluent KSQLDB

KSQLDB allows writing SQL like queries that filter, join and aggregates stream data producing new streams.

KQSLDB queries never stops. They can be used to create real-time stream processing applications.

KQSLDB runs on a dedicated server and expose its own REST API.

Use cases for KSQLDB:

* Streaming ETL
* Anomaly detection
* Events monitoring

## Confluent Platform

Functionalities not available in Apache Kafka:

* replication between clusters
* automatic balancing
* enterprise grade security
* cluster and topics monitoring

Products included in the platform which are free to use:

* KSQLDB
* REST Proxy
* Connectors
* Schema Registry

Products included in the platform which have commercial license:

* Replicator
* Auto Data balancers
* MQTT Proxy
* K8S operator
* Control Center

## Broker and topic design

The two main factors to be considered when design a **topic** are the partitions and the replication factor.

The brokers number in the cluster limit the replication factor: 3 brokers, max 3 replicas.

The number of consumers in the consume group limit the minimum number of partitions: 4 consumers, minimum 4 partitions.

Also, the memory (RAM) should be controlled.
