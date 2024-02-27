# Day 2

## Indexes and Optimization

* Values in an index point to document identity
  * As a result, if the document moves the index doesn't change

### Index Prefix Compression

* MongoDB indexes use a special compressed format and you can not change it
* Each entry is a delta from the previous one
* If there are identical entries, they need only one byte
* As indexes are inherently sorted, this makes them much smaller
* Small indexes require less RAM
* Indexes and collections exist in memory

### Index Misconceptions

* MongoDB is so fast that it doesn't need indexes
* Every field is automatically indexed
  * The only field is automatically indexed is \_id
* NoSQL uses hashes, not indexes



### When to use an Index

* A good index should support every query or update
* Scanning records is very inefficient, even if it is not all of them
* The developer should determine the best index

### Using and choosing indexes

* Mongodb checks in the plan cache to see if an optimal index has been chosen before and if not:
  * picks all candidate indexes
  * runs query using them to score
* Plan cache entries are evicted when:
  * using that index becomes less efficient
  * a new relevant index is added
  * the server is restarted

## Explain Verbosity

* queryPlanner shows the winning query plan but does not execute query
* executionStats executes the query and gathers stats
* allPlansExecutions runs all candidate plans and gathers stats
* default is "queryPlanner"

Ex.

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

* key metrics are in green
  * nReturned is number of documents returns
  * totalKeysExamined is number of index keys examined
  * totalDocsExamined is number of documents examined
* explain plans are complicated with nested stages of processing



Applying Explain()

* A flag is sent to the server with the operation to say it's an explain command
* **If the fucntion does not return a cursor, the explain flag needs to be set in the colleciton object instead before calling the function**
* We can set this one on the cursor as we do for sort() or limit()



Explainable Operations:

<figure><img src="../../.gitbook/assets/image (28).png" alt="" width="209"><figcaption></figcaption></figure>

## Creating an Index

* db.listings.createIndex({num ber\_of\_review:1})
  * name defaults to the field, but you can rename it
* In production, you need to be careful running this as it will cause blocking
  * **Their are workarounds, but the training does not talk about this**

## Index Types

<figure><img src="../../.gitbook/assets/image (29).png" alt="" width="244"><figcaption></figcaption></figure>

###

### Single-Field Indexes

* Specified as field name and direction
* The direction is irrelevant for a single field index
* The field itself can be any data type including an object
  * Indexes the whole object essentially as a comparable blob
  * Can be used to run range searches or exact matches
* Indexing an array is also possible, but it will be covered later

### Unique Index

* Indexes can enforce a unique constraint with the following flag:
  * {unique: true}
* NULL is a value and so only one record can have a NULL in unique field

### Partial Indexes

* Partial indexes index a subset of documents based on values
* Can greatly reduce index size
  * {partialFilterExpression: {archived: false } }

### Sparse Index

* Sparse indexes don't index missing fields
  * Ex. db.scores.createIndex({score:1},{sparse:true})
* These indexes are superseded by partial indexes

### Hashed Indexes

* Hashed indexes index a 20 byte md5 of the BSON value
* Support exact match only!
* Cannot be used for unique constraints
* Can potentially reduce index size if original values are large
* Downside: random values in a BTree use excessive resources
  * Ex. ({name: "hashed"})



## Listing Indexes

* getIndexes() on a collection gives us the info of existing indexes



## Performance

* Indexes improve read performance and potentially write performance
* Each index adds \~10% overhead for writes (Hashed indexes, multikey indexes, text indexes, and wildcard indexes can add more)
* An index is modified any time a document:
  * Is inserted (applies to most indexes)
  * Is delete (applies to most indexes)
  * Is updated in such a way that its indexed fields change

## Index Limitations

* Up to 64 indexes per collection, but you should avoid getting even remotely close to that
  * Write performance degrades to unusable between 20 and 30
* 4 indexes per collection is a good recommended number, but the instructor recommended less than 10 in the real world





###
