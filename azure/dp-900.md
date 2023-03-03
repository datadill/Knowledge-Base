# DP-900

File Formats

* _Avro_ is a row-based format. Avro is a good format for compressing data and minimizing storage and network bandwidth requirements.
* _ORC_ (Optimized Row Columnar format) organizes data into columns rather than rows. It was developed by HortonWorks for optimizing read and write operations in Apache Hive (Hive is a data warehouse system that supports fast data summarization and querying over large datasets).
* _Parquet_ is another columnar data format. A Parquet file contains row groups. Data for each column is stored together in the same row group. Each row group contains one or more chunks of data. A Parquet file includes metadata that describes the set of rows found in each chunk. Parquet specializes in storing and processing nested data types efficiently.



Four common types of Non-relational database

* Key:Value
* Document databases
  * which are a specific form of key-value database in which the value is a JSON document (which the system is optimized to parse and query)
* Column family databases
  * which store tabular data comprising rows and columns, but you can divide the columns into groups known as column-families. Each column family holds a set of columns that are logically related together.
* Graph databases
  * which store entities as nodes with links to define relationships between them.



ACID

* Atomicity
  * each transaction is treated as a single unit, which succeeds completely or fails completely.
* Consistency
  * transactions can only take the data in the database from one valid state to another.
* Isolation
  * concurrent transactions cannot interfere with one another, and must result in a consistent database state.
* Durability&#x20;
  * when a transaction has been committed, it will remain committed.



* **Azure SQL Database** – a fully managed platform-as-a-service (PaaS) database hosted in Azure
* **Azure SQL Managed Instance** – a hosted instance of SQL Server with automated maintenance, which allows more flexible configuration than Azure SQL DB but with more administrative responsibility for the owner.
* **Azure SQL VM** – a virtual machine with an installation of SQL Server, allowing maximum configurability with full management responsibility.
* **Azure Cosmos DB** is a global-scale non-relational (_NoSQL_) database system that supports multiple application programming interfaces (APIs), enabling you to store and manage data as JSON documents, key-value pairs, column-families, and graphs.
* **Azure SQL Edge** - A SQL engine that is optimized for Internet-of-things (IoT) scenarios that need to work with streaming time-series data.



Azure Storage

* **Blob containers** - scalable, cost-effective storage for binary files.
* **File shares** - network file shares such as you typically find in corporate networks.
* **Tables** - key-value storage for applications that need to read and write data values quickly.
* Data engineers use Azure Storage to host _data lakes_ - blob storage with a hierarchical namespace that enables files to be organized in folders in a distributed file system.



**Azure Synapse Analytics** is a comprehensive, unified data analytics solution that provides a single service interface for multiple analytical capabilities, including:

* **Pipelines** - based on the same technology as Azure Data Factory.
* **SQL** - a highly scalable SQL database engine, optimized for data warehouse workloads.
* **Apache Spark** - an open-source distributed data processing system that supports multiple programming languages and APIs, including Java, Scala, Python, and SQL.
* **Azure Synapse Data Explorer** - a high-performance data analytics solution that is optimized for real-time querying of log and telemetry data using Kusto Query Language (KQL).

A**zure Databricks** is an Azure-integrated version of the popular Databricks platform, which combines the Apache Spark data processing platform with SQL database semantics and an integrated management interface to enable large-scale data analytics.



**Azure HDInsight** is an Azure service that provides Azure-hosted clusters for popular Apache open-source big data processing technologies, including:

* **Apache Spark** - a distributed data processing system that supports multiple programming languages and APIs, including Java, Scala, Python, and SQL.
* **Apache Hadoop** - a distributed system that uses _MapReduce_ jobs to process large volumes of data efficiently across multiple cluster nodes. MapReduce jobs can be written in Java or abstracted by interfaces such as Apache Hive - a SQL-based API that runs on Hadoop.
* **Apache HBase** - an open-source system for large-scale NoSQL data storage and querying.
* **Apache Kafka** - a message broker for data stream processing.



