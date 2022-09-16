---
description: >-
  Source:
  https://www.youtube.com/watch?v=g4u0NL2-3XM&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=13
---

# Azure Resource Manager

Built around the idea of resource providers

* Think about Azure as a big cloud service that is made up of resource providers
* You have Virtual Machines, Disk, Images, Snapshots, etc..
  * These are defined within the Computer resource provider
  * There is also a Billing resource provider as another example
* An Azure VM is made up of 5 resources
  * VM, NSG, Network Interface, Disk, and Public IP Address
    * These are all its' own resources with their own access controls, tags, activity log, etc
* With ARM, it doesn't matter if you are using Powershell, the Azure Portal, REST API.. it does not matter since they all communicate with ARM
* You can create a JSON file, which is declarative, to quickly spin up resources with ARM
* BICEP is a more friendly JSON file
