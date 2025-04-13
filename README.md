# Distributed Database Design for Supermarkets

A simple, scalable system design for managing checkout operations at supermarkets. The design is based on real-world assumptions and a sample dataset from GitHub.

## Overview

High customer traffic. Fast checkouts. Simple design.  
This project builds a checkout system that is both scalable and fault tolerant. I worked on similar projects before, and I learned that clear planning makes a big difference.

## Functional Requirements

The system covers the day-to-day tasks of a supermarket checkout:

- **Cart Management:**  
  - **Add products:** Quickly add items to your cart.  
  - **Remove products:** Remove items if needed.

- **Checkout:**  
  - Calculate prices.  
  - Process payments.  
  - Update inventory right away.

- **Analytics:**  
  - Use transaction data to run reports and analysis.

## Non-Functional Requirements

Key features that keep the system running smoothly:

- **Performance:**  
  - Supports **50 ops/sec** during peak times.
  
- **Availability:**  
  - High availability across multiple nodes.  
  - Uses a design that favors availability and partition tolerance (CAP theorem).

- **Scalability:**  
  - Easy to add more nodes as user traffic grows.
  
- **Fault Tolerance:**  
  - The system remains operational even if one or more nodes fail.
  
- **Data Retention:**  
  - Implements TTL (Time-To-Live) to clear data after 90 days.

## Architecture

The architecture is designed to handle high traffic and data from various locations:

- **Data Streaming:**  
  - Uses **Kafka** to ingest high volumes of transactions.  
  - Data from supermarket checkout events is streamed in real time.

- **Regional Data Centers:**  
  - Placed near retail stores to reduce latency.  
  - Each region acts like its own branch for better performance.

- **Database Layer:**  
  - **Cassandra** is chosen for its distributed nature and high write throughput.  
  - Data is stored in keyspaces with partition keys (based on branch).  
  - Clustering keys (like date/time and invoice ID) ensure data is sorted and unique.

- **Monitoring:**  
  - Tools like Prometheus (and Graphene) are used to track system health and performance.

- **Flow:**  
  - Producer scripts (written in Python) read data from the dataset and push it to Kafka.  
  - Consumer scripts pick up data from Kafka and insert it into Cassandra.  
  - The whole system is tested under various consistency levels (ALL, QUORUM, ANY).

## Data Modeling

- **Keyspace Creation:**  
  - One keyspace per data center.  
  - Replication factors are set according to node count.

- **Table Structure:**  
  - Partition key: **branch** (ensures data locality).  
  - Clustering keys: **date/time** and **invoice ID** for uniqueness and order.

## Implementation Summary

- **Producer-Consumer Model:**  
  - A Python-based producer reads 10 rows at a time from a CSV dataset and sends them to Kafka.
  - Consumers pick up the data and store it in Cassandra.

- **Testing:**  
  - Tested with manual inserts.  
  - Checked fault tolerance by simulating node failures.
  - Verified consistency levels.

- **Additional Features:**  
  - **TTL:** Automatically deletes records after 90 days.
  - **Monitoring:** Resource usage and data replication are tracked.

## Dataset Reference

The system uses a dataset from GitHub.  
Reference: [Supermarket Sales Dataset](https://github.com/plotly/datasets/blob/master/supermarket_Sales.csv)

---

This README gives an end-to-end view of the system. It covers the functional needs, the system’s demands, and the underlying architecture. Enjoy building your scalable checkout system!
