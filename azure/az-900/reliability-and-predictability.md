---
description: >-
  Source:
  https://www.youtube.com/watch?v=kD2YqdDaO1w&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=7
---

# Reliability and Predictability

Nodes and racks can fail, but in the cloud it an heal itself



Reliability

* For example, if your VM goes down, Azure can cover it and redeploy that VM elsewhere
* With storage accounts, there are at minimum 3 copies of your data
  * With databases, you might have additional replication with log replay on a separate server
* Auto Scale is the ability to add additional resources to your app and deallocate those resources after the fact
* SLA
  * Each service has a different SLA
* Design for failure is something that needs to be considered when developing on Azure
  * Your server might be active->active, active->passive, etc, but you need to design for that
* Azure monitor allows you to create alerts and action groups so that you can keep an eye on those resources



Predictability

* Many different SKUs are available in Azure
  * Certain memory, CPU, throughput, etc
* Behavior&#x20;
* Templates allow you to quickly deploy new assets using a standardized format
  * This also allows for **automation** to perform maintenance activities
  * Devops
