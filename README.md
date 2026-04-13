# Python Jupyter Notebooks for Apache Kafka®

A hands-on series of Jupyter Notebooks for learning core Apache Kafka concepts with Python. Everything runs locally via Docker — no cloud account required.

If you have questions or improvement suggestions, please open an issue. Contributions are welcome!

## Quick Start

1. Clone the repository
2. Start the local environment:

```bash
docker compose up -d
```

3. Open JupyterLab at **http://localhost:8888**
4. Open the `work` folder and start with **00 - Local Setup.ipynb**

## Docker Services

`docker compose up` starts four services:

| Service | Description | Port |
|---|---|---|
| `kafka` | Apache Kafka 4.1.1 — KRaft mode (no Zookeeper) | `29092` (host-mapped) |
| `postgres` | PostgreSQL 16 (used by the Kafka Connect notebook) | `5432` |
| `kafka-connect` | Kafka Connect with JDBC Sink connector | `8083` |
| `jupyter-notebook` | JupyterLab | `8888` |

> **Note:** `kafka-connect` downloads the JDBC connector plugin on first start, which takes about a minute. The other services are ready in ~15 seconds.

## Notebook Overview

| Notebook | Topic |
|---|---|
| [00 - Local Setup](00%20-%20Local%20Setup.ipynb) | Install libraries and create the local connection config |
| [01 - Producer](01%20-%20Producer.ipynb) | Create a Kafka Producer and publish messages |
| [02 - Consumer](02%20-%20Consumer.ipynb) | Read messages with a Kafka Consumer |
| [03-00 - Partition Producer](03%20-%2000%20-%20Partition%20Producer.ipynb) | Create a partitioned topic and send messages to specific partitions |
| [03-01 - Consumer Partition 0](03%20-%2001%20-%20Consumer%20-%20Partition%200.ipynb) | Read from partition 0 |
| [03-02 - Consumer Partition 1](03%20-%2002%20-%20Consumer%20-%20Partition%201.ipynb) | Read from partition 1 |
| [04 - New Consumer Group](04%20-%20New%20Consumer%20Group.ipynb) | Consumer groups and offset management |
| [05 - Kafka Connect](05%20-%20Kafka%20Connect.ipynb) | Stream data into PostgreSQL with the JDBC Sink connector |
| [0N - Cleanup](0N%20-%20Cleanup.ipynb) | Delete all topics created during the session |

## Notebook Details

### Produce and Read Messages

![Producer](images/producing.png)

**01 - Producer.ipynb** creates a Python Kafka Producer and publishes messages to a topic. After producing the first message, open **02 - Consumer.ipynb** alongside it.

![Place consumer alongside the producer](images/move-consumer.gif)

**02 - Consumer.ipynb** reads from the topic from the point it attaches — it does not replay history. To read messages already produced, start the consumer cell *before* producing. This is Kafka's default behaviour and can be changed by adding `auto_offset_reset='earliest'` to the consumer config.

![Consumer](images/consumer.png)

### Understanding Partitions

![Partitions](images/partitions.png)

**03-00 - Partition Producer.ipynb** creates a topic with two partitions using `KafkaAdminClient` and sends a message to each. Open **03-01** and **03-02** side by side to read from partition 0 and partition 1 independently.

### Consumer Groups

![Consumer groups](images/consumer_groups.png)

Messages in Kafka are not deleted when read — they remain available for other consumers. **04 - New Consumer Group.ipynb** creates a consumer in a new group with `auto_offset_reset='earliest'`, reading the full topic history from the start. Once both consumers are running, messages sent from **01 - Producer** arrive in both.

### Kafka Connect

![Kafka Connect](images/connect_pg.png)

**05 - Kafka Connect.ipynb** sends JSON messages with an embedded schema to a topic, then registers a JDBC Sink connector via the Kafka Connect REST API to stream those messages into the local PostgreSQL database.

### Cleanup

**0N - Cleanup.ipynb** deletes all topics created during the session using `KafkaAdminClient`.

# License

This project is licensed under the [Apache License, Version 2.0](LICENSE).

Apache Kafka is either a registered trademark or trademark of the Apache Software Foundation in the United States and/or other countries. Aiven has no affiliation with and is not endorsed by The Apache Software Foundation.
