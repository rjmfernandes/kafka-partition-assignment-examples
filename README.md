# Partition assignment example

This is a fork for readme purposes of the original Tomas' repository https://github.com/tomasalmeida/kafka-partition-assignment-examples

See the behavior of the partition assignment in different configuration

## Pre-requisites
* docker-compose
* [Confluent CLI](https://docs.confluent.io/platform/current/installation/installing_cp/zip-tar.html)

## How to run

### 1. Producer

In one terminal, start the cluster and producer (you will be asked some questions):

```bash
./launch-producer.sh
```

Example:

```text
      ./launch-producer.sh
      [+] Running 3/3
      ⠿ Network kafka-partition-assignment-examples_default  Created                <-- cluster being started                                                                                                                      0.0s
      ⠿ Container zookeeper                                  Started                                                                                                                                      0.4s
      ⠿ Container broker                                     Started                                                                                                                                      0.7s
      
      Should I create a new topic? [y/n]: y
      How many partitions? 1
      Created topic topic-0.
      
      Should I create a new topic? [y/n]: y
      How many partitions? 3
      Created topic topic-1.
      
      Should I create a new topic? [y/n]: n
      all topics = topic-0,topic-1
      
      ... # producer will be started
```


### 2. Status
In another terminal, check the status

```bash
 ./status.sh r3
```

### 3. Consumers

And in another split terminal, launch the consumer group

      ./launch-consumer.sh <strategy> <initial consumers> <consumer-group> <static-assignment>

For example:

```bash
./launch-consumer.sh coop 3 r3 true
```
     
Strategies can be:
* round: https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/consumer/RoundRobinAssignor.html
* range: https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/consumer/RangeAssignor.html
* sticky: https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/consumer/StickyAssignor.html
* coop: https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/consumer/CooperativeStickyAssignor.html

`static-assignment` can be true or false (false by default) (timeout is 10 seconds)

### Clean up

1. Stop **gently** all scripts
2. Run

```bash
docker-compose down -v
```
    

# References:
- https://medium.com/streamthoughts/understanding-kafka-partition-assignment-strategies-and-how-to-write-your-own-custom-assignor-ebeda1fc06f3