**Azure Stream Analytics** is a real-time stream processing engine that captures a stream of data from an input, applies a query to extract and manipulate data from the input stream, and writes the results to an output for analysis or further processing.



**Microsoft Purview** provides a solution for enterprise-wide data governance and discoverability. You can use Microsoft Purview to create a map of your data and track data lineage across multiple data sources and systems, enabling you to find trustworthy data for analysis and reporting.





**Azure Cosmos DB**

Cosmos DB uses indexes and partitioning to provide fast read and write performance and can scale to massive volumes of data. You can enable multi-region writes, adding the Azure regions of your choice to your Cosmos DB account so that globally distributed users can each work with data in their local replica.

Cosmos DB is highly suitable for the following scenarios:

* _IoT and telematics_
* _Retail and marketing_
* _Gaming_
* _Web and mobile applications_

**MongoDB** is a popular open source database in which data is stored in Binary JSON (BSON) format. Azure Cosmos DB for MongoDB enables developers to use MongoDB client libraries and code to work with data in Azure Cosmos DB.

MongoDB Query Language (MQL) uses a compact, object-oriented syntax in which developers use _objects_ to call _methods_. For example, the following query uses the **find** method to query the **products** collection in the **db** object:

Javascript.. db.products.find({id: 123}) results in: { "id": 123, "name": "Hammer", "price": 2.99 }

Azure Cosmos DB for **PostgreSQL** is a native PostgreSQL, globally distributed relational database that automatically shards data to help you build highly scalable apps. You can start building apps on a single node server group, the same way you would with PostgreSQL anywhere else. As your app's scalability and performance requirements grow, you can seamlessly scale to multiple nodes by transparently distributing your tables.

Azure Cosmos DB for Table is used to work with data in key-value tables, similar to Azure Table Storage. It offers greater scalability and performance than Azure Table Storage. You can query it like this:

https://endpoint/Customers(PartitionKey='1',RowKey='124')

Azure Cosmos DB for **Apache Cassandra** is compatible with Apache Cassandra, which is a popular open source database that uses a column-family storage structure. Column families are tables, similar to those in a relational database, with the exception that it's not mandatory for every row to have the same columns. Cassandra supports a syntax based on SQL.

Azure Cosmos DB for **Apache Gremlin** is used with data in a graph structure; in which entities are defined as vertices that form nodes in connected graph. Nodes are connected by edges that represent relationships. You can query it like this:

g.V().hasLabel('employee').order().by('id')

or add a new node/edge like this:

g.addV('employee').property('id', '3').property('firstName', 'Alice') g.V('3').addE('reports to').to(g.V('1'))





**Azure Blob Storage**

In an Azure storage account, you store blobs in _containers_. A container provides a convenient way of grouping related blobs together. You control who can read and write blobs inside a container at the container level.

Within a container, you can organize blobs in a hierarchy of virtual folders, similar to files in a file system on disk. However, by default, these folders are simply a way of using a "/" character in a blob name to organize the blobs into namespaces. The folders are purely virtual, and you can't perform folder-level operations to control access or perform bulk operations.



Azure Blob Storage supports three different types of blob:

* **Block blobs**. A block blob is handled as a set of blocks. Each block can vary in size, up to 100 MB. A block blob can contain up to 50,000 blocks, giving a maximum size of over 4.7 TB. The block is the smallest amount of data that can be read or written as an individual unit. Block blobs are best used to store discrete, large, binary objects that change infrequently.
* **Page blobs**. A page blob is organized as a collection of fixed size 512-byte pages. A page blob is optimized to support random read and write operations; you can fetch and store data for a single page if necessary. A page blob can hold up to 8 TB of data. Azure uses page blobs to implement virtual disk storage for virtual machines.
* **Append blobs**. An append blob is a block blob optimized to support append operations. You can only add blocks to the end of an append blob; updating or deleting existing blocks isn't supported. Each block can vary in size, up to 4 MB. The maximum size of an append blob is just over 195 GB.



Hot/Cold/Archive Tiers

