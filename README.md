# OpenMessaging Benchmark Framework

## Java version

```
java -version
openjdk version "11.0.24" 2024-07-16
OpenJDK Runtime Environment Homebrew (build 11.0.24+0)
OpenJDK 64-Bit Server VM Homebrew (build 11.0.24+0, mixed mode)
```

## Maven version

```
mvn --version
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /opt/homebrew/Cellar/maven/3.9.9/libexec
Java version: 22.0.2, vendor: Homebrew, runtime: /opt/homebrew/Cellar/openjdk/22.0.2/libexec/openjdk.jdk/Contents/Home
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "14.5", arch: "aarch64", family: "mac"
```

## Build command

```
mvn clean install -Dspotless.apply.skip -Dspotbugs.skip=true -Dlicense.skip=true -Dspotless.skip=true -DskipTests
```

## Running the benchmark

This is intended to run a single producer, single consumer benchmark locally with Kafka running on the same host:
1. Start a single node kafka cluster
2. Start a worker in one terminal 
```
sudo ./bin/benchmark-worker
```
This will start a worker listening on port 8080 and stats port of 8081
3. Start another worker in another terminal
```
sudo ./bin/benchmark-worker --port 8082 -sp 8083
```
4. Start the driver in a seperate terminal
```
sudo bin/benchmark \
  --drivers driver-kafka/kafka-exactly-once.yaml \
  --workers http://127.0.0.1:8080,http://127.0.0.1:8082 \
  workloads/1-topic-1-partition-1kb.yaml
```

## Things to keep in mind to get it running

If you see things running but topics not getting created, check the workload config. Some configs have `replicationFactor`
and `min.insync.replicas` set to greater than 1. We are only running a single node cluster, so you never receive an ack.
Change these to 1 if running with `acks=all`.

---

[![Build](https://github.com/openmessaging/benchmark/actions/workflows/pr-build-and-test.yml/badge.svg)](https://github.com/openmessaging/benchmark/actions/workflows/pr-build-and-test.yml)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

**Notice：** We do not consider or plan to release any unilateral test results based on this standard. For reference, you can purchase server tests on the cloud by yourself.

This repository houses user-friendly, cloud-ready benchmarking suites for the following messaging platforms:

* [Apache ActiveMQ Artemis](https://activemq.apache.org/components/artemis/)
* [Apache Bookkeeper](https://bookkeeper.apache.org)
* [Apache Kafka](https://kafka.apache.org)
* [Apache Pulsar](https://pulsar.apache.org)
* [Apache RocketMQ](https://rocketmq.apache.org)
* Generic [JMS](https://javaee.github.io/jms-spec/)
* [KoP (Kafka-on-Pulsar)](https://github.com/streamnative/kop)
* [NATS JetStream](https://docs.nats.io/nats-concepts/jetstream)
* [NATS Streaming (STAN)](https://docs.nats.io/legacy/stan/intro)
* [NSQ](https://nsq.io)
* [Pravega](https://pravega.io/)
* [RabbitMQ](https://www.rabbitmq.com/)
* [Redis](https://redis.com/)

> More details could be found at the [official documentation](http://openmessaging.cloud/docs/benchmarks/).

## Build

Requirements:

* JDK 8
* Maven 3.8.6+

Common build actions:

|             Action              |                 Command                  |
|---------------------------------|------------------------------------------|
| Full build and test             | `mvn clean verify`                       |
| Skip tests                      | `mvn clean verify -DskipTests`           |
| Skip Jacoco test coverage check | `mvn clean verify -Djacoco.skip`         |
| Skip Checkstyle standards check | `mvn clean verify -Dcheckstyle.skip`     |
| Skip Spotless formatting check  | `mvn clean verify -Dspotless.check.skip` |
| Format code                     | `mvn spotless:apply`                     |
| Generate license headers        | `mvn license:format`                     |

## License

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
