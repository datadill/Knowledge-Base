# Day 1

## Storage

MongoDB stores data as BSON:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

* In the above example, you can enforce uniqueness of "Dental" value for the "type" key by creating an index
* Additionally, you can put validation on the data itself, but their may be downsides to that

## Terminology

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* namespace is nothing more than database name + collection name that helps you distinguish between two similarly named collections

## Benefits of MongoDB

* Agility
  * The documents within a given collection are not required to have the same schema
    * ex. Data types do not have to match across documents
  * MongoDB's take is that the application should be doing the validation, not the database (although it is possible to do so on the DB side)
* Usability
  * Option to use a wide array of tools and languages to query MongoDB
* Utility
  * Complex indexed queries, smart edits, aggregation frameworks
* Availability and scalability
  * HA via replica sets, multiple copies of data, different hosts/locations, continuous replication
  * Scale via sharding
    * Partition data over multiple replica sets
    * Provides unlimited hardware scaling
  * Compression data
* Enterprise tooling
  * Atlas, Ops manager, cloud manager, K8s, terraform, etc
  * Ops manager - Very good to use once you start spinning up multiple replica sets and your environment gets more complicated. It is very difficult to do a PITR correctly on a sharded DB without using this tool

## When should MongoDB be used?

* high speed access to complex objects
  * atomic partial updates
  * fast retrieval
  * secondary indexes
  * aggregation capabilities
* When you want to store larger data structures together
  * Large arrays
    * Exception: keep arrays under 200 elements for performance
  * text fields
  * binary data
* Rapid development
* When you need to store structures of varying shapes

