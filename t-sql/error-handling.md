# ❌ Error Handling

## Stored Procedures

TRY..CATCH only catches medium level SQL errors!

* This does not include failovers, system outages, etc
* Even if you use BEGIN TRAN and COMMIT TRAN, the transaction will remain open if the user closes the query mid-transaction and will result in a "Zombie" Thread
  * Always use ROLLBACK in your CATCH block
* What if we use BEGIN TRAN..COMMIT TRAN wrapped in BEGIN TRY..BEGIN CATCH?
  * Same problem as before.. no rollback and transaction remains open
* To fix this issue, we must use SET XACT\_ABORT ON;
  * "No matter what kind of error occurs, bail the whole thing out"
  * This method catches **almost** anything

![](../.gitbook/assets/image.png)



