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
