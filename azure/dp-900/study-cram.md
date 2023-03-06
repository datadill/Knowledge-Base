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

Unstructured Data

