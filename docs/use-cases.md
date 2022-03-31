---
section: Getting Started
title: Use Cases
---

# Use Cases

...

## Who is LitebaseDB for?

...

### When should you use LitebaseDB?

- If your application has infrequent, intermittent, or unpredictable workloads.
- If the pay-per-use pricing model of LitebaseDB provides you with good margin. Taking into account the true cost of database ownership when it comes to provisioning servers and maintaing the system.

### When should'nt you use LitebaseDB?

There are situations where LitebaseDB will not be a good fit for your project.
These include situations like:

- Workloads that require scaling to 10's of thousands of writes per second. In the context of how LitebaseDB interacts with SQLite, such a write heavy load would lead to concurrency issues. SQLite can only have one writer at a time, so there are throughput limitations.
- Your project requires hundreds of millions of database requests per month without an appropriate business model. A LitebaseDB database can handle trillions of requests per month, but it only makes sense to use this service if your business model supports the underlying cost.

## Common Use Cases

### Primary Datastore

...

### Supplemental Datastore

...

### Business Analytics

...
