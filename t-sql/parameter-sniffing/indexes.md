# Indexes

```
CREATE OR ALTER PROC dbo.usp_TopScoringPostsByDate
	@StartDate DATETIME, @EndDate DATETIME AS
BEGIN
SELECT TOP 200 p.Score, p.Title, p.Body, p.Id, p.CreationDate, u.DisplayName
  FROM dbo.Posts p
  INNER JOIN dbo.Users u ON p.OwnerUserId = u.Id
  WHERE p.CreationDate BETWEEN @StartDate AND @EndDate
  ORDER BY p.Score DESC;
END
GO

/* And remember that we have this index: */
CREATE INDEX CreationDate ON dbo.Posts(CreationDate);
```

The above stored proc is asking for the most popular questions and answers submitted in the given date range

* The index does not cover this query, which forces SQL to make a decision
  * SQL might seek into a very small range and then do a key lookup to get all the columns in the SELECT
* However, if you pass in a large date range then SQL will choose to scan the table
* The goal is to make this proc work well for everyone by changing the index strategy, not necessarily to make it fast for people that pass in a big date range&#x20;

If we add an index on CreationDate, Score... SQL Seeks into the date range and does a sort wth a TOP operation

* If we run it for a date range with 1 row, SQL does the key lookup BEFORE the SORT, which is kind of odd behavior
  * Ideally, SQL would seek into the date range, do the SORT, and then do the key lookups to remove the tipping point
* If we run it for a large date range it still does a SCAN on the table despite only needing the top 200 records
  * SQL Seek operations are handcuffed to Key Lookups and must be performed in unison

In order to fix this issue, you can break the query up into phases

* CTEs and Temp Tables are good examples
* Ex. In a CTE, go find the top 200 and ORDER BY, but only use the columns contained within an index.. only after that is done should you call back to the CTE and do your JOINs to get the wide columns previously left out via Key Lookups
* When you cache the plan for the small date range and run it for the large one, the query still finishes fast, but the SORT spils to disk.. but.. that doesn't really matter
* When you cache the plan for the large date range and run it for the small one, the query is still fast, but it now goes parallel

From Brent:

Fixing parameter sniffing with indexes is all about giving SQL Server a narrower copy of the data to reduce the blast radius.

Sometimes we have to encourage SQL Server to use the index by breaking the work up into different phases.

WE STILL HAVE PARAMETER SNIFFING. These plans can have different:

* Parallelism
* Memory grants

But they will at least look CLOSER than they looked before, and it may not matter AS MUCH which one goes in first.

If your biggest challenge in a parameter sniffing problem is deciding between an index seek vs key lookup, your goal is to reduce the number of key lookups that SQL Server is forced to do. Give it enough in the index to let it do the filtering necessary.

The index helps you find the rows you want.

Once you've found the rows you want, 100-10,000 key lookups isn't a big deal at all (and the numbers may go even higher on bigger databases.) Although if someone says they want more than 10,000 rows on a single report, I'm like look, buddy, it's time to do table scans.

If the biggest problem you're trying to solve is the choice between an index seek + key lookup versus a table scan, your goal is to find the parts of the filtering & sorting that require key lookups, and see if you can move those to the index instead.

Even the index alone may not cut it: if we can't fully cover the query, we may need to break the query into phases so that we can do a sort before we do a key lookup.

If the biggest problem is choosing between two indexes on the same table, index tuning can help, but it's probably not going to be the only solution by itself. We're probably also going to have to introduce branching logic or a recompile hint to let ourselves get different query plans for different sets of parameters.

If the biggest problem you're trying to solve is which table to process first because different parameters should focus on different tables, indexes alone won't be enough.
