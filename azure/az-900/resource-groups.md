---
description: >-
  Source:
  https://www.youtube.com/watch?v=g6thrYZhPZY&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=10
---

# Resource Groups

* Everything has to live in one and only one RG
* It has to live in a single region, but that is just the metadata itself
  * You can have many different services or resources from different regions in the same RG
* Resource is not a boundary of usage
  * i.e. Networking can span multiple RGs
* A good reason for creating a new RG is if everything within that RG has the same lifecycle
* Role Base Access Control (RBAC) is another good reason to use an RG
* You can put policies on RGs, which are similar to guard rails
* You can place a budget or spend limit on an RG
* Sometimes you might have all your databases in the same RG so that the DBAs can easily manage that RG
