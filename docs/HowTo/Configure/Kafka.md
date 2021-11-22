---
Description: How to configure Kafka
---

# Apache Kafka

ConsenSys Rollups uses [Apache Kafka](https://kafka.apache.org/) as the message broker and streaming
platform to submit and listen for requests, and get information about batches.

Information about batches is accumulated asynchronously, and notifies the user when a request is
submitted and finalized onchain.

Configure Kafka access for the [engine](Configuration-File.md#engine-configuration-file) and
[manager](Configuration-File.md#manager-configuration-file) components separately.

!!! example "Kafka configuration example"

    ```toml
    [kafka]
        kafka_type = "Real"
        brokers = "localhost:9092"
        timeout_ms = 5000
        batch_updates_topic = "sumo-state-updates"
    ```

If using [high availability (HA)](../../Concepts/High-Availability.md), configure the additional Kafka HA settings in the
[`[ha.kafka]`](../../Reference/Configuration-File.md#hakafka) section of the configuration file.
