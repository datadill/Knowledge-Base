# Query Hints

```
/* In the index-tuning module, we hit a wall when we tried to use index tuning
alone to solve a tough choice between two indexes on this proc: */
CREATE OR ALTER PROC dbo.usp_TopScoringPostsByDateAndScore
	@StartDate DATETIME, @EndDate DATETIME, @MinimumScore INT AS
BEGIN
SELECT TOP 200 p.Score, p.Title, p.Body, p.Id, p.CreationDate, u.DisplayName
  FROM dbo.Posts p
  INNER JOIN dbo.Users u ON p.OwnerUserId = u.Id
  WHERE p.CreationDate BETWEEN @StartDate AND @EndDate
    AND p.Score >= @MinimumScore
  ORDER BY p.Score DESC;
END
GO
```

The above query gets an excellent plan when they each use their own respective indexes, but they don't work very well when parameter sniffing comes into play

* Users are really only concerned with the sniffing issue where timeouts are concerned and less so the situation where one query runs fast and the other is sort of slow, but not timing out
* You can use hints to help SQL choose the right index, but if you hint with the name and the index gets renamed then the query will start to fail
