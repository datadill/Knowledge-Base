---
description: 'Source: Microsoft Learn'
---

# Deploy IaaS Soluton with Azure SQL

Reason for deploying SQL to a Azure VM

* versions of SQL available
* Use of other SQL Services such as SSIS
* Compatibility with On-prem



Licensing models

* Pay as you go (by the minute)
* BYOL
* Azure Hybrid Benefit (AHB) for windows licensing
  * very similar to SQL BYOL
* Reserving for VM for 1 or 3 years in advance



VM Types

* [General purpose](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-general) - These VMs provide a balanced ratio of CPU to memory. This VM class is ideal for testing and development, small to medium-sized database servers, and web servers with a low to medium amount of traffic.
* [Compute optimized](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-compute) - Compute optimized VMs have a high CPU-to-memory ratio and are good for web servers with a medium amount of traffic, network appliances, batch processes, and application servers. These VMs can also support machine learning workloads that can't benefit from GPU-based VMs.
* [Memory optimized](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-memory) - These VMs provide high memory-to-CPU ratio. These VMs cover a broad range of CPU and memory options (all the way up to 4 TB of RAM) and are well suited for most database workloads.
* [Storage optimized](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-storage) - Storage optimized VMs provide fast, local, NVMe storage that is ephemeral. They are good candidates for scale-out data workloads such as Cassandra. It is possible to use them with SQL Server, however since the storage is ephemeral, you need to ensure you configure data protection using a feature like Always On Availability Groups or Log Shipping.
* [GPU](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-gpu) - Azure VMs with GPUs are targeted at two main types of workloads—naturally graphics processing operations like video rendering and processing, but also massively parallel machine learning workloads that can take advantage of GPUs.
* [High performance compute](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes-hpc) - High Performance Compute workloads support applications that can scale horizontally to thousands of CPU cores. This support is provided by high-performance CPU and remote direct memory access (RDMA) networking that provides low latency communications between VMs.



SQL Server Config

* You can configure specific SQL Server settings such as, Security and Networking, SQL Authentication preferences, SQL instance settings, and a few other options.



Understand hybrid scenarios

* Disaster recovery
* SQL Server backups
* Azure ARC
  * In this hybrid scenario, Azure Arc enables the inventory of all registered SQL Server deployments and assesses their configurations, usage patterns, and security to provide actions and recommendations based on best practices.
  * By using [Azure Arc enabled SQL Servers](https://learn.microsoft.com/en-us/sql/sql-server/azure-arc/overview), you gain the benefits of centralized server management.
  * You also get Azure Defender real-time security alerts and vulnerability reporting on both on-premises SQL Servers and their host operating systems. In addition, Azure Sentinel can provide more security threat introspection if necessary.



Security Considerations

* When deploying a hybrid SQL solution, all core infrastructure, such as Active Directory and DNS, must exist on-premises and in Azure.
* S2S VPN or ExpressRoute needed



Performance and Security

* While Azure offers various types of storage (blob, file, queue, table) in most cases SQL Server workloads will use Azure managed disks. The exceptions are that a Failover Cluster Instance can be built on file storage and backups will use blob storage.



Azure-managed disks all offer two types of encryption

* Azure Server-side encryption is provided by the storage service and acts as encryption-at-rest provided by the storage service
* Azure Disk Encryption uses BitLocker on Windows, and DM-Crypt on Linux to provide OS and Data disk encryption inside of the VM



Types of Disk

* Operating System
* Temporary
* Data



* The best practices for SQL Server on Azure recommend using Premium Disks pooled for increased IOPs and storage capacity. Data files should be stored in their own pool with read-caching on the Azure disks.
* Transaction log files won't benefit from this caching, so those files should go into their own pool without caching. TempDB can optionally go into its own pool, or using the VM’s temporary disk, which offers low latency since it's physically attached to the physical server where the VMs are running. Properly configured Premium SSD will see latency in single digit milliseconds. For mission critical workloads that require latency lower than that, you should consider Ultra SSD.



SQL Security

* Microsoft Defender for SQL provides Azure Security Center security features such as vulnerability assessments and security alerts.
* Azure Defender for SQL can be used to identify and mitigate potential vulnerabilities in your SQL Server instance and database.&#x20;
* Azure Security Center is a unified security management system that evaluates and offers opportunities for improving several security aspects of your data environment. Azure Security Center provides a comprehensive view of the security health of all your hybrid cloud assets.



SQL Compression

* Row - Row compression basically stores each value in each column in a row in the minimum amount of space needed to store that value.
* Page - Page compression is a superset of row compression, as all pages will initially be row compressed prior to applying the page compression. Then a combination of techniques called prefix and dictionary compression are applied to the data. Prefix compression eliminates redundant data in a single column, storing pointers back to the page header. After that step, dictionary compression searches for repeated values on a page and replaces them with pointers, further reducing storage. The more redundancy in your data, the greater the space savings when you compress your data.
* Columnstore Archival - Columnstore objects are always compressed, however, they can be further compressed using archival compression, which uses the Microsoft _XPRESS_ compression algorithm on the data.&#x20;



