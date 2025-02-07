# System Design Interview Preparation Notes

Below are the main categories for the System Design Interview

**Table of Contents** 
- [System Design Interview Preparation Notes](#system-design-interview-preparation-notes)
    - [1) Communication](#1-communication)
    - [2) Designing at Scale](#2-designing-at-scale)
    - [System Properties](#system-properties)
      - [Latency(T):](#latencyt)
      - [Throughtput](#throughtput)
      - [Availability](#availability)
      - [Storage](#storage)
    - [3) Concrete and Quantitative Solution](#3-concrete-and-quantitative-solution)
    - [Solution Patterns](#solution-patterns)
      - [Sharding Data](#sharding-data)
      - [Replication Types](#replication-types)
      - [Write Ahead Logging (WAL)](#write-ahead-logging-wal)
      - [Separating Data and Metadata Storage](#separating-data-and-metadata-storage)
      - [Types of Load Distribution/Load Balancing Algorithms](#types-of-load-distributionload-balancing-algorithms)
    - [4) Tradeoffs and Compramises](#4-tradeoffs-and-compramises)
    - [5) Best Practices](#5-best-practices)
    - [6) Useful Links\*\*](#6-useful-links)

### 1) Communication

- Listen the requirement
- Ask Clarifying questions
- Understanding problem & design the solution</br>
- Dig deeper & ask clarifying questions</br>
- How you arrived is IMP</br>

### 2) Designing at Scale

- Explain How the system is working</br>
- Any bottleneck in the design</br>
- If there are multiple components, what are the APIs and how do they work together?</br>
- How to provide great service to all</br>
- They are not expecting one & perfect solution
- Ask How many requests per second the system can handle
- During the interview: Large dataset given for Sharding</br>
- During the interview: Identify fastest machine among it and discard rest</br>
- **Short Polling:** 
  - client making requests to resources continuously short cycles (every 3 secs). Bad idea
- **Long Polling:** - client request to resources, keeps conn open untill server responds. recomanded when low traffic
- **WebSockets:** - ex: client to resources conn open & bidirectional comminication. ex: web chat application

### System Properties

#### Latency(T):
  - time to perform packet transfer accross n/w
  - measured in ms/s/min/hours, calculated in average
  - use cache or In-memory rather than heavy operation
  - 1/T = f (Frequency)
  - **What Causes Latency?** 
    -  Physical distance
    -  Complex computation - expensive operation
    -  Congestion - too many requests
    -  Too many nodes
  - **How to Improve Latency?** 
    -  Better paths - minimize request travel path
    -  Caching - dramatically improves
    -  Protocol choice - HTTP/2 or TCP with low congresion logic
#### Throughtput
  - mount of data that can be sent per unit
  - measured in TPS, calculated in average
  - use concurreny to achieve Throughtput
  - **What Causes Low Throughtput?** 
    -  Congestion
    -  Protocol overhead
    -  High Latency
  - **How to Improve Throughtput?** 
    -  Increasing bandwidth 
    -  Improving latency
    -  Protocol choice - use TCP congestion avoidance feature
#### Availability
  - amount of time that a system is available
  - Uptime / (Uptime + Downtime)
  - **What Causes Low Availability?** 
    -  Hardware failure
    -  Software bugs
    -  Complex architectures
    -  Dependent service outages
    -  Request overload
    -  Deployment issues
  - **How to Improve Availability?** 
    -  Failover systems
    -  Clustering
    -  Backups & replications
    -  Geographic redundancy
    -  Automatic testing, deployment, and rollbacks 
#### Storage
  - File Storage
  - Block Storage
    - HDD
    - SSD
  - Object Storage

### 3) Concrete and Quantitative Solution

- Reality and Laws of Physics</br>
- Cost of Operations such as...</br>
  - **Read from Disk** **HDD or SSD. Which is better?:**
    - storage is non-volatile, RAM is directly connected to the CPU on a wide and fast bus speed.
    - storage is much slower than memory, used for persistent </br>
  - **Read from memory?:**
    - RAM is olatile when poweroff data will be lost.
    - Read and Write Speed is fast, used for Cache
  - **Local Area Network (LAN) round-trip ?:**
    - Called as RTT duration in milliseconds (ms).
    - Takes n/w request from Source => Destination => back to Source
    - Reducing RTT is a primary goal of a CDN. latency can be measured in the reduction of RTT
  - **Cross-Continental Network?:**
    - Internet exchange points(IXP) IXP physical location through which DNS and CDN.
    - Exists at Edge locations (works as transit) - Edge locations reducing latency, improving round-trip time, and potentially reducing costs
- During the interview: estimate the resources to run & Diagram

### Solution Patterns

#### Sharding Data
  - distributes data across different db's
  - benefit is... less R/RW traffic, less replication and more cache hits
  - benefit is... less Indexing space & faster queries
  - common ways is based on User last name & geo location etc
  - **downsides:** complex queries, joins, app logic to handle sharding, increase complexity and more hadware
#### Replication Types
  - Ideally replication is Sync or Async
  - **Snapshot Replication:** copies a "snapshot" of the database. Useful when data doesnot change.
  - **Transactional Replication:** full copy of the database, data copied realtime, incremental and order
  - **Merge Replication:** combines data from several sources into a single database. useful to discover and address conflicting changes
  - **Peer to Peer Replication:** based on Transactional but near real-time between multiple servers. useful for web applications
  - **Bi-directional Replication:** transactional replication topology. server publishes data and then subscribes to a publication with the same data from the other server

#### Write Ahead Logging (WAL)
  - method for ensuring data integrity, quickly identify risky data loss (for DBAs)
  - write transaction log ahead of data files will be written
  - when modification occurs 1st change will be made in memory, then written to transaction log
  - If write to the transaction Log success then data will be written.
  - used for recovery model to identify how much info & how long data will remain
  
#### Separating Data and Metadata Storage
  - for robustness, scalability and efficiency
  - metadata describes the structure of the data
  - tells the system how to render, cache, decompress, language
  - seperation of concerns, protect data and analytics

#### Types of Load Distribution/Load Balancing Algorithms
  - distributing tasks over a set of computing nodes
  - for performance and reliability
  - horizontal dynamic scaling, Abstraction, throughtput, availability and
  - L4 & L7 Load Balancers
  - eliminate a single point of failure, SSL Termination and Sticky Session
  - **Round robin** 
    - sequentially diverting traffic to servers
  - **Weighted Round robin** 
    - diverting traffic based on server characteristics
  - **Random** 
    - traffic to random servers, weighted or requests etc
  - **User IP Hashing** 
    - session stickiness based on user IP 
  - **Least Connection/Least Load**
    - send traffic to least connections of the server
  - **Fastest**
    - send traffic to those servers that respond the quickly 
  - **Observed** 
    - current connections + the response time
  - **Predictive**
    - predict which server will perform well based on rank, more rank more traffic   
  - **URL Hashing** - requested content to server
  - **power of d** 
    - mulitple LB's. LB in the pool sends a request to the least busy server
    - nginx uses d=2

### 4) Tradeoffs and Compramises

How system responses various failures</br>
Multiple solutions, commit to one and iterate on it</br>

### 5) Best Practices

- Explain the thought process</br>
- Clarify: Many questions will be deliberately open-ended to get an idea of how you solve technical problems
- Improve: Think & Explain ways to improve</br>
- Practice: Practice on white board

### 6) Useful Links\*\*

- <https://github.com/donnemartin/system-design-primer>
- <https://github.com/ted-ly/system-design-interview>
- <https://gist.github.com/vasanthk/485d1c25737e8e72759f>
- <https://github.com/madd86/awesome-system-design>
- <https://github.com/codersguild/System-Design>
- <https://github.com/yangshun/tech-interview-handbook/blob/master/experimental/design/README.md>
- <https://github.com/shashank88/system_design>
- <https://github.com/checkcheckzz/system-design-interview>
- <https://github.com/puncsky/system-design-and-architecture>
- https://salmaeng71.medium.com/big-o-notation-cheat-sheet-4a7e5632c93e
- 	https://static.packt-cdn.com/downloads/4874OS_Appendix_Big_O_Cheat_Sheet.pdf

* https://craigndave.org/wp-content/uploads/2020/09/big-o-notation-cheat-sheet.pdf

* https://gist.github.com/vasanthk/485d1c25737e8e72759f
* https://medium.com/the-legend/system-design-trd-cheatsheet-aff03e823afc
* https://www.naniz.co/posts/2020/12/system-design-algorithm-and-interview-cheat-sheet/
