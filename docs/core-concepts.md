---
section: Getting Started
title: Core Concepts
---

# Core Concepts

{.text-lg.font-medium}
Relational SQL Database over HTTP

LitebaseDB is a data service with SQLite compatability. You can create or upload
an existing database and immediately send queries to the database via a secure
HTTP endpoint. There are no persistent connections to the database so you never
have to manage connections.

## System Architecture

LitebaseDB is designed and optimized to operate on a per request basis, taking
advantage of the low-cost of runnning SQLite databases. Most relational database
management systems require a server that is always running to operate. These
systems usually are designed to work as a singular system with allocated memory,
storage, and compute working together on a single or cluster of servers.

However, once these systems need to scale they usually require additional technical
requirements. Which often leads to cluster mangement, sharding, and decoupling
the database's compute and storage to allow independent scaling operations.
These concepts are sometimes anthetical to the original design specification
of these types of databases.

On the other hand, SQLite was designed as an embedded database that allows a
process running an application to directly read and write to database files on
disk. It is portable and does not require a separate server process to be running.

When requests arrive to the LitebaseDB service, each is routed toa dedicated
compute environment for each database. The process responsible for resolving the
request creates an SQLite connection, executes the query, and returns a response.
This level of isolation allows each LitebaseDB database to scale independently to
thousands of requests per second.

**Database requests flow through the LitebaseDB service in the following manner:**
{class="text-lg rounded border p-2 inline-block mt-0"}
Client -> Load Balancer -> Router -> Data Runtime -> Filesystem

1. Client sends requests to LitebaseDB using a secure endpoint.
2. The request hits a load balancer which is then forwarded the router layer.
3. A router node will process the request and forward it to the appropriate
   compute environment, known as the Data Runtime.
4. The Data Runtime receives a request, opens a database connection, and executes
   the query on the SQLite database stored in a secure filesystem.

### Regions

LitebaseDB is a multi-regional service. For best [performance](/docs/performance)
you should deploy your database in the closest region to your application. Today
you can deploy a database to one of our regions. In the future you will also be
able to deploy read replicas to multiple regions.

> {alert:warning} During our Beta period database can only be deployed to the
> us-east-1 region.

| Region ID      | Region Name              | Availability |
| -------------- | ------------------------ | ------------ |
| us-east-1      | US East (N. Virginia)    | Ready        |
| us-west-1      | US East (N. California)  | Coming soon  |
| eu-west-1      | Europe (Ireland)         | Coming soon  |
| af-west-1      | Africa (Cape Town)       | Coming soon  |
| ap-east-1      | Asia Pacific (Hong Kong) | Coming soon  |
| ap-northeast-1 | Asia Pacific (Tokyo)     | Coming soon  |
| ap-south-1     | Asia Pacific (Mumbai)    | Coming soon  |
| ca-central-1   | Canada (Central)         | Coming soon  |

### Network Load Balancer

At each regional endpoint there is a netwok load balancer that listens for TLS
connections and forwarding requests to the router layer.

### Router Nodes

Within the router layer there is a fleet of nodes prepared to receive incoming
traffic and direct them to the dedicated compute environment for each database.

### Data Runtime

This is the execution environment in which database requests are resolved. Here,
we leverage AWS Lambda functions to isolate database requests and resolve them
without having to worry about server management.

### Filesystem

Connected to each Data Runtime is single isolated filesystem. Within the volume
we securely store the SQLite database along with hot backup files.

## Querying the database

Once your database has been created, you can send SQL statements using one of
our database clients. Typically a database request is written as a single string:

{title=verified-users.sql}
```sql
SELECT * FROM users WHERE verified_at IS NOT NULL LIMIT 15;
```

However, LitebaseDB expects request payloads to be formatted in JSON:

{title=verified-users.json}
```json
{
  "statement": "SELECT * FROM users WHERE verified_at IS NOT NULL LIMIT 15"
}
```

### Statement Parameters

Parameters should be used to protect against SQL injection attacks. It's a
dangerous idea to accept unfiltered strings from user input. Using parameters
will treat the input as literal strings and never executed. Parameters can be
sent in requets using `?` placeholders.

```json
{
  "statement": "INSERT INTO users (username, password) VALUES (?, ?)",
  "parameters": [
    "orbit10",
    "5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8"
  ]
}
```

> {alert:info} Using statement bindings is the most secure way to send
> sensitive information to LitebasDB. Stament bindings is also more performant,
> as it allows staments to be prepared and cached.

## Supported clients

To send requests to your database, you'll typically want to use one of our
clients. These clients are responsible for securing your requests to the
LitebaseDB service.

**We currently maintain the following 1st party clients:**

| Language/Framework | Availability             |
| ------------------ | ------------------------ |
| PHP                | :white_check_mark:       |
| Laravel            | :white_check_mark:       |
| NodeJS             | :white_check_mark:       |
| Ruby               | :hourglass_flowing_sand: |
| Python             | :hourglass_flowing_sand: |

### Contribute new clients

We are actively working on creating more clients to support as many languages
and frameworks as possible.

We will glady accept contributions from the LitebaseDB community. If you would like to contribute a new client to the official LitebaseDB organization on [GitHub](https://github.com/litebasedb),
please reach out to us at [dev@litebasedb.com](mailto:dev@litebasedb.com).
