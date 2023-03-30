---
description: 'Source : PASS 2022 - Dive into SQL Server''s Buffer Pool and Kill Wasted Space!'
---

# SQL Server Buffer Pool

SELECT \* FROM Foo WHERE Bar = 1&#x20;

* this is a heap table
* goes through TDS (tabular data stream) no matter what engine you are using (e.g. ODBC) -> protocol layer -> Lexer / Parser (Lexer is concerned with words and parser looks at sentences), which creates a "parse tree" -> query optimizer -> query executor (could be waiting on CPU or resource\_semaphor) -> access methods -> buffer manager (we want to initiate some logical IO and if it is not found in buffer pool, it goes to disk and brings the page into the buffer pool using the BUF array to keep track of the pages we are pulling in) -> data page gets sent to client
  * When a SQL Server starts, it is quite aggressive with what it grabs into the buffer pool

![](../.gitbook/assets/image.png)



![](<../.gitbook/assets/image (1).png>)

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

