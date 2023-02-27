---
description: >-
  https://www.youtube.com/watch?v=ZBXVAD4S0Tc&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=34
---

# Functionality and Usage of Key Vault

Good for storing keys/credentials/etc

* Supports 3 entities:&#x20;
  * Secret (read and write)
    * Ex. Password
  * Key
    * Can be generated/imported/take actions, but you can not export
  * Certificates
    * Helps manage the lifecycle and distribution



Permissions

* Access can be controlled via a policy, but it grants access to the entire vault
* RBAC (role based access control) allows you to grant permissions to specific objects



* Managed Identity is tied to a particular instance of a resource and can let an application authenticate to the key vault&#x20;
  * You would provide the apps identity, permissions to the key vault
