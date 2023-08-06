---
description: 'Source : PASS 2022 - Dive into SQL Server''s Buffer Pool and Kill Wasted Space!'
---

# SQL Server Buffer Pool

SELECT \* FROM Foo WHERE Bar = 1&#x20;

* this is a heap table
* goes through TDS (tabular data stream) no matter what engine you are using (e.g. ODBC) -> protocol layer -> Lexer / Parser (Lexer is concerned with words and parser looks at sentences), which creates a "parse tree" -> query optimizer -> query executor (could be waiting on CPU or resource\_semaphor) -> access methods -> buffer manager (we want to initiate some logical IO and if it is not found in buffer pool, it goes to disk and brings the page into the buffer pool using the BUF array to keep track of the pages we are pulling in) -> data page gets sent to client
  * When a SQL Server starts, it is quite aggressive with what it grabs into the buffer pool

UPDATE Foo SET fname = 'p' WHERE Bar = 1

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* For the update query above, go through all the normal steps and you eventually find the pages in the buffer pool
  * You then need to interact with the transaction manager and start writing to the log to ensure durability
    * The SQL Log Mgr will start writing to a log buffer (60kb in size)
    * At the same time, the transaction manager is making modifications to the page in the buffer pool and will mark it as dirty
    * Once the COMMIT is initiated, the log buffer will then go to the .ldf file
      * Azure PaaS, under the covers it writes to a page server of some sort versus a transaction log
      * Once the COMMIT is initiated and the log buffer has written to the durable storage, you are now complete even if the server goes down before replying to the client
* Log buffer flush
  * Log buffer is full (60KB)
  * Commit transactions initiated
    * Exception: delayed durability (can cause data loss, but if you have lots of tiny lil writes it can help).. It means that the commit statement does not trigger to log flush operation
*   Writing back to the database .mdf/.ndf (3 scenarios)

    * CHECKPOINT
      * To shorten database recovery
      * Pages remain buffer pool
    * Lazy writer (it is up all the time.. not so lazy!)
      * Uses an LRU-k (least recently used) algorithm to remove pages from buffer pool
      * If the pages are dirty they have to be written to disk otherwise just remove them
      * If we need more space in memory then the warm pages will be removed next
    * Eager writer
      * SELECT INTO, Bulk Insert, WriteText and UpdateText.

    <figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* In a clustered index, the table is physically ordered, but the rows themselves could be anywhere on a page
  * at the bottom of the page is a slot array, which is indeed in physical order



GAM - tracking space on one object on 64k extent

sGAM - tracking space on multiple objects on 64k extent

PFS - Tracks page space frequently in buckets every 8088 pages

* Empty
* 0-50% Full
* 51-80% Full
* 81-95% Full
* 96-100% Full



Index Allocation Map (IAM)

* tracks an allocation unit
  * IN\_ROW\_DATA
    * Heaps or indexes
  * LOB\_DATA
    * XML, VARCHAR
  * ROW\_OVERFLOW\_DATA
    * Rows/columns that exceed 8060 bytes



Some causes of wasted Bpool space?

* HEAPs (in operation)
* duplicats/redundant/unused indexes
* Page splits
* Is\_deleted BIT NOT NULL AKA soft deletes



Easy wins?

* Delete unused indexes
* page compression
  * For OLTP, not warehousing
* columnstore compression



