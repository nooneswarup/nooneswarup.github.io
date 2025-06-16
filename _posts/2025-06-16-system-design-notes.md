---
layout: post
title: "System Design Concepts"
date: 2025-06-16 00:58 -0500
author: nooneswarup
categories: [System Design]
tags: [engineering, architecture, cloud]
---

### 1. Functional Requirements
Define the scope of the system by asking clarifying questions. This involves understanding the features, user interactions, and specific functionalities required.

### 2. Non-Functional Requirements
These are the attributes of a system.
* **Latency:** What is the maximum acceptable delay for a response?
* **Availability:** Does the system need to be highly available (e.g., `99.99%` uptime)?
* **Consistency:** Should the system prioritize strong consistency (all clients see the same data at the same time) or eventual consistency? This often varies between different microservices.
* **Durability:** Should data, once saved, be permanent and survive system failures?
* **Scalability:** How will the system handle growth in users, data, and traffic?

#### The CAP Theorem
The CAP Theorem states that a distributed system can only guarantee two of the following three properties during a network failure:
* **Consistency:** Every read receives the most recent write or an error.
* **Availability:** Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
* **Partition Tolerance:** The system continues to operate despite network partitions (messages being dropped or delayed between nodes).

**Rule of Thumb:** Since network failures are inevitable in distributed systems, you must choose between prioritizing Consistency or Availability.

### 3. Estimation & Back-of-the-Envelope Math
* **Users & Traffic:**
    * **Daily Active Users (DAU):** Often estimated as `30%` of Monthly Active Users (MAU).
    * **Read/Write Ratio:** The ratio of read operations to write operations (e.g., 100:1 for a news feed).
    * **Peak Traffic:** Typically `2x` to `5x` the average daily traffic.
* **Data & Retention:**
    * **Data Retention:** How long to store data (e.g., 3-5 years).
    * **Replication:** Number of data copies (e.g., `3` replicas for durability).
* **Quick Conversions:**
    * **Time:** 1 day ≈ 100,000 seconds (actually 86,400).
    * **Data Sizes:** `kb` (10³), `mb` (10⁶), `gb` (10⁹), `tb` (10¹²), `pb` (10¹⁵ bytes).
* **Traffic Estimates (QPS - Queries Per Second):**
    * **Write QPS:** `(Total Write Requests per Day) / (Seconds in a Day)`
    * **Read QPS:** `(Total Read Requests per Day) / (Seconds in a Day)`
* **Storage Estimates:**
    * **Daily Storage:** `(Users) * (Writes per User per Day) * (Size per Write)`
    * **Total Storage:** `(Daily Storage) * (365) * (Retention Years) * (Replicas)`
* **Memory Estimates (for Cache):**
    * Identify data for caching (e.g., hot read items).
    * Apply the **80/20 rule**: 20% of the data often generates 80% of the traffic.
    * **Cache Size:** `(Number of Items to Cache) * (Average Size per Item)`

### 4. High-Level Design (HLD) Components

* **Reverse Proxy:** Hides server addresses and can handle SSL termination and compression.
* **Load Balancer:** Distributes traffic across servers to prevent bottlenecks.
    * **Types:** Active-Passive, Active-Active.
    * **Layers:** Layer 4 (TCP level) and Layer 7 (HTTP level, allows for smarter routing).
* **API Gateway:** A single entry point for all clients (e.g., `AWS API Gateway`, `Kong`). Manages authentication, authorization, routing, monitoring, rate limiting, and logging.
* **Caching Strategies:** Store frequently accessed data in memory for fast lookups.
    * **Cache Layers:** Browser, CDN, Webserver, Application, Databases.
    * **Cache Invalidation & Eviction Policies:**
        * **Cache Aside:** The application manages reading/writing from storage and updating the cache.
        * **Write-Through:** Application writes to the cache, which then writes to the database. Consistent but slower writes.
        * **Write-Behind:** Application writes to the cache, which acknowledges immediately and updates the DB asynchronously. Fast writes, but risk of data loss on cache failure.
* **Distributed Cache:** `Redis`, `Memcached`.
* **CDN (Content Delivery Network):** Caches static assets (`JS`, `CSS`, images) at edge locations closer to users. (e.g., `Cloudflare`, `AWS CloudFront`).

### 5. Databases & Storage

* **Relational (SQL):** For structured data requiring ACID transactions and complex queries.
* **NoSQL:**
    * **Key-Value:** `DynamoDB`, `Redis`. Fast lookups.
    * **Document:** `MongoDB`. Flexible JSON-like documents.
    * **Graph:** `Neo4j`. For data with complex relationships.
