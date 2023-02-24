---
description: >-
  https://www.youtube.com/watch?v=bPNkXwRFsek&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=18
---

# Public/Private Endpoints

A public endpoint is an address that can be communicated with over the internet

* That doesn't mean anybody can talk to it because it still requires authentication&#x20;
* In your vnet/subnet you might create a service endpoint that is then allowed to access the firewall of your public endpoint resource

A private endpoint is an IP address within a subnet that we specify that represents a connection to a very specific resource such as a storage account

* You might have a second vNet that has a peering connection to vNet1 or even an on-prem network that connects to vNet 1, which then has access to utilize that private endpoint&#x20;
