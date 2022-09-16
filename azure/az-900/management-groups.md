---
description: >-
  Source:
  https://www.youtube.com/watch?v=bPdDiEtCVhM&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=12
---

# Management Groups

What if you had hundreds of subscriptions and wanted to manage them all the same?

* Management Groups are the solution!
* AAD Tenant would be at the top of the pyramid
  * You can then add a root management group, which can have up to 6 levels of management groups
    * Does not include the root or subscriptions underneath
  * Under your root management group, you might have a dev management group and a prod management groups
    * You can apply budgets, RBAC, or policies to each management group that inherits downwards
