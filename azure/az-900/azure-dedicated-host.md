---
description: >-
  https://www.youtube.com/watch?v=RnNqmTH9xok&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=36
---

# Azure Dedicated Host

When you create a VM, that VM sits on a host

* Azure by definition is multi-tenant which means that you might have VMs from other tenants on the same host
* Sometimes you might need your own dedicated host for performance or licensing purposes
* You can create a Host Group, which you then add multiple hosts (physical racks) to&#x20;
  * They can span multiple AZs
    * These racks can be referred to as Fault Domains
      * You can then add VMs or VMSS and target those fault domains, which no other tenant is allowed to access
      * When you create a dedicated host, you can only put the same sku of VMs on that host, however they can still be sized differently
* Isolated VMs are so big that it takes over the entire host
  * They don't use host groups, but because they are so big there is no chance somebody else is on that host
  * Since you are the only one on the host, you can control the maintenance window
