# Windows 10 Local Confluent Kafka Docker Guide

## Kafka Setup

auto create topics enable: true

delete topic enable: true

log retention: 15 minutes

log retention check interval: 15 minutes

replication factor: 3

number of partitions: 3

*Zookeeper* localhost:22181,localhost:32181,localhost:42181

*Zookeeper* zookeeper-1,zookeeper-2,zookeeper-3

*Kafka* localhost:30000,localhost:30001,localhost:30002

*Kafka* kafka-1,kafka-2,kafka-3


## Prerequisites:
Assumes you have the basic knowledge of docker and kafka

Docker-compose installed

## Running docker containers
Move to current directory

`docker-compose up -d`

# Test produce/consume examples:

Produce 17 messages in topic `epic`:
`docker-compose exec kafka-1 bash -c "seq 17 | kafka-console-producer --request-required-acks 1 --broker-list localhost:30000,localhost:30001,localhost:30002 --topic epic && echo 'Produced 17 messages.'"`

Produce your own message in topic `epic`:
`docker-compose exec kafka-1 bash -c "kafka-console-producer --broker-list localhost:30000,localhost:30001,localhost:30002 --topic epic"`

**Note** You may see `LEADER NOT AVAILABLE` upon first time producing, can ignore if you can consume the message from below

Consume messages from topic `epic`
`docker-compose exec kafka-1 kafka-console-consumer --bootstrap-server localhost:30000,localhost:30001,localhost:30002 --topic epic --new-consumer --from-beginning --max-messages 20`

List Topics:
`docker-compose exec zookeeper-1 kafka-topics --zookeeper localhost:22181,localhost:32181,localhost:42181 --list`

Delete Topic:
`docker-compose exec zookeeper-1 kafka-topics --zookeeper localhost:22181,localhost:32181,localhost:42181 --delete --topic epic`

**Note** requires to be in directory with `docker-compose.yml` file, otherwise use <docker exec ...>

## Stop Services:

`docker-compose down`
