# Bonus

Sometimes the problem with parameter sniffing is that SQL does not sniff a query because of a lack of parameter:

<pre class="language-sql"><code class="lang-sql"><strong>/*SQL Server Sniffs 'Answer' from the PostTypes table, 
</strong><strong>but does not sniff the PostTypeID associated to that PostType*/
</strong><strong>SELECT COUNT(*)
</strong>FROM dbo.PostTypes pt
INNER JOIN dbo.Posts p ON pt.Id = p.PostTypeId
WHERE pt.Type = N'Answer'

/*SQL Server appropriately sniffs the PostTypeID and gets a perfect estimate*/
SELECT COUNT(*) FROM dbo.Posts
WHERE PostTypeID = '2' /*Answer*/
</code></pre>

* In the first example above, SQL does SHOW\_STATISTICS on an index on Posts for the PostTypeID, but it uses the density vector instead of building a plan for the PostTypeID we care about, which is 2 (answer)
* To fix this, you can declare a variable called @PostTypeID and then simply select the PostTypeID into the variable and then use that variable in the WHERE clause of the query
  * In order for SQL Server to build the appropriate plan that sniffs the variable, you will need to use OPTION(RECOMPILE) on the query
    * Alternatively, you can use dynamic sql
