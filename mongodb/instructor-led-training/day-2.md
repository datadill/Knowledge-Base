# DF200

## Indexes and Optimization

* Values in an index point to document identity
  * As a result, if the document moves the index doesn't change

## Index Prefix Compression

* MongoDB indexes use a special compressed format and you can not change it
* Each entry is a delta from the previous one
* If there are identical entries, they need only one byte
* As indexes are inherently sorted, this makes them much smaller
* Small indexes require less RAM
* Indexes and collections exist in memory

## Index Misconceptions

* MongoDB is so fast that it doesn't need indexes
* Every field is automatically indexed
  * The only field is automatically indexed is \_id
* NoSQL uses hashes, not indexes



## When to use an Index

* A good index should support every query or update
* Scanning records is very inefficient, even if it is not all of them
* The developer should determine the best index

## Using and choosing indexes

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

### Unique Indexes

* Indexes can enforce a unique constraint with the following flag:
  * {unique: true}
* NULL is a value and so only one record can have a NULL in unique field

### Partial Indexes

* Partial indexes index a subset of documents based on values
* Can greatly reduce index size
  * {partialFilterExpression: {archived: false } }

### Sparse Indexes

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

### Multikey Indexes

* A multikey index is an index that is on an array
* Can index primitives, documents, or sub-arrays
* Are created using createIndexes() just like single-field indexes
  * If any field in the index is ever found to be an  array than the index is multikey
* An index entry is created on each unique value found in the array

### Compound Indexes

* Based on more than one field
  * Most common type of index
  * Same concept as RDBMS indexes
  * Up to 32 fields
  * Created like a single field index
* The field order and direction is very important
* Can be used as long as the first field in index is in the query
  * Other fields in the index do not need to be in the query

Field Order Matters

* Equality First
  * What fields, for a typical query, are filtered the most
  * Selectivity is NOT cardinaility, selective can be a boolean choice
  * Ex. Normally male/female is not selective
  * Dispatched vs delivered is selective though
* Then sort and range
  * Sorts are much more expensive without indexes
  * Directions matter when doing range queries

ex:

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

* equality, sort, range (ESR) - methodology used 85% of the time
  * 15% of the time, you may want to use ERS instead

Ex.&#x20;

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

*   In above example, the index should swap and put the equality operator first like so:

    <figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>


* Another example:

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

If you are creating a multi-key index, only one of those values can be an array

* An error will be thrown if you try to index on multiple arrays within the same document
* The error thrown can happen on either an INSERT or the creation of the index itself



### Time to Live (TTL) Indexes

* Not a special index, but rather a flag
* MongoDB automatically deletes documents using the index based on time
* Background thread in server runs regularly to delete expired documents

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

* Alternative to the above example, you can use a field and  set expireAfterSeconds: 0
* Sometimes it is better to write your own programmatic data cleaner and schedule via CRON



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



## Indexing Arrays

* 1st example below uses index 1
* 2nd example below uses index 1
* 3rd example below uses index 2

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

* elements in a nested array cannot be indexed, but you can change the structure like this and index it:

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

## Finding Slow Operations

* Missing index is usually the most common cause
* Bad client code is a close second cause
* Two ways to locate:
  * database log
    * Gets last 1024 log entries
    * Returns a single document
  * profiling a database
* You can filter on "s" attribute for severity
* You can also download all of the logs directly from the mongo atlas
* db.setProfilingLevels(level,slowms)
  * 0 - profiler is off
  * 1 - store only slow queries
  * 2 - store all queries
  * slowms: threshold in milliseconds for slow operations

## Aggregations

* Aggregation allows us to compute new data
  * Calculated fields
  * summarized and grouped values
  * reshaped documents
* Aggregation is a pipeline
  * Each transformation is a single step known as a stage
  * This is easier to:
    * debug
    * understand
    * rewrite and optimize
  * Think of a powershell pipe!



Stages:

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Ex.

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

* Advised to put the $project at the end
* Everything done in aggregate is done in memory
* Unlike a find command, Aggregate is more controllable and reads from top to bottom

### Dollar Overloading

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

* In above image, the 3rd line reads like this:
  * Set a new field in the document called area with a value that is 5x10
* 1st line
  * Match stage means you are filtering
* 2nd line
  * You are altering or adding a new field
* 4th line
  * Set a new field and use a $map function against prices, assigning p as your iterator variable and walk over the array multiplying each value \$$p by 1.08

### Expressions

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In the above, the most nested value gets executed first and it goes up the chain
* Other arithmetic expressions:
  * $subtract, $add, $multiply, $divide
* String expressions:
  * $concat, $ltrim, $indexOf, etc
* Expressions can affect index usage just like SQL Server



General recommendation when writing aggregations is to break up the commands into stages/variables:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
