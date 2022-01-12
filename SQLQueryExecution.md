Overview 
An execution plan is a behind-the-scenes look at the road a query takes in order to deliver it’s final result. They are generated from the underlying data statistics combined with what the query and it’s parameters are trying to accomplish. When the initial query is read, the execution plan generation engine or “Query Optimizer” searches for the best way to deliver the results of the query in the quickest way possible. To do this, it needs to know what the underlying data looks like. This is facilitated by the statistics that are stored for each table, column, and index. With these statistics in hand, the goal is to eliminate the largest number of records as quickly as possible, and iterate through this process until the final result is delivered. That said, it is not an easy job. There are many variables that come into play when determining a query’s path. A few of these include the selection of indexes, join algorithms, join order, parallelism. 
 
Estimated Execution Plan - (CTRL + L) 
is created without executing the query and contains an approximate execution plan 
 
The estimated execution plan is designed to show what SQL Server would most likely do if it were to execute the query. Using statistics, it estimates how many rows may be returned from each table.  
It chooses the operators it will use to retrieve the data – scans or seeks. It decides how to best join the tables together – nested loops, merge joins, hash joins. It is a reasonably accurate guide to what SQL Server will do.  
 
Actual Execution Plan - (CTRL + M) 
is created after execution of the query and contains the steps that were performed 
 
The actual execution plan is shown after a query is executed. The difference here is that SQL Server can tell you exactly how many reads were performed, how many rows were read, and what joins were performed. 
 
Differences between Estimated Execution Plan Vs. Actual Execution Plan 
If you are experiencing differences in how the query is executed or how many rows are managed during the query, 
That might be the sign of a not well maintained index or statistics. 
In scenarios with huge differences SQL will let you know how to automatically fix issues. 
In scenarios with minor differences you will need to manually identify and treat index and statistics update. 
 
Reading 
Read the execution plan from right to left. The first paths the query takes are on the right hand side and extend all the way to the left until it gets to the final node. A cost percentage is assigned to each operation. 
 
This gives you a relative understanding of how expensive each operation is for that query. Sometimes this will lead you to find a missing index, modify an "existing index, or even break the query up into multiple parts. The cost can sometimes be misleading though. It is not always that the most expensive "operation is the slowest or most intensive.  
Hovering your mouse over each operation will display additional details about each operation. 
 
These drill downs include valuable information including the index or object being referenced, the columns that are output by the operation, the predicates (or filters), estimated and actual statistics.  
 
ToolTip item 	Description 
Physical Operation 	The physical operator used, such as Hash Join or Nested Loops.  
Physical operators displayed in red indicate that the query optimizer has issued a warning, such as missing column statistics or missing join predicates.  
This can cause the query optimizer to choose a less-efficient query plan than otherwise expected. For more information about column statistics, see Using Statistics to Improve Query Performance.  
When the graphical execution plan suggests creating or updating statistics, or creating an index, the missing column statistics and indexes can be immediately created or updated using the shortcut menus in SQL Server Management Studio Object Explorer. For more information, see Indexes How-to Topics. 

Logical Operation 	The logical operator that matches the physical operator, such as the Inner Join operator.  
The logical operator is listed after the physical operator at the top of the ToolTip. 
Estimated Row Size 	The estimated size of the row produced by the operator (bytes). 
Estimated I/O Cost 	The estimated cost of all I/O activity for the operation. This value should be as low as possible. 
Estimated CPU Cost 	The estimated cost of all CPU activity for the operation. 
Estimated Operator Cost 	The cost to the query optimizer for executing this operation. The cost of this operation as a percentage of the total cost of the query is displayed in parentheses. Because the query engine selects the most efficient operation to perform the query or execute the statement, this value should be as low as possible. 
Estimated Subtree Cost 	The total cost to the query optimizer for executing this operation and all operations preceding it in the same subtree. 
Estimated Number of Rows 1 	The number of rows produced by the operator. 
 
Missing Index Hints 
Many times, the execution plan will give you hints about what index could be added to speed up the query. You’ll see this at the top of the execution plan in green text. 
 
In order to view the details of the missing index, right click on the green text and choose “missing index details”. This will open a new window with the create statement for the index. 
 
Identify Parallelism 
With the Actual Execution plan you can also identify where and when parallelism is taking actions. 
Parallelism can be identified by its specific icon and test called "Parallelism" that is usually a Blue Arrow. 
Or a parallelized action can be identified with a Yellow Dot with 2 Black Arrows going from right to left. 
Example Below 
 

