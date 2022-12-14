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
