# Memory Grants

Two worst cases are:

* The Tiny Grant That Constantly Spills to TempDB
  1. Parameters are used that ask for a tiny memory grant
  2. Other parameters need a HUGE grant, but don't get it
  3. When they run, they spill to TempDB and take forever General symptom: unhappy users
* The Big Unused Grant:
  1. Parameters are used that ask for a large memory grant
  2. Other parameters don't need the grant
  3. SQL Server ends up leaving all the memory unused every time the query runs, causing RESOURCE\_SEMAPHORE waits. More info: https://www.brentozar.com/training/mastering-server-tuning-wait-stats-live-3-days-recording/2-5-memory-waits-resource\_semaphore-38m/ General symptom: unhappy sysadmins, low PLE.
