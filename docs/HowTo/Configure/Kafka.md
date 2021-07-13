---
Description: How to configure Kafka
---

# Apache Kafka

The operator uses [Apache Kafka](https://kafka.apache.org/) as the message broker and streaming
platform to submit and listen for requests, and get information about batches.

Information about batches is accumulated asynchronously, and notifies the user when a request is
submitted and finalized onchain.

Configure the operator to use Kafka in the [configuration file](Config-Files.md).

!!! example "Kafka configuration example"

    ```toml
    [kafka]
        kafka_type = "Real"
        brokers = "localhost:9092"
        timeout_ms = 5000
        batch_updates_topic = "sumo-state-updates"
    ```
