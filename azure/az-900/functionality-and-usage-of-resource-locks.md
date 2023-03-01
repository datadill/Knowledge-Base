---
description: >-
  https://www.youtube.com/watch?v=eF_KilJRxbE&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=47
---

# Functionality and Usage of Resource Locks

Resource locks can be applied on various levels such as subscription, resource group, resources, etc

* Can be applied at the top and it will trickle down
* You can assign a resource lock as "can not delete" or "read only"
* ARM is the control plane and resource locks are where those are applied
  * The data plane that varies by the individual resource is not affected
    * For example, if it was a blob or database, you could still interact with the data

