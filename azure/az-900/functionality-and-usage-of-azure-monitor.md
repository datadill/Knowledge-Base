---
description: >-
  https://www.youtube.com/watch?v=v68jL-l9Fww&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=31
---

# Functionality and Usage of Azure Monitor

In Azure you might have audit logs, activity logs, service health, resource metrics and logs, etc

* Azure Monitor has a built-in time series database where metrics are sent by default
  * Think of it like a central hub where logs can be accessed
* Logs are a little bit different
  * By default Azure might save 7 days, 14 days, etc
  * However, you can choose to send logs elsewhere
    * Event Hub is a published subscribed service where an external solution can take it from that location
    * Log Analytics Workspace can hold up to 2 years
* Azure Monitor has alert rules that analyze metrics or logs and generate alerts
  * You can also perform actions automatically&#x20;
    * Send SMS, call webhook, email etc
