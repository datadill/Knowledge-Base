---
description: 'Source: https://www.youtube.com/watch?v=0gtpasITVnk'
---

# Study Cram

Control plane permissions do not mean you have permissions on the data plane

* Example: on a storage account, there are many roles available that are focused on the ARM control plane, but most of those roles will **not** provide permissions on the data itself
  * However, storage accounts do have some roles specific to the data plane



Azure SQL DB Network connection

* Azure SQL DB has a public IP that you are able to connect to via a DNS name
* You can also create a vNet with a private endpoint so that anything on the vNet or connected to the endpoint can then connect to the database
  * If you are not on the vNet, you can connect via S2S VPN or peering via expressRoute
    * Then of course you need the proper database permissions&#x20;

Semi-Structured Data

* No fixed schema
  * Think of JSON where Person1 might have different data available then Person2
* As data gets bigger, you want to be able to shard your data
  * Partition keys with an even distribution can help you shard the data effectively over logical partitions that will then separate over separate physical partitions&#x20;

Storage Accounts (BLOB)

* Block - made up of a sequence of blocks up to 50,000
* Page - good for random read/write access (512 byte pages)
* Append - can't modify, but can append&#x20;
  * ex. Logging

Azure Files

* SMB & NFS
* Sync on-prem and cloud

Cosmos DB

* Multi region and multi write
* Variable consistency
  * Strong <-> Session <-> Eventual
    * Strong - no matter where you are everyone sees the same data (multi write is not possible)
* Multi API
  * SQL, MongoDB, Document, Cassandra, etc&#x20;
* Request Units pricing model
  * Provisioned / autoscale/ serverless
    * autoscale is usually better and more cost effective

Microsoft Purview

* What data is out there?
  * Classify the data
    * You can use expressions or you can utilize trainable classifiers which utilizes machine learning
* Apply governance



