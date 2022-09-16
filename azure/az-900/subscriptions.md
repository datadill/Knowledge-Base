---
description: >-
  Source:
  https://www.youtube.com/watch?v=9vKAYW_WkLo&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=11
---

# Subscriptions

A Subscription is a base unit of an agreement between a customer and microsoft

* This is a billing boundary as well as a boundary for security
* Azure AD is a tenant within azure that contains groups, users, devices, etc
* Every subscription trusts 1 and only one 1 Azure AD tenant
* You can once again apply budget, RBAC, and policies to subscriptions
  * You might have one or more RGs in that subscription with their own budgets or policies, but by default they will inherit from the subscription
* You might have a Prod subscription and a Dev subscription
* You can move some stuff between subscriptions, but that is rather limited so you should always try to provision in the proper subscription

