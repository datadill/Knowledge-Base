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
