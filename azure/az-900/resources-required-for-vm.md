---
description: >-
  https://www.youtube.com/watch?v=PP5BWZ0cAJo&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=15
---

# Resources Required for VM

When we create a VM, it lives within a particular subscription, which has at least 1 resource group

* The VM, at minimum has an OS managed disk, which is in itself an Azure resource
  * There are also ephemeral disks where it uses cache or temporary disk for that OS, but if you deallocate or stop that VM then you lose that OS state
  * We may also have additional Data disks
    * You pay for the VM along with those disk
* The VM requires connectivity via a vNet, which has subnets
  * We attach a virtual NIC to the VM, which connects to a subnet in a vNet.
  * You don't pay for the NIC or subnet, but you do pay for egress
    * In the future you may be charged for AZ to AZ movement
* You might have an optional public IP, another self contained resource, that is binded to the NIC
* You might have a NSG as well that is attached to the subnet (preferred) or the NIC directly
