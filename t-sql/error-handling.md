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

![](<../.gitbook/assets/image (1) (3).png>)

* In the above image, you still want to be sure to return errors to the client with **THROW** or **RAISERROR**
  * RAISERROR is more powerful, but THROW is easier to implement&#x20;

![](<../.gitbook/assets/image (1) (1) (2).png>)



### Debugging

Table variables ignore ROLLBACK, which make them perfect for debugging&#x20;

![](<../.gitbook/assets/image (2).png>)

![](<../.gitbook/assets/image (1) (2).png>)



