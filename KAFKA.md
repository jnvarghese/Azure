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
