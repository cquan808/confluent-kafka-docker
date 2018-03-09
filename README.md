# Windows 10 Local Confluent Kafka Docker Guide

## Prerequisites:
-Assumes you have the basic knowledge of docker and kafka
-Docker-compose installed

## Running docker containers
Move to current directory

`docker-compose up -d`

*Zookeeper* localhost:22181,localhost:32181,localhost:42181
*Zookeeper* zookeeper-1,zookeeper-2,zookeeper-3
*Kafka* localhost:30000,localhost:30001,localhost:30002
*Kafka* kafka-1,kafka-2,kafka-3

# Test produce/consume examples:

Produce 17 messages in topic `dope`:
`docker-compose exec kafka-1 bash -c "seq 17 | kafka-console-producer --request-required-acks 1 --broker-list localhost:30000,localhost:30001,localhost:30002 --topic dope && echo 'Produced 17 messages.'"`

Produce your own message in topic `dope`:
`docker-compose exec kafka-1 bash -c "kafka-console-producer --broker-list localhost:30000,localhost:30001,localhost:30002 --topic dope"`

**Note** You may see `LEADER NOT AVAILABLE` upon first time producing, can ignore if you can consume the message from below

Consume messages from topic `dope`
`docker-compose exec kafka-1 kafka-console-consumer --bootstrap-server localhost:30000,localhost:30001,localhost:30002 --topic foo --new-consumer --from-beginning --max-messages 20`
