# Summary

Index Tuning: Reduce residual predicate and sorts

Query Hints: Take choices away from SQL Server

Recompile: For queries running <1 time per minute, always at the statement level and never the proc level as otherwise you lose visibility to the proc

Branching/Dynamic SQL: Postpones compilation until SQL Server knows more about the data



Diagnosing Sniffing Issues:

* Plan cache right now: limited info, no history, no parameters, no spills
* Plan cache over time with sp\_BlitzFirst: requires planning ahead, but way more powerful
* Live snapshots with sp\_BlitzWho: real parameters and real plans

