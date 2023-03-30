# RECOMPILE

When adding OPTION RECOMPILE, consider the compile time it takes and the effect that has on the CPU

* 6ms compile time may not seem like a lot, but if you execute a query 1000 times a second then you are using 6 cores 24/7 just for that one query to compile new plans

Finding the time SQL spends on compilations can be difficult, but Erik Darling has a procedure that creates an EE session to track this

* EXEC dbo.sp\_HumanEvents @event\_type = 'recompilations', @seconds\_sample = 30;

Brent: "If a query runs once a minute or less, OPTION RECOMPILE your heart out"

Partitioning can be an excellent way to improve performance, but it can also have a negative impact on recompilation time as SQL considers additional performance enhancements which make the plan compilation take longer&#x20;
