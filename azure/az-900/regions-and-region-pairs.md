---
description: >-
  Source:
  https://www.youtube.com/watch?v=kD2YqdDaO1w&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=7
---

# Regions and Region Pairs

Azure as a Service is comprised of lots of data centers around the world



Regions

* These data centers are then grouped by particular region (East US, West US, etc)
  * The region is described by a 2ms **max** envelope roundtrip latency&#x20;
  * That means all servers in a region sit within a defined geographical area that allows for this max of 2ms latency
* Sovereign Cloud
  * US Government uses their own special cloud along with other governments such as China
* If you have users in the east coast, you might want them to hit East US, but users on the west coast might hit West US
* Sometimes you have to follow a country's laws, which means your data has to be in a specific region



Region Pairs

* Microsoft always considers natural disasters when choosing locations for their data centers
* Region pairs give you resiliency in the event there is some sort of failure or disaster
* Some services like Azure SQL DB allow you to choose how your data is replicated, but other services have defined pairings of regions that you can't change
  * Geo-redundant data will automatically pair to its' pair region
* Pairings for each data center can be found online
* Won't update two regions from the same pairing at the same time
* They help you as the customer as well as Microsoft in the event of an issue
