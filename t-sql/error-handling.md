# ❌ Error Handling

## Stored Procedures

TRY..CATCH only catches medium level SQL errors!

* This does not include failovers, system outages, etc

Even if you use BEGIN TRAN and COMMIT TRAN, the transaction will remain open if the user closes the query mid-transaction and will result in a "Zombie" Thread

