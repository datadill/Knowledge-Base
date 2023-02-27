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
* A classic chicken and egg issue is that an app might try and get credentials, but it needs to authenticate to the key vault first
  * Managed identities provide an automatically managed identity in Azure Active Directory (Azure AD) for applications to use when connecting to resources that support Azure AD authentication. Applications can use managed identities to obtain Azure AD tokens without having to manage any credentials.
