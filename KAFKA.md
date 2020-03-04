KAFKA Commands

bin/zookeeper-server-start etc/kafka/zookeeper.properties

bin/kafka-server-start etc/kafka/server.properties

bin/kafka-topics --create --topic my_topics --zookeeper localhost:2181 --replication-factor 1 --partitions 1

bin/kafka-console-producer --broker-list localhost:9092 --topic my_topics

bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic my_topics --from-beginning

Fault Tolerance



bin/kafka-topics --create --topic replicated_topic --zookeeper localhost:2181 --replication-factor 3 --partitions 1

bin/kafka-topics --describe  --topic replicated_topic --zookeeper localhost:2181

bin/kafka-console-producer --broker-list localhost:9092,localhost:9093,localhost:9094  --topic replicated_topic

bin/kafka-console-consumer --bootstrap-server localhost:9092,localhost:9093,localhost:9094 --topic replicated_topic --from-beginning


Topic is a logical name with 1 or more partitions and partitions are replicated. Order is guarantied for a partitions. Writes are to the Leader of the partition.	
KTble is an in memory representation of data in a Kafka topic.
Kafka Connector - is a system/a framework to get the legacy data into the topic/events,.

Derive materialized view instead of caching, 
Free state management if you use k-streams, if one of the instance is going down , Kafka streams will have persisted that internal state to a Kafka topic inside cluster for the next instance to pick it
Start simple and Evolve-
1. Broadcast events to a Kafka topic
2. Retain them in log
3. Evolve the event stream with streaming services/functions
4. Recast the event stream into memorialized view when you need to query.

Example Code - http://bit.ly/kafka-microservice-examples
Https://github.com/jamesward/koober


A sidecar proxy is an application design pattern which abstracts certain features, such as inter-service communications, monitoring and security, away from the main architecture to ease the tracking and maintenance of the application as a whole.A sidecar proxy is attached to a parent application to extend or add functionality.Sidecar proxies are typically used within the service mesh control plane (CP), microservices or containers.

A service mesh is divided into a data plane (DP) and a control plane. The service mesh DP is responsible for the communication of services within the mesh and can provide features such as load balancing, encryption and failure recovery through a separate, dedicated layer of infrastructure. A sidecar proxy is attached to the CP, which manages and configures each sidecar in relation to its designated service. All network traffic from an individual service is filtered through the sidecar proxy, which operates as its own infrastructure layer.
