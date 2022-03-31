---
description: LitebaseDB is a high performance production ready database.
section: Getting Started
title: Performance
---

# Performance

LitebaseDB was designed to take on demanding workloads while also remaing
flexible and cost efficient. However, due to the very nature of how the system
is architectured, there are things to keep in mind.

* Databases are equipped to handle high read throughput
* Workloads that require high write capacity should test and monitor their
  throughput to ensure the service meet application requirements.

> {alert:warning} SQLite supports an unlimited number of simulataneous readers
> but will only a single writer at any instant in time.

When a database's execution environment is warm requests typically execute in
less than a millisecond with single-digit millescond response times for requests
in the same region.

## Read performance

...

### What to expect

...

## Write performance

...

### What to expect

...

## Auto-Scaling

### Cold Starts

...

### Limits

...

## Benchmarks

\* These tests are based on requests between a client application and the
LitebaseDB service within the same region.

| Operation       | Latency |
| --------------- | ------- |
| Read 1000 rows  | 10ms    |
| Write 1000 rows | 100ms   |


