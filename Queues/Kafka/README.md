# CheatSheets Kafka

## Table of Contents
- [What is Kafka?](#what-is-kafka)
- [Kafka TOP 5 Use Cases](#kafka-top-5-use-cases)
- [Kafka with Spring](#kafka-with-spring)


## What is Kafka?
Apache Kafka is a distributed data streaming platform that can publish, subscribe to, store, and process streams of records in real time. It is designed to handle data streams from multiple sources and deliver them to multiple consumers. In short, it moves massive amounts of data—not just from point A to B, but from points A to Z and anywhere else you need, all at the same time.

#### Reference
* https://www.redhat.com/en/topics/integration/what-is-apache-kafka


## Kafka TOP 5 Use Cases

![kafka_top_5_use_cases](https://github.com/SpaikSaucus/cheatsheets/blob/main/Queues/Kafka/Kafka_Top_5_use_cases.gif?raw=true)

1️⃣ Data Streaming: 🌊

Think of data as a fast-flowing river. We need a way to tap into that stream, harness its power, and direct it where it needs to go. That's exactly what Apache Kafka does! It empowers us to create real-time streaming applications, enabling them to transform or react to data swiftly as it flows between systems.

2️⃣ Log Aggregation and Web Activity Tracker: 🕵️‍♂️

Logs are generated in tons every second, and managing them can be a daunting task. But, no worries! Apache Kafka is here to save the day. It offers a unified, high-throughput, low-latency platform for handling real-time data feeds, making it a go-to choice for log aggregation.

3️⃣ Message Queue: 📨

In the era of microservices, an efficient message queue system is a must-have. Apache Kafka as a message queue delivers scalability, built-in fault tolerance, replication, and high durability, far surpassing traditional messaging systems.

4️⃣ Change Data Capture: 

Databases changing by the second? Meet Kafka's sidekick for Change Data Capture: Debezium! Together, they're like a dynamic duo. Debezium keeps an eye on your databases, capturing each change, while Kafka swiftly streams these updates to other systems. It's a real-time, super-efficient tag team, making sure your data is always in sync, 24/7!

5️⃣ Data Replication: 🔁

Apache Kafka is the trusted lieutenant for transferring data between different systems reliably. It serves as a robust and fault-tolerant bridge, ensuring that your data is replicated and synchronized across systems.

#### Reference:
* [Brij kishore Pandey: Top 5 Kafka use cases](https://www.linkedin.com/feed/update/urn:li:activity:7102613450475859968)


## Kafka with Spring

#### Reference:
* https://www.baeldung.com/spring-kafka