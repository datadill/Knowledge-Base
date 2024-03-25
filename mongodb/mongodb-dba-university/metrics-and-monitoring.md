# Metrics & Monitoring

Core metrics:

* Query targeting
  * Ideal ratio is 1 where a document is returned for every one read
  * very high ratio negatively impacts performance
* Storage
  * writes are refused at capacity and can cause crashing
  * key metrics include disk space percent free, disk latency, disk IOPs, disk queue depth
* CPU
  * May need to optimize with indexes or upgrade hardware
* Memory
  * System should be sized to hold all indexes
  * Swap usage, and memory usage
*   Replication lag

    * delay between primary and secondary in seconds



Additional Metrics:

* opcounters
  * number of operations per seconds run on mongodb process since startup
    * It tracks: command, query, insert, delete, update, getMore
* network traffic
  * Average rate of physical bytes
    * bytes in / bytes out (physical)
  * Number of requests sent to DB
    * numRequests
* connections
  * Organized by application, shell client, as well as internal processes
  * Can affect system performance
  * Large connection count may be suboptimal connection strategy
* tickets available
  * when available tickets drops to zero, other operations must wait
  * indiciates undersized cluster or poorly performing queries
