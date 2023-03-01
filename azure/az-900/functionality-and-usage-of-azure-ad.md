---
description: >-
  https://www.youtube.com/watch?v=3JCxrM_Y4Go&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=43
---

# Functionality and Usage of Azure AD

Azure AD is a cloud identity provider

* A tenant is what stores users, groups, policies, computers, etc
* It works with OIDC, SAML, WS-FED, OAuth 2.0, etc that allows us to interact with other services

Active Directory Domain Services

* Similar, but different to Azure AD
* Supports Kerberos, NTLM, LDAP, etc
* Azure AD is completely flat whereas on-prem AD is hierarchical in nature
  * It supports group policy and delegation
* Azure AD Connect provides the ability to syncronize on-prem AD to cloud AD



3rd party SaaS solutions can be setup to trust a given AD tenant

* Office 365 along with other Azure services also supports this setup providing a central point of governance
