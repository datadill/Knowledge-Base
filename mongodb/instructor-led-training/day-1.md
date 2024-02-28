# DF100

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



### Reading Documents

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
* db.customers.find({lastpurchase: {$exists: false})
  * Returns any document where lastpurchase does not exist
* db.customers.find({lastpurchase: {$exists: true, $eq: null})
  * Returns any document where last purchase field exists and the value is set to NULL
* db.fun.find({hobbies: {$all: \["rockets", "cars"]\}})
  * returns any documents where the array contains all of those values, but the array itself could contain more values
  * It is an AND operator
* db.fun.find({hobbies: {$in: \["rockets", "cars"]\}})
  * OR variant
* db.ages.find({age: {$lt: 39, $gt: 21\}})
  * when testing an array,. the moment you implement an operator document, you are now testing whether the SET of values in the array are going to meet the criteria
  * age: \[40,20,8] would evaluate as true for the above logic because a value of less than 39 exists and a value of greater than 21 exists
*   db.ages.find({age: {$elemMatch: {$lt: 39, $gt: 21\}}})

    * elemMatch is an array operator that will walk the value of the array until it finds one element that must match ALL of the criteria
    * The caveat to this is that $elemMatch only executes against an array and will not return documents that have ints

    <figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>



### Updating Documents

* updateOne vs updateMany
  * updateOne(query, change) - changes only the first matching document
  * updateMany(query, change) - changes all matching documents

Operators

* Ex.  {$inc: {score: 50, numGames: 1}, $push: {gameId: 22, winLoss: "win"\}}
  * Because all of this is being done in the same "mutation" document, you are guaranteed an atomic operation
* $set - assign or replace a value on an existing document
  * use dot notation to set a field in an embedded document
    * be extra cautious when setting a NEW value inside of an embedded document as you are going to erase the entire existing field and replace it with your new value unless you use the dot notation
    * {$set : { staff: {principal: "jones"} } vs {$set : { "staff.principal": "jones"} }&#x20;
* $unset - remove a field from a document
  * the value you set can be a "" blank string, it does not matter
  * it does NOT set the value to NULL, it physically removes the data
  * Ex. $unset: {"Singer":""\}}) is the same as $unset: {"Singer":"myrandomvalue"\}})&#x20;
    * This is because it needs to be the standard across the mongodb environment
* $inc / $mul - self explanatory
  * Their are no decrement or divide operators
    * Instead you will use a fractual value between 1 and 0 or a negative value&#x20;
* $max / $min - can modify a field depending on its current value
  * you could do a read + update, but their are problems with that..
    * problem #1, the value you are trying to update could change between your read and update
    * problem #2, you incur additional load on the database because it is two operations
  * $max / $min is essentially like adding a conditional to prevent an update
  * $max makes it so that you can avoid an index on a field and increases performance
    * This is more efficient than using $gt
    * You find all objects that satisfy your conditional and on that document evaluate whether the value is less than, if so, update the value, otherwise do nothing
  * Most common use cases are dates and numbers
    * e.g. you want to $max date when you want to update a date changed field

### Deleting Documents

* Recommended to find or findOne what you want to delete before deleting to verify the command is valid
* deleteOne() and deleteMany()
  * $unset of the UPDATE command only removes a field whereas delete removes a document
* replaceOne() is typically not used&#x20;
  * It erases everything on the document except the \_id field and replaces it with the content you are trying to set
  * Typically you would just use $set

## Updating, Locking, and Concurrency

* If two processes attempt to update the same document at the same time they are serialised
* The conditions in the query must always match for the update to take place
* In the example, if the two updates take place in parallel - the result is the same
* Locks are at the document level

Ex. In below example, transaction B does nothing

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

* This is where stuff starts to back up and you run out of CPU
* Furthermore, every single time the lead blocker runs it's operation, all of the queries in queue must re-evaluate

## Advanced Arrays

$push - append and element to the end of an array

* Can be used in updateOne and updateMany
* Fails if the field is not an array
* Creates an array field if it does not already exist
* Can be used with multiple modifiers
* Ex. db.playlists.updateOne({name:"funky"}, {$push: {name: { artist: "AC/DC, track: "Thunderstruck\}}
  * name must not be a string, it has to be an array

$pop - removes last or first element from an array

* Can be used in updateOne or updateMany commands
* Fails if the field is not an array
* Removing the first element renumbers all array elements
* Ex. db.playlists.updateOne({name:"Funky",{$pop: {tracks: 1\}})
  * You can also use -1 to delete the **first** element in the array

$pull - remove specified elements from an array

* Elements can be specified by value or condition
* Will throw an error if not an array

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

$addToSet - appends an element to an array if it does not already exist

* Does not affect existing duplicates in the array
* Elements in the modified array can have any order
* Fails if the field is not an array

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

$each - if you use $push to add an array to an existing array, it will nest the array so you need to use $each to add multiple values

* Faster performance than using a FOR loop in code
* You can also combine $each and $sort to insert new data in an ordered fashion
  * To be clear, this reorganizes the entire array as the push occurs
  * Note: the benefits of this command don't make much sense if you can't trust that all applications are modifying documents with $sort as the benefit only comes when you read a document and do not have to specify $sort

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

$sort and $slice - sort and keep the top (or bottom) N elements

* This is an example of a design pattern
* Used for high/low lists - high scores, top 10 temperatures, etc
* Order of operations applied (left to right) matters, which is contradictory to an earlier discussion

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Modifying a specific element in an array

* Use the index of the array to target and modify the value

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

* You can also use "hrs.$" syntax to change the first index that matches the condition:

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Modifying all matching elements

* Query to find documents is not used to decide what elements to change
* separate arrayFilters(s) apply update to matching array elements
* this example adds 2 to everything less than 1 hr
  * **nohrs** is like a variable in the below example
* You must use arrayFilters when updating multiple items within an array

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Expressive Updates

* Mongo, unlike SQL Server, will actually persist the value of area so that subsequent reads do not have to recalculate the value
* Note: if somebody modifies $w or $h field, I am not sure what happens as the instructor did not cover it

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Upsert

* Most mongodb operations taht update also allow the flag "upsert: true"
* Upsert inserts a new document if none are found to update
* Values in both the query and update are used to create a new record

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

fineOneAndUpdate()

* To understand this command, you must first understand updateOne()
  * updateOne() finds and changes document atomicly and doesn't return the updated document unless you do a fineOne() afterwards (two separate transactions)
* Imagine getting the next one-up number from a sequence
* fineOneAndUpdate() prevents a potential race condition

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
