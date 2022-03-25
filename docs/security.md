---
description: How we maintain data security and best practices to follow to ensure your data remains private.
section: Getting Started
tags: ['security']
title: Security, how we secure your data
---

# Security

{.text-xl}
How we maintain data security and best practices to follow to ensure your data
remains private.

Security at LitebaseDB is the highest priority. Everything we design and build
stands on security-first principles so we can provide our customers with
a best in class experience security and privacy at the forefront.

## How your data is processed

...
...

### Encryption of data at rest

...

### Encryption of data in transit

...

## How your data is secured

Our platform is designed to run in the cloud on Amazon Web Services. As a data
provider, we continue to take exteme caution while ensuring our systems are
secure and your data is private. Here is a brief overview of how we secure your
data today:

1. ### Secure Access Keys

    Each database can create multiple access keys that allow a user or software
    to programatically read and write to a database. Access keys can also be managed
    limiting each access key's ability to read, write, and perform other capabilities.

    Access Keys are unique for every database and for issuance. In addition to the
    Access Key Id and Secret pair, we also generate a server secret that
    is used to create an additional signature for incoming requests that will pass
    between our router nodes and your data runtime.

2. ### Request Signing Process

    All requests that send data to the LitebaseDB Service must be authenticated
    with a request signature that follows our signing process specification. The
    signature is created using a database Access Key Id and Secret and is inserted
    as a HTTP header to verify the authenticity and integrity of the request
    without transmitting your secrets.

    We have a list of clients available so that requests may be made in many
    different languages.

3. ### Client Encryption

    In addition to signing requests, we also utilize AES 256 bit encryption for
    client requests, ensuring the statement and parameter properties are secured.
    Statements are encrypted using the Access Key ID as the salt so that we can
    decrypt statements as needed to provide query statistics.
    Parameters are encrypted using your Access Key Secret as the salt and we highly
    encourage using parameters also known as statement bindings when transmitting
    sensitive information. Doing so will ensure that only your application and
    your data envrionment can fully read your queries.

    Responses receive from the LitebaseDB service will also be encrypted using
    the Access Key Secret so the client can decrypt and read the data.  

4. ### TLS Encryption

    Clients can only send requests to the LitebaseDB Serive using HTTPS. This
    requirement ensures any data transmitted between clients and the LitebaseDB
    Service are fully encrypted during transit.

5. ### Network Isolation

    All incoming traffic to the LitebaseDB Service is routed through load balancers
    that have been isolated within a virtual private cloud. Requests are then
    transferred to our router nodes within the same virtual private cloud that are
    isolated from the internet.

    This layer in our architecture is responsible for
    request validation, access control list controls, rate limiting, signing
    requests headed to the compute layer, and a suite of other operations needed
    provide a robust and secure service. Data passing through this layer of the
    service is encrypted so that not even we can fully know what you are reading
    or writing from your database.

6. ### Dedicated Compute

    Within our virtual private cloud we also create isolated compute instances for
    each database. We never resolve database queries in the same compute
    environment as other databases. Once a request reaches isolated compute we
    verify multiple signatures to ensure each request orignated from our router
    layer and from a verified client.

    * Database compute is alway isolated in a private subnet.
    * Requests are encrypted in transit.
    * Access Key rules and permissions are enforced to control capabilities.

7. ### Secure Storage

    Each database receives its own isolated files system volume. We use fine grained
    policies to ensure that only a database's dedicated compute instatnces can mount
    and perform operations on the data in the volume.

    * Data encryption in transit.
    * Data encryption at rest.
    * Resource and system user policies that restrict acccess.

8. ### Audit Logs

    ...

## Compliance

We are committed to offering our customers with compliance tools and security measures that allow our customers to demonstrate compliance with applicable
legal and regulatory requirements. Please contact us for specific information
about compliance.

## Frequently Asked Questions

Can you view my data?
:   No, we do not have access to your data. We simply administer the
    infrastructure. When you send queries to the LitebaseDB service encryption
    prevents us from fully understanding what you are reading or writing.

Are my backups secure?
:   Yes, all hourly and additional incremental backups are stored within your
    secure filesystem. Backups that are older than 24 hours are securely stored
    in a AWS bucket specifically for your database with encryption enabled.

What tools do you provide to monitor security?
:   You can monitor the audit logs for your account to observer which users
    within your account have perfomed sensitive tasks. You can also view a
    summary of your issues access keys and monitor their usage using the query
    log interface.

{.bg-black .text-white .mt-16.p-8 .rounded}
If you have any questions or concerns about security, please contact us at [security@litebasedb.com](mailto:security@litebasedb.com)
