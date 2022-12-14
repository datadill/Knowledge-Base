# Branching

```sql
CREATE OR ALTER PROC dbo.usp_RptUsersByReputation @Reputation INT AS
BEGIN
	SELECT TOP 1000 *
	FROM dbo.Users
	WHERE Reputation = @Reputation
	ORDER BY DisplayName;
END
GO

/* These two get different plans: */
EXEC usp_RptUsersByReputation @Reputation = 2 WITH RECOMPILE;
EXEC usp_RptUsersByReputation @Reputation = 1 WITH RECOMPILE;
```

* Reputation = 2 returns few records while 1 returns a ton
* Reputation = 1 wasted 5gb of RAM (25% of server RAM), but doesn't show a yellow bang whereas Reputation = 2 does even though it only wastes a couple MBs
* When you cache for the large and run for the small, the small query runs fast, but consumes a lot of cores going parallel and most importantly 5gb of memory
  * When you cache it the other way around, the small query runs fine, but the large one takes forever because it doesn't go parallel, it does key lookups, and it doesn't have enough memory
    * This indicates that there is no one single plan that will solve this probem
  * Branching with IF statement does **not** work



**Dynamic SQL**

```sql
CREATE OR ALTER PROC dbo.usp_RptUsersByReputation @Reputation INT AS
BEGIN
	DECLARE @StringToExecute NVARCHAR(4000) = N'SELECT TOP 1000 * FROM dbo.Users
		WHERE Reputation = @Reputation
		ORDER BY DisplayName;';
	EXEC sp_executesql @StringToExecute, N'@Reputation INT', @Reputation;
END
GO
```

* The above still gets sniffed because the text is the same
  * In order to fix this, we can use comment injection to change the text of the query

```sql
CREATE OR ALTER PROC dbo.usp_RptUsersByReputation @Reputation INT AS
BEGIN
	DECLARE @StringToExecute NVARCHAR(4000) = N'SELECT TOP 1000 * FROM dbo.Users
		WHERE Reputation = @Reputation
		ORDER BY DisplayName;';

	IF @Reputation = 1
		SET @StringToExecute = @StringToExecute + N' /* Big data */';

	EXEC sp_executesql @StringToExecute, N'@Reputation INT', @Reputation;
END
GO
```

* In the above code, adding IF @Reputation = 1 adds technical debt

Traffic Cop is another solution to achieve branching that allows you to have 2 separate procs with the exact same code, but each proc has their own execution plan

* In order to try and find big/small data, you can run a COUNT(\*) on the primary table to see how many records might be returned, but the issue with this approach is that you are counting from a table every single time
  * To get around this, you can use a temp table on the inner proc that is generated from the outer proc so that you aren't doing unnecessary work
    * _Not shown below, but you could also put @@ROWCOUNT below the INSERT INTO #MatchingUsers to avoid the SELECT COUNT(\*)_

```sql
CREATE OR ALTER PROC dbo.usp_RptUsersByReputation_Joins @Reputation INT AS
BEGIN
	CREATE TABLE #MatchingUsers (Id INT);
	INSERT INTO #MatchingUsers (Id)
		SELECT Id
		FROM dbo.Users
		WHERE Reputation = @Reputation;

	IF 10000 < (SELECT COUNT(*) FROM #MatchingUsers)
		EXEC usp_RptUsersByReputation_Joins_BigData @Reputation = @Reputation;
	ELSE
		EXEC usp_RptUsersByReputation_Joins_SmallData @Reputation = @Reputation;
END
GO

/* Because then you can use the temp table in the child stored procs: */
CREATE OR ALTER PROC dbo.usp_RptUsersByReputation_Joins_BigData @Reputation INT AS
BEGIN
	SELECT TOP 1000 *
	FROM #MatchingUsers m
		INNER JOIN dbo.Users u ON m.Id = u.Id
		INNER JOIN dbo.Posts p ON u.Id = p.OwnerUserId
		INNER JOIN dbo.Comments c ON p.Id = c.PostId
		INNER JOIN dbo.Users uCommenter ON c.UserId = uCommenter.Id
		INNER JOIN dbo.Badges b ON uCommenter.Id = b.UserId
	WHERE u.Reputation = @Reputation
	ORDER BY u.DisplayName;
END
GO
```

What to take away from this demo:

* All of the code in a batch gets compiled at once initially.
* If you want a different plan, you have to:
  * Ask for it - like with a RECOMPILE hint
  * Change a lot of data, forcing stats to update
  * Postpone compilation for part of the query: (like build a child stored procedure or dynamic SQL)
  * Comment injection can be super powerful and low maintenance
* But when you do any of the above, you're building technical debt: if the data distribution changes over time, you may need to revisit the triggers you used to spawn the branching logic.