* If you read data in the Archive tier, it must be rehydrated into the Hot/Cold tier, which can take several hours
* You can create lifecycle management policies for blobs in a storage account. A lifecycle management policy can automatically move a blob from Hot to Cool, and then to the Archive tier, as it ages and is used less frequently (policy is based on the number of days since modification). A lifecycle management policy can also arrange to delete outdated blobs.



Azure Data Lake Store (Gen1) is a separate service for hierarchical data storage for analytical data lakes, often used by so-called _big data_ analytical solutions that work with structured, semi-structured, and unstructured data stored in files. Azure Data Lake Storage Gen**2** is a newer version of this service that is integrated into Azure Storage; enabling you to take advantage of the scalability of blob storage and the cost-control of storage tiers, combined with the hierarchical file system capabilities and compatibility with major analytics systems of Azure Data Lake Store.

To create an Azure Data Lake Store Gen2 files system, you must enable the **Hierarchical Namespace** option of an Azure Storage account. You can do this when initially creating the storage account, or you can upgrade an existing Azure Storage account to support Data Lake Gen2. Be aware however that upgrading is a one-way process – after upgrading a storage account to support a hierarchical namespace for blob storage, you can’t revert it to a flat namespace.



Azure Files is essentially a way to create cloud-based network shares, such as you typically find in on-premises organizations to make documents and other files available to multiple users. By hosting file shares in Azure, organizations can eliminate hardware costs and maintenance overhead, and benefit from high availability and scalable cloud storage for files.

After you've created a storage account, you can upload files to Azure File Storage using the Azure portal, or tools such as the _AzCopy_ utility. You can also use the Azure File Sync service to synchronize locally cached copies of shared files with the data in Azure File Storage.



Azure Files supports two common network file sharing protocols:

* _Server Message Block_ (SMB) file sharing is commonly used across multiple operating systems (Windows, Linux, macOS).
* _Network File System_ (NFS) shares are used by some Linux and macOS versions. To create an NFS share, you must use a premium tier storage account and create and configure a virtual network through which access to the share can be controlled.



Azure Table Storage is a NoSQL storage solution that makes use of tables containing _key/value_ data items. Each item is represented by a row that contains columns for the data fields that need to be stored. **However, don't be misled into thinking that an Azure Table Storage table is like a table in a relational database.** All rows in a table must have a unique key (composed of a partition key and a row key), and when you modify data in a table, a _timestamp_ column records the date and time the modification was made; but other than that, the columns in each row can vary. For example, a table holding customer information might store the first name, last name, one or more telephone numbers, and one or more addresses for each customer unlike a traditional normalized RDBMS. The number of fields in each row can be different, depending on the number of telephone numbers and addresses for each customer, and the details recorded for each address.

To help ensure fast access, Azure Table Storage splits a table into partitions. Partitioning is a mechanism for grouping related rows, based on a common property or partition key. Rows that share the same partition key will be stored together. Partitioning not only helps to organize data, it can also improve scalability and performance in the following ways:

* Partitions are independent from each other, and can grow or shrink as rows are added to, or removed from, a partition. A table can contain any number of partitions.
* When you search for data, you can include the partition key in the search criteria. This helps to narrow down the volume of data to be examined, and improves performance by reducing the amount of I/O (input and output operations, or _reads_ and _writes_) needed to locate the data.



Real-Time Processing

Data processing is simply the conversion of raw data to meaningful information through a process. There are two general ways to process data:

* _Batch processing_, in which multiple data records are collected and stored before being processed together in a single operation.
* _Stream processing_, in which a source of data is constantly monitored and processed in real time as new data events occur.



Microsoft Azure supports multiple technologies that you can use to implement real-time analytics of streaming data, including:

