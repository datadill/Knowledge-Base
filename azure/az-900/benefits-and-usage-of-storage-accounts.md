---
description: >-
  https://www.youtube.com/watch?v=b8BrfsxLSx8&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=19
---

# Benefits and Usage of Storage Accounts

Storage accounts live in a specific region

* You should always consider the region in where a storage account is placed geographically
  * Put it in a region where the compute services are going to be that will leverage the storage account
* Standard is normally a general purpose v2
  * It has LRS, ZRS, GRS, GZRS
    * You can also utilize read secondaries for certain file types like Blob
* Premium (low latency) could be Block Blob, Page Blob, or Files and it requires you to choose one of them
  * It has LRS and ZRS

Redundancy

* LRS - Locally redundant storage has 3 copies of the data in the same building
* ZRS - Zone redundant storage has 3 copies over the 3 different AZs
* GRS - Geo redundant storage has 3 copies in one building of region 1 and 3 copies in one building of region 2
* GZRS - Geo Zone redundant storage has 3 copies in region 1 in 3 separate buildings and 3 copies in region 2 in 3 separate buildings

Services

* Blob - unstructured piece of data
  * Block - made up of blocks and lives in a container
    * access tiers: hot, cool, archive
  * ADLS Gen 2&#x20;
  * Page - made up of pages and are great for random read/writes
  * Append - constantly adding to the end of something such as a log file
  * Files - SMB/NFS
    * Azure File Sync establishes a relationship to Azure to sync files up to Azure with on-premises windows based file servers
  * Queues - Small pieces of data that are first in first out (FIFO)
  * Tables - Schemaless key-value pairs

