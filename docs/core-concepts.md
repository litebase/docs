---
section: Getting Started
---
# Core Concepts

{.text-lg}
Relational SQL Databases over HTTP

LitebaseDB is a data service with SQLite compatability. You can create or upload an existing database and immediately send queries to the database via HTTP requests.

## Querying the database

```sql
SELECT * FROM users WHERE verified_at IS NOT NULL LIMIT 15
```

### Statement bindings

...

## Available clients

We currently maintain the following 1st party clients:

* PHP
* Laravel
* NodeJS

### Contribute new clients

We are actively working on creating more clients to support as many languages
and frameworks as possible.

We will glady accept contributions from the LitebaseDB community. If you would like to contribute a new client to the official LitebaseDB organization on Github,
please reach out to us at [dev@litebasedb.com](mailto:dev@litebasedb.com).

## How is LitebaseDB architectured?

LitebaseDB is designed and optimized to operate on a per request basis, taking
advantage of the low-cost of runnning SQLite databases. Most relational database management systems require a server that is always running to operate. These systems usually are designed to work as singular system with allocated memory, storage, and compute running on a single or cluster of servers.

However, once these systems need to scale they require additional technical requirements. Which often leads to cluster mangement, sharding, and decoupling the database's compute and storage to allow independent scale. These concepts are sometimes anthetical to the original design specification of these types of databases.

On the other hand, SQLite what designed as an embedded database that interacts with an underlying filesystem. It is portable, and does not need a database server running prior to the physical database having the readiness needed to operate.

**Database requests flow through the LitebaseDB service in the following manner:**

* Clients send the requests to the LitebaseDB service using a regional url
* The request will hit a load balancer that forward the request to a router node layer.
* A router node will process the request and forward it to the compute layer, the Data Runtime.
* Once the data runtime receives a request it executes the query statement against a SQLite database to return a result.

### Router Nodes

...

### Data Runtime

...

### Filesystem

...

## Performance

### Read performance

...

**What to expect**
...

### Write performance

...

**What to expect**
...

### Auto-Scaling

...

## Use Cases

...

### When should you use LitebaseDB?

* If your application has infrequent, intermittent, or unpredictable workloads.
* If the pay-per-use pricing model of LitebaseDB provides you with good margin. Taking into account the true cost of database ownership when it comes to provisioning servers and maintaing the system.

### When should you not use LitebaseDB?

There are situations where LitebaseDB will not be a good fit for your project.
These include situations like:

* Workloads that require scaling to 10's of thousands of writes per second. In the context of how LitebaseDB interacts with SQLite, such a write heavy load would lead to concurrency issues. SQLite can only have one writer at a time, so there are throughput limitations.
* Your project requires hundreds of millions of database requests per month without an appropriate business model. A LitebaseDB database can handle trillions of requests per month, but it only makes sense to use this service if your business model supports the underlying cost.
