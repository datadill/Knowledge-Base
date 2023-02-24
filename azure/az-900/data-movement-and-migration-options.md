---
description: >-
  https://www.youtube.com/watch?v=jNBcXnMTo9s&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=21
---

# Data Movement and Migration Options

Register up to 100 servers into a sync group where fileshares will sync to Azure

* You can keep that connection for resiliency purposes
* Additionally, you can do cloud tiering where if you start running out of free space on-prem, you can start offloading infrequently accessed files to azure
  * If you start to access that data, you can bring it back down



Azure File Explorer and Storage Browser provide an interactive GUI&#x20;

* Great for adhoc file interactions

Az Copy

* copy/sync on your local device to the cloud, but it can also do direct cloud to cloud
* You can also pull in data from AWS or GCP to Azure

Azure Migrate

* Can move VMs and databases
* Does some initial analysis and recommend the correct sku.. then replicate and cutover

Azure Data Box

* Importing into Azure (not exporting)
* They ship the encrypted disk and you ship it back
* Box that is 50 lbs and has 80TB of data that you plug into your network and copy files via SMB/NFS and ship it back
* Box heavy is 500 lbs and has 770 TB
