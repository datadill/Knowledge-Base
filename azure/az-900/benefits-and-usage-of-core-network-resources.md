---
description: >-
  https://www.youtube.com/watch?v=aNK0C9Oj2sg&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=17
---

# Benefits and Usage of Core Network Resources

A virtual network resources can not span regions

* It is confined by the subscription -> region
* At least one IPv4 CIDR
  * Typically use RFC 1918
    * This was created so that each company could use the same IP internally, but when deploying externally it would translate publicly
* A vnet might have several subnets
  * Ex. 10.01/24, 10.02/24, 10.3/24
  * vNets/subnets can span Availability Zones
  * In Azure you lose 5 IPs per subnet
* When picking an address space, you must pick a unique one as otherwise things will start breaking
  * It is possible to "peer" networks, which provides the ability to allows vNets to connect to each other across regions or even completely separate tenants
  * This is where you must be careful that you don't overlap IPs
* You can connect your on-prem network to Azures vNet
  * You can do this over the internet via a site-to-site VPN with a VPN gateway in Azure
    * Policy based gateways only allow for one connection, it is static, and is limited in use
      * This should be avoided
    * Route based gateways allows for multiple connections and allows for point-to-site
      * Some developer sitting at home might want to be part of the network
      * It also supports ExpressRoute, which is private
  * Lots of resources don't live in a vNet
    * Ex. Storage accounts&#x20;
      * A storage account can have a publicly facing endpoint that has a firewall so you can allow services from external vNets
        * These are called Service Endpoints
      * Alternatively, you can disable the public IP of the storage account and instead use a private endpoint where you establish an IP within the vNet that the source resource is contained within to be able to communicate with the external storage account
