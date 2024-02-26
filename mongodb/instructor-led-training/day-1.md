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
    * Sharding can be complex so it is important to consider this as your data grows
      * Do not plan to add sharding on day 1, but around 2 TB is when you want to start having that conversation
  * Compression data
* Enterprise tooling
  * Atlas, Ops manager, cloud manager, K8s, terraform, etc
  * Ops manager - Very good to use once you start spinning up multiple replica sets and your environment gets more complicated. It is very difficult to do a PITR correctly on a sharded DB without using this tool

## When should MongoDB be used?

* High speed access to complex objects
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
* Large data volumes
* Distributed data



## Things to be aware of

* Easy to get things wrong and performance can suffer
* DBAs need to be trained and certified
  * Devs perform traditional DBA tasks, but DBAs have very important tasks as well

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>



## CRUD Operations

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* If multiple documents satisfy the query for a single document command, it will return the first 1 on disk if it exists
  * Searching the index takes precedence
  * The same document may not always be returned unless you specify additional criteria

```
// db.customers.insertOne({
    _id: "bob@gmail.com",
    name: "Bob Smith",
    spend: 0,
    orders: [],
    lastpurchase: null
})
```

* In the above command, mongo will create the customers collection if it does not already exist
* at least one ID field must exists and one will be generated for you if you do not specify it
  * It is ALWAYS called \_id
  * This ID must be unique
  * **You can not change the ID of a document for any reason**
  * Mongo can and will generate a unique value for you
* If you are querying the ID field repeatedly and you know it is going to unique, then it makes sense to make that the ID&#x20;
  * For example, an email value.. although if the value changes you must delete the document
* If you let mongoDB generate the ID, it will be unique across the entire database, but it only MUST be unique across the collection
  * i.e. you could easily have the same ID across multiple collections if you set it to something like "myemail@gmail.com"

```
let friends = [ 
    {_id: "joe" }, 
    {_id: "bob" }, 
    {_id: "joe" }, 
    {_id: "jen" } 
]

db.collection1.insertMany(friends)
```

* In the above example.. Joe & Bob will all be inserted into the database
  * Both Joe (the duplicate record) and Jen are NOT inserted
    * This is the default insertMany behavior
      * Atomicity is at the document level
      * You can do multi-document transactions, but there are tradeoffs!
      * It is also possible to modify the default behavior
        * db.collection2.insertMany(friends, {ordered: false} )
          * In the above snippet, 3 records would be inserted, which includes "Jen"
* Tip: mongo is indexed at 0

