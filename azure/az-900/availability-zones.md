---
description: >-
  Source:
  https://www.youtube.com/watch?v=h0enGb17lnw&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=9
---

# Availability Zones

For a data center you need:

1\) Independent Power

2\) Independent Cooling

3\) Independent Networking



Availability Zones help with a facility level failure

* Not every region support availability zones, but most do
* Usually there are 3 availability zones to pick from
  * 1,2, and 3 does not necessarily indicate building #1, building #2, etc, but it does guarantee different buildings
* A subscription is a billing boundary as well as a resource boundary
  * For each subscription you will see AZ1, AZ2, AZ3
    * These zones may be unique to this subscription
      * i.e. The data center might have 20 physical buildings and your AZs may map to separate buildings than other subscriptions
* Similar to regions, microsoft will roll out updates to one AZ at a time
* Zonal is when something goes to a specific AZ (i.e. a VM)
* Zone redundant when it spans across the AZs
  * Sometimes you can choose this and other times it is forced







