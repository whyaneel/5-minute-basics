# Setup
- Create a file `docker-compose.yml` with these 2 services (zookeeper,
  kafka)

```
version: "3"
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:5.5.1
    container_name: demo-zookeeper
    networks:
      - 5-minute-basics_default
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
    ports:
      - 2181:2181

  kafka:
    image: confluentinc/cp-kafka:5.5.1
    container_name: demo-kafka
    depends_on:
      - demo-zookeeper
    networks:
      - 5-minute-basics_default
    environment:
      KAFKA_ADVERTISED_HOST_NAME: kafka
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
    ports:
      - 9092:9092
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

networks:
  5-minute-basics_default:

```
- Run below command, to setup single node kafka cluster locally.
```
docker-compose up
```

# Quickstart
## Opens an interactive `bash` shell on the container
    docker exec -it demo-kafka bash

## Basics
### Create a Topic
    kafka-topics --zookeeper zookeeper:2181 --create --topic GREETINGS_TOPIC --partitions 3 --replication-factor 1 --if-not-exists
    
    kafka-topics --zookeeper zookeeper:2181 --create --topic TEMPORARY_TOPIC --partitions 6 --replication-factor 1 --if-not-exists

### List Topics
    kafka-topics --zookeeper zookeeper:2181 --list

### Describe a Topic
    kafka-topics --zookeeper zookeeper:2181 --describe --topic GREETINGS_TOPIC

### Delete a Topic
    kafka-topics --zookeeper zookeeper:2181 --delete --topic TEMPORARY_TOPIC

### Produce Messages to the Topic
    kafka-console-producer --broker-list kafka:9092 --topic GREETINGS_TOPIC

> Just type messages as you wish

> Ctrl + C to exit

### Consume Messages from the Topic
- Only new messages will be interpreted
```
kafka-console-consumer --bootstrap-server kafka:9092 --topic GREETINGS_TOPIC
```
- To get all messages
```
kafka-console-consumer --bootstrap-server kafka:9092 --topic GREETINGS_TOPIC --from-beginning
```

## Advanced
### Consumer Groups, for parallel processing of the messages (run in mutliple terminal windows)
The below is representation 3 instances of a consumer group GREETINGS_APPLICATION
```
kafka-console-consumer --bootstrap-server kafka:9092 --topic GREETINGS_TOPIC --group GREETINGS_APPLICATION

kafka-console-consumer --bootstrap-server kafka:9092 --topic GREETINGS_TOPIC --group GREETINGS_APPLICATION

kafka-console-consumer --bootstrap-server kafka:9092 --topic GREETINGS_TOPIC --group GREETINGS_APPLICATION
```

### List Consumer Groups
    kafka-consumer-groups --bootstrap-server kafka:9092 --list

### Describe Consumer Groups
To observe CURRENT-OFFSET,  LOG-END-OFFSET
```
kafka-consumer-groups --bootstrap-server  kafka:9092 --describe --group GREETINGS_APPLICATION
```

To reset offset to earliest (never run on PROD, unless you know the impact)
```
kafka-consumer-groups --bootstrap-server kafka:9092 --group GREETINGS_APPLICATION --reset-offsets --to-earliest --all-topics --execute
```

Run describe command again and observe the offset

:clap: Hope you learnt some basics of Topic, Producer, Consumer