* **Search Engine:** `Elasticsearch` for full-text search.
* **In-Memory Database:** `Redis` for caching, session management.
* **Queue:** Acts as a buffer to decouple services (`AWS SQS`, `RabbitMQ`).
* **Stream/Event Processing:** Process data in near-real-time (`Kafka`, `Flink`).
* **Change Data Capture (CDC):** Streams database changes to other systems.
* **Object Storage:** `AWS S3` for unstructured data like videos and images.

#### In-Depth Look: PostgreSQL
* **Model:** Tables, rows, columns with a predefined schema.
* **Pros:** Full ACID compliance, great for complex queries, open source, mature.
* **Cons:** Horizontal scaling (sharding) is complex, schema changes are difficult.
* **Managed Solution:** `AWS RDS`.

#### In-Depth Look: DynamoDB
* **Model:** Schema-less tables with items (rows) and attributes (columns). Requires a Partition Key.
* **Pros:** Fully managed, massively scalable, supports ACID, has built-in CDC (DynamoDB Streams) and an optional cache (DAX).
* **Cons:** Not for complex queries/joins, can be expensive, vendor lock-in.

#### In-Depth Look: AWS S3 (Cloud Object Storage)
* **Use Case:** Store large files, images, videos, logs.
* **Key Features:**
    * **Multi-part Upload:** Upload large files in parallel chunks.
    * **Pre-signed URLs:** Generate temporary, secure URLs for direct user uploads/downloads, offloading bandwidth from your servers.
    * **S3 Notifications:** Trigger processes (e.g., a Lambda) on file upload to update metadata.

#### In-Depth Look: Kafka (Distributed Event Streaming)
* **Use Case:** High-throughput, durable message queue or real-time stream processing.
* **Core Concepts:**
    * **Producer/Consumer:** Applications that write/read messages (events).
    * **Topic:** A category to which messages are published.
    * **Partition:** A topic is split into partitions for parallel processing and scalability.
    * **Consumer Group:** Guarantees each message is delivered to only *one* consumer within the group.

### 6. Monitoring, Logging & Alerting
* **Monitoring:** Track system health (CPU, memory, latency) with tools like `Prometheus` and visualize with `Grafana`.
* **Logging:** Aggregate and analyze logs using stacks like `ELK` (Elasticsearch, Logstash, Kibana).
* **Alerting:** Notify developers of issues via `Slack`, PagerDuty, etc.

### 7. Networking Fundamentals
* **Protocols:** `IP` (addressing), `TCP` (reliable delivery), `UDP` (fast, less reliable), `HTTP` (web), `WebSockets` (real-time, two-way), `WebRTC` (peer-to-peer).
* **DNS (Domain Name System):** Translates domain names to IP addresses.
* **Common Ports:** `HTTP: 80`, `HTTPS: 443`, `SSH: 22`.
* **HTTP Status Codes:** `2xx` (Success), `3xx` (Redirection), `4xx` (Client Error), `5xx` (Server Error).

### 8. API Design
* **Paradigms:** `REST API`, `GraphQL`, `gRPC`.
* **Best Practices:**
    * **Versioning:** `/api/v1/...`
    * **Pagination:** `limit` and `offset` or cursor-based.
    * **Filtering:** Allow filtering by parameters.
    * **Rate Limiting:** Protect from abuse.
    * **CORS (Cross-Origin Resource Sharing):** Securely allow requests from other domains.

### 9. Common System Design Problems
1.  Web Crawler
2.  Rate Limiter
3.  Distributed Cache
4.  Message Queue
5.  E-commerce Website (e.g., Amazon)
6.  Collaborative Editor (e.g., Google Docs)
7.  Proximity Service (e.g., Yelp)
8.  Typeahead / Autocomplete
9.  Ride-Sharing App (e.g., Uber)
10. Video Streaming Service (e.g., Netflix)
11. URL Shortener (e.g., TinyURL)
12. Cloud File Storage (e.g., Dropbox)
13. On-Demand Delivery (e.g., Gopuff)
14. Chat Application (e.g., WhatsApp)
15. Leaderboard (e.g., Leetcode)
16. Social Media Feed (e.g., Facebook, Twitter)
17. Ticket Booking System (e.g., Ticketmaster)
18. Post Search (e.g., Facebook Post Search)
19. Youtube
20. Stripe
21. FB live comments
22. Tinder
23. Google Maps
24. Online Banking