* **Azure Stream Analytics**: A platform-as-a-service (PaaS) solution that you can use to define _streaming jobs_ that ingest data from a streaming source, apply a perpetual query, and write the results to an output.
* **Spark Structured Streaming**: An open-source library that enables you to develop complex streaming solutions on Apache Spark based services, including **Azure Synapse Analytics**, **Azure Databricks**, and **Azure HDInsight**.
* **Azure Data Explorer**: A high-performance database and analytics service that is optimized for ingesting and querying batch or streaming data with a time-series element, and which can be used as a standalone Azure service or as an **Azure Synapse Data Explorer** runtime in an Azure Synapse Analytics workspace.

#### _Sources_ for stream processing <a href="#sources-for-stream-processing" id="sources-for-stream-processing"></a>

The following services are commonly used to ingest data for stream processing on Azure:

* **Azure Event Hubs**: A data ingestion service that you can use to manage queues of event data, ensuring that each event is processed in order, exactly once.
* **Azure IoT Hub**: A data ingestion service that is similar to Azure Event Hubs, but which is optimized for managing event data from _Internet-of-things_ (IoT) devices.
* **Azure Data Lake Store Gen 2**: A highly scalable storage service that is often used in _batch processing_ scenarios, but which can also be used as a source of streaming data.
* **Apache Kafka**: An open-source data ingestion solution that is commonly used together with Apache Spark. You can use Azure HDInsight to create a Kafka cluster.

#### _Sinks_ for stream processing <a href="#sinks-for-stream-processing" id="sinks-for-stream-processing"></a>

The output from stream processing is often sent to the following services:

* **Azure Event Hubs**: Used to queue the processed data for further downstream processing.
* **Azure Data Lake Store Gen 2** or **Azure blob storage**: Used to persist the processed results as a file.
* **Azure SQL Database** or **Azure Synapse Analytics**, or **Azure Databricks**: Used to persist the processed results in a database table for querying and analysis.
* **Microsoft Power BI**: Used to generate real time data visualizations in reports and dashboards.



#### Azure Stream Analytics <a href="#sinks-for-stream-processing" id="sinks-for-stream-processing"></a>

Azure Stream Analytics is a service for complex event processing and analysis of streaming data. Once started, a Stream Analytics query will run perpetually, processing new data as it arrives in the input and storing results in the output.

Azure Stream Analytics is a great technology choice when you need to continually capture data from a streaming source, filter or aggregate it, and send the results to a data store or downstream process for analysis and reporting. If your stream process requirements are complex or resource-intensive, you can create a Stream Analysis _cluster._

__

**Apache Spark**

Apache Spark is a distributed processing framework for large scale data analytics. You can use Spark on Microsoft Azure in the following services:

* Azure Synapse Analytics
* Azure Databricks
* Azure HDInsight

Spark can be used to run code (usually written in Python, Scala, or Java) in parallel across multiple cluster nodes, enabling it to process very large volumes of data efficiently. Spark can be used for both batch processing and stream processing.

To process streaming data on Spark, you can use the _Spark Structured Streaming_ library, which provides an application programming interface (API) for ingesting, processing, and outputting results from perpetual streams of data.

Spark Structured Streaming is built on a ubiquitous structure in Spark called a _dataframe_, which encapsulates a table of data. You use the Spark Structured Streaming API to read data from a real-time data source, such as a Kafka hub, a file store, or a network port, into a "boundless" dataframe that is continually populated with new data from the stream. You then define a query on the dataframe that selects, projects, or aggregates the data - often in temporal windows. The results of the query generate another dataframe, which can be persisted for analysis or further processing.



**Delta Lake**

Delta Lake is an open-source storage layer that adds support for transactional consistency, schema enforcement, and other common data warehousing features to data lake storage. It also unifies storage for streaming and batch data, and can be used in Spark to define relational tables for both batch and stream processing. When used for stream processing, a Delta Lake table can be used as a streaming source for queries against real-time data, or as a sink to which a stream of data is written.

The Spark runtimes in Azure Synapse Analytics and Azure Databricks include support for Delta Lake.

Delta Lake combined with Spark Structured Streaming is a good solution when you need to abstract batch and stream processed data in a data lake behind a relational schema for SQL-based querying and analysis.
