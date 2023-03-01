---
description: >-
  https://www.youtube.com/watch?v=flCoRc1uv9o&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=39
---

# Functionality and Usage of NSGs

You might have 2 vNets in Azure that can be peered together to create one universal/free-flowing vNet that shares IPs

* Additionally, you could have a private network that connects via site to site VPN
* This vNet by default has outbound internet connectivity as well as other various Azure services

Network Security Group has to be in the same subscription/region

* Has a name, priority, rules, etc
* By default, NSGs deny all inbound traffic and allows for all outbound traffic
* Service Tags represent a set of ip addresses for a particular resource such as a storage account, which makes it easier to manage firewall rules
* Can be applied to a subnet for ease of management, but they are enforced on the host of the VM of the physical switch
  * Can also be applied to a NIC, but that makes it harder to manage
