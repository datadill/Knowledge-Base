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

If you have to Insert 200 documents, is it better to use insertMany or insertOne in a FOR loop?

* insertMany is better because the network roundtrips are lessened



* You can insert up to 100,000 documents or 48mb in a single trip



#### Reading Data

* db.customers.findOne({})
  * Returns the first record
* db.customers.findOne({name: "Andy Smith"})
  * Strings are case-sensitive
* db.customers.findOne({name: "Andy Smith", spend: 0})
  * The above is an AND operation
* db.customers.findOne({name: /smi/i, spend: 0})
  * REGEX expressions work, but you should be using a text index or Atlas Search if you use Atlas assuming you do this operation frequently
* db.customers.findOne({name: /smi/i, spend: 0.0000})
  * For numerical values, regardless of precision, Mongodb will still return the spend for customers with a spend of 0
  * In application code, you should use the proper data types as it will be mapped within Mongo

Looking for multiple documents?

* db.customers.find({})
  * Equivalent of SELECT \*
* db.customers.find({},{name: 1, spend: 1})
  * 1 specifies that the field will be returned
  * \_id is always returned
  * conversely, if you have 0, it will exclude just those fields
  * you can NOT mix and match inclusion/exclusion with ONE exception
    * You can explicitly exclude the \_id field while also including others
    *
    * db.customers.find({lastpurchase: null})
  * Will return documents even where lastpurchase field does not exist as it implicitly defines the value is null
  * db.customers.find({gibberish:null})
    * Returns every single document
  * find vs findOne
    * find will return a cursor that has a maximum of 100 documents or in some drivers, 16mb (C# is 48mb)
      * A lot of drivers will obfuscate this behavior and iterate through the cursor for you
    * If no documents are found, it will still return the object, but it will be empty
  * db.customers.find({}).sort({age: -1}).skip(30).limit(10)
    * Commands can be chained
    * \-1 is descending
    * order of chained operations does not matter, but in order to not confuse people, always write by **sort**, **skip**, and then **limit** for best practices
      * You can change this behavior with an "aggregate query", but we have not yet learned what that is
  * db.customers.find({}).sort({age: -1, plate: -1}).skip(30).limit(10)
  * .count() vs .countDocuments() vs .countDocuments({})
    * they should almost always be the same, but their is a VERY extreme edge case where you are dealing with ultra precise application and high frequency apps
      * .countDocuments() is faster
      * .count()&#x20;
      * .countDocuments({}) with an empty document cheats and uses the metadata stats, but it could be off by +/- 1
  * db.people.find({address: {city: "Houston"\}})
    * This will work ONLY if the field names, field order, and values match identically because mongodb creates a blob of the document
    * The reason for this is because in Mongodb allows for 100 levels of nesting so it is more performant to just hash the document
    * In the real world, you would typically not use the above syntax, but rather would use the below syntax
    * **VERY IMPORTANT: querying on an embedded document like above will rehash every single document on the fly, it does not save this hash on an INSERT**
    * **Embedded documents include arrays and nested documents**
  * db.people.find({"address.city": "Houston"})
    * when referencing a child field (nested document), you MUST have double quotes around the key in the JSON document in your FIND operation
  * db.fun.find({hobbies: "rockets"})
    * MongoDB will walk an array and return a document if rockets exists in the array
  * db.fun.find({hobbies: \["rockets, "cars"]})
    * Since this is an embedded document, it must be an exact match
  * db.taxis.find({age: {$gt: 37} } )
    * Operator documents are always prefixed with a $
  * db.taxis.find({age: {$gt: 37}, plate: {$lt: 20, $lte: 50, $ne: 38, $in: \[40,44,45] } } )
    * This operates in AND behavior
    * operators can be on the same field as well
  * db.pets.find({$or: \[{species: "cat", color: "black"},{species: "dog", color: "brown"}] })
    * nesting boolean logic is valid
    * ORDER within the query document does not matter unless you are referencing embedded documents
    * ORDER within the OR array does matter in the sense that the query will stop executing when the first condition is satisfied
      * In terms of performance, it may be faster for your query to start with the condition that is more frequent
  * db.pets.find({color: {$not: {$eg: "Brown" } } })&#x20;
    * Find all documents where color is not brown
    * The same as: db.pets.find({color: {$ne: "Brown" } } )
    * $not is considered an Operator document
*





