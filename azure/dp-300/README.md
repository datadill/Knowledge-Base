---
description: 'Source: Microsoft ESI Training'
---

# DP-300

Azure SQL VM

* Great for compatibility&#x20;
* Great for individuals installing SSIS/SSRS/SSAS or other 3rd party software directly on the SQL Server
* Backup functionality
  * Back up to URL
    * Standard backup syntax that goes directly to Azure Blob Storage
  * Azure Backup
    * Enterprise backup solution that automatically handles your backups



VM Storage

* Standard
* Standard SSD
* Premium SSD
* Ultra Disk
* For prod tLogs, you should only ever use Premium SSD or Ultra disk
  * premium == 5-10ms latency
  * ultra == 1-2ms latency
* Stanard usually suffices for backups



Microsoft guarantees uptime of at least 99.9% for single instance Azure virtual machine, when using Premium SSD or Ultra Disk for all disks.



**Azure SQL Database**

* vCore vs DTU
  * vCore decouples compute and storage
    * General Purpose, Business Critical, Hyperscale
      * Business critical includes a read-only replica and more memory per core/ssd storage
      * Hyperscale lets you scale storage and compute resources well over the scale of the other 2 tiers
  * DTU
    * Basic, Standard, Premium
      * Couples together storage, CPU, etc
    * Scaling may incur a brief connection interruption at the end of the operation
      * This can be somewhat mitigated by apps performing proper retry logic
      * 2 scenarios cause a blip
        * Initiation of a scaling operation
        * adding a DB to an elastic pool
    * Azure SQL Managed Instance doesn't support DTU-based purchasing model

Serverless Compute Tier

* despite name, does require a server with database
  * it is best thought of as an autoscale/pause solution for Azure SQL DB
    * auto-pause allows you to define period of inactivity before pausing DB
    * auto-start will cause the DB to spin back up when it is used
      * during this time, only storage costs are incurred&#x20;

Deployment Models

* Single vs Elastic
  * Elastic shares resources
  * Single has its' own set of resources even if the same underlying logical server is used
  * Elastic is great for multi-tenant DBs where each tenant avg CPU is low

Networking

* Azure SQL DB by default has a public internet endpoint, which can be controlled via firewall rules or limited to specific azure netowrks, using freatures like Virtual Netowrk endpoints or Private Link

Backups

* With SQL Database, you can increase administration efficiency by knowing that databases are backed up regularly, and that they are continuously copied to a read-access geo-redundant storage (RA-GRS).
* Full backups are performed every week, differential backups every 12 to 24 hours, and transaction log backups every 5 to 10 minutes.
* You can easily restore databases to other geographic locations for DR
* When provisioning a SQL DB, the backup storage is created separately with the maximum size of the data tier selected at no additional cost
* SQL Database retention can be set from 1 to 35 days, but the default is 7
* In order to leverage PITR, you must be restoring a DB to the same server the backup was originated
* Long Time Retention (LTR)
  * Useful for when you need to set the retention beyond what Azure offers, up to 10 years

Automatic Tuning

* Automatic tuning currently includes the following features:
  * Identify Expensive Queries
  * Forcing Last Good Execution Plan
  * Adding Indexes
    * First applied to a copy of your DB and then later applied
  * Removing Indexes

Elastic Query (preview)

* i.e. Linked Server for azure
  * Vertical Partitioning - Cross DB
  * Horizontal Partitioning - AKA sharding, distribution of rows across several scaled out DBs where the schema is the same

Elastic Job (preview)

* SQL Server Agent for Azure SQL DB

SQL Data Sync

* The Data Sync feature allows you to incrementally synchronize data across multiple databases running on SQL Database or on-premises SQL Server. Similarly, Data Sync is a good option if you need to offload intensive workloads in production with a separate database that can be used for analytics and/or ad hoc operations.
* Data Sync is based on a hub topology, where you define one of the databases in the sync group to work as a hub database. The sync group can have multiple members, and you can only synchronize changes between the hub database and individual databases. Data Sync tracks changes using insert, update, and delete triggers through a historical table created on the user database.
* When you create a sync group, it will ask you to provide a database responsible to store the sync group metadata. The metadata location can be either a new database or an existing database as long it resides in the same region as your sync group.
* Azure MI not supported



**Azure SQL MI**

The fully managed platform as a service (PaaS) provides some of the following benefits:

* Automatic backups
* Automatic patching
* Built-in high availability
* Security and performance tools
* Embedded auditing capabilities
* provides several other features including cross-database queries, common language runtime (CLR), access to the system databases, and use of the SQL Agent features.



Hybrid Licensing

* 1 Core Enterprise == 1 vCore of SQL DB or SQL MI Business Critical
* 1 Core Standard == 8 vCores of general purpose

Connections to SQL Managed Instance are made through TDS endpoints. While the routing and security on these connections differ, there's a gateway component that handles and routes connections to the database service. This gateway component is also deployed in a highly available fashion.



Backup and Restore

There are some important considerations when running backup and restore operations on SQL Managed Instance databases:

* It isn't possible to overwrite an existing database during the restore process. Before restoring a database, you must ensure that it doesn't exist.
* For SQL Managed Instance, backups can only be restored to another managed instance. It isn't possible to restore a managed instance database backup to a SQL Server running on a virtual machine or SQL Database.
* Copy-only backup to Azure blob storage is available for SQL Managed Instance. SQL Database doesn't support this feature.



High  Availability

SQL Database and SQL Managed Instance have similar high availability architectures, which guarantee 99.99% uptime. Windows and SQL Server updates are handled by the backend infrastructure, generally without any effect to your application, though it's important to place a [retry logic](https://learn.microsoft.com/en-us/azure/azure-sql/database/troubleshoot-common-connectivity-issues) into your application.

* Auto failover groups
  * Can contain 1:M DBs
* Read only secondaries



Migration Options

There are a couple of ways to migrate on-premises databases:

* Restoring a backup
* Using Database Migration Service (DMS)

Backup and restore will incur more downtime, as it isn't possible to restore with **NORECOVERY** option, and apply log backups.

The Database Migration Service is a managed service that connects your on-premises (or Azure Virtual Machines) SQL Server to SQL Managed Instance with near zero downtime. As a result, it acts like an automated log shipping process, meaning you can keep your target databases in sync right up to the point of cutover.



Machine Learning Services

Machine Learning Services provides machine learning operations within your relational database structure. This feature supports Python and R packages, ideal for high-intensive predictive capabilities. This option is available on SQL Managed Instance, SQL Server on Azure virtual machine, and on-premises SQL Server.

