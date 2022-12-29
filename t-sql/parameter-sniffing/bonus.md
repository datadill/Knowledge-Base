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
