---
description: >-
  https://www.youtube.com/watch?v=LSVewE4mKfE&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=24
---

# Benefits and Usage of Big Data and Analytics Services

Back in the day, ETL was the most common approach due to limited disk space

* In the modern world, we have ADLS Gen 2 that sits on top of blob so it makes sense to load the raw data into ADLS Gen 2
  * If in the future you ever want to come back and add more data to the ETL process, you can easily access everything stored in ADLS Gen 2 in its' raw format
* In the final transform phase of ELT, you clean and wrangle the data before loading into the destination where it can be analyzed&#x20;
  * Azure Data Factory is the orchestrator that facilitates this data movement&#x20;

HDInsight

* HDInsight is all about open source frameworks that Microsoft has created managed solutions for
  * Hadoop is about dividing tasks into smaller parts&#x20;
    * Disk based
      * Map reduce breaks things down into key value pairs of data that can be shuffled around&#x20;
  * Storm is real-time processing for machine learning
  * Spark is mostly batch jobs and data transformation&#x20;
    * Memory based
  * Kafka is all about big data streaming
  * Hive LLAP is interactive querying like from a data lake
  * HBase is NoSQL storage

Databricks

* Built off apache spark
* Microsoft Databricks is a managed solution built in Azure
* Has a delta lake that sits on top of a data lake

Azure Synapse Analytics&#x20;

* Brings everything above under a single umbrella/workspace
