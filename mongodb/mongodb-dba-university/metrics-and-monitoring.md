# Metrics & Monitoring

### Core metrics

* Query targeting
  * Ideal ratio is 1 where a document is returned for every one read
  * very high ratio negatively impacts performance
* Storage
  * writes are refused at capacity and can cause crashing
  * key metrics include disk space percent free, disk latency, disk IOPs, disk queue depth
* CPU
  * May need to optimize with indexes or upgrade hardware
* Memory
  * System should be sized to hold all indexes
  * Swap usage, and memory usage
*   Replication lag

    * delay between primary and secondary in seconds



### Additional Metrics:

* opcounters
  * number of operations per seconds run on mongodb process since startup
    * It tracks: command, query, insert, delete, update, getMore
* network traffic
  * Average rate of physical bytes
    * bytes in / bytes out (physical)
  * Number of requests sent to DB
    * numRequests
* connections
  * Organized by application, shell client, as well as internal processes
  * Can affect system performance
  * Large connection count may be suboptimal connection strategy
* tickets available
  * when available tickets drops to zero, other operations must wait
  * indicates undersized cluster or poorly performing queries



### Atlas CLI

* atlas metrics processes \<host\_name>:\<port>
  * You can also add params like period, granularity, output, type, etc
* Atlas also has real-time monitoring charts
* You can kill long running operations in this dashboard

atlas processes list

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

atlas metrics processes atlas-jj12z4-shard-00-00.p31f3ej.mongodb.net:27017 --period P1D --granularity PT1M

* P1D stands for 1 day
* PT1M is 1 minute intervals

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

or..

atlas metrics processes atlas-jj12z4-shard-00-00.p31f3ej.mongodb.net:27017 --period P1D --granularity PT1M --output json --type CONNECTIONS | jq '.measurements\[0].dataPoints |= .\[-10:]'

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>



### Configure Alerts

* You can configure alert settings at organization and project levels
* Must have "project owner" role
* Shared tiered clusters will only triggers alerts for:
  * connections, logical size, opcounters, network
* All Atlas projects come with defaults, but they can be edited
  * Atlas alerts are a little bell icon in the top right

#### CLI

```
atlas alerts settings list --output json
```

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### Responding to Alerts

Alerts are shown here:

<div align="left" data-full-width="false">

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

</div>

* Notifications will continue until an alert is acknowledged
  * No further notification are sent until the acknowledgement period ends, you resolve the condition, or you unacknowledge the alert

#### CLI

```
atlas alerts list --output json
```

```
atlas alerts acknowledge <alertId> --comment <comment>
```

```
atlas alerts unacknowledge <alertId>
```



### Integrations

Good for hybrid situations or for when you are migrating on-prem to cloud

Examples:

* Prometheus, pagerduty, datadog, sumo, splunk, custom web hooks, etc
  * Prometheus and DD are only on M10+ clusters

In database dashboard, click elipses and go to Integrations

* Typically you fill in your credentials here for your connection



### Self-Managed Monitoring

Cloud Manager or a hybrid solution listed above can be used

* note: prometheus for example can not directly collect metrics from an onprem solution, but their are open source connectors such as Percona
* The account collecting data will need clusterMonitor role



### Command Line Metrics

serverStatus

* diagnostic command that returns a document showing current instance state
* This command is used by monitoring platforms to collect valuable metrics
*   Ex:&#x20;

    ```
    db.runCommand(
       {
         serverStatus: 1
       }
    )
    ```
* Ex helper command: db.serverStatus()

currentOp

* admin command that returns document about active operations
* monitoring apps use this command to find slow operations
*   Ex.&#x20;

    ```
    db.adminCommand(
       {
         currentOp: true,
         "$all": true
       }
    )
    ```

killOp

* terminates operations using opId
*   Ex.&#x20;

    ```
    db.adminCommand(
       {
         killOp: 1,
         op: <opid>,
         comment: <any>
       }
    )
    ```
* Helper function: db.killOp()
