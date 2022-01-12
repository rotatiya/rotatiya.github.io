What is the purpose of this article ? 
Important Performance Counters of SQL Server and Windows Server  
that you can use to troubleshoot/monitor performances. 
       
Article 
 
1. SQLServer: Buffer Manager: Buffer cache hit ratio 
The buffer cache hit ratio counter represents how often SQL Server is able to find data pages in its buffer cache when a query needs a data page.  
The higher this number the better, because it means SQL Server was able to get data for queries out of memory instead of reading from disk.  
 
You want this number to be as close to 100 as possible. Having this counter at 100 means that 100% of the time SQL Server has found the needed data pages in memory.  
A low buffer cache hit ratio could indicate a memory problem. 
The higher the value, the better. 
 
You can find the value of the Buffer Cache Hit Ratio by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:Buffer Manager' and [counter_name] = 'Buffer Cache Hit Ratio' 
 
 
2. SQLServer: Buffer Manager: Page life expectancy 
PLE or Page Life Expectancy is the number of seconds a page will stay in the buffer pool without references.  
In simple words, if your page stays longer in the buffer pool (area of the memory cache) your PLE is higher, leading to higher performance as every time request comes there are chances it may find its data in the cache itself instead of going to the hard drive to read the data. 
 
Minimum recommneded value for stable performance: 300 seconds 
I have seen busy system with value as low as even 45 seconds and on unused system as high as 1250 seconds. 
It is highly recommended to increase SQL dedicated RAM if the value gets below 300. 
 
You can find the value of the PLE by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] LIKE '%Manager%' AND [counter_name] = 'Page life expectancy' 
 
3. SQLServer: SQL Statistics: Batch Requests/Sec 
Batch Requests/Sec measures the number of batches SQL Server is receiving per second. 
This counter is a good indicator of how much activity is being processed by your SQL Server box.  
The higher the number, the more queries are being executed on your box.  
 
Like many counters, there is no single number that can be used universally to indicate your machine is too busy, 
Although take into consideration that 1000 requests per second represent a busy system. 
You should review this counter over time to determine a baseline number for your environment. 
This performance counter become more meaningfull if paired with other counters like SQL Compilation/Sec. 
 
You can find the value of the Batch Requests/Sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:SQL Statistics' AND [counter_name] = 'Batch Requests/sec' 
 
4. SQLServer: SQL Statistics: SQL Compilations/Sec 
The SQL Compilations/Sec measure the number of times SQL Server compiles an execution plan per second. 
Compiling an execution plan is a resource-intensive operation.  
 
Compilations/Sec should be compared with the number of Batch Requests/Sec to get an indication of whether or not complications might be hurting your performance.  
To do that do this math division: 
Batch Request/Sec : SQL Compilations/Sec = Ratio of Batch Compile. 
Recommended value for Ratio of Batch Compile is 1 compile every 10 batch request. 
 
You can find the value of the Ratio of Batch Compile by running the following query: 
SELECT(SELECT [cntr_value] FROM sys.dm_os_performance_counters  
WHERE [object_name] = 'SQLServer:SQL Statistics' AND [counter_name] = 'Batch Requests/sec') 
/(SELECT [cntr_value] FROM sys.dm_os_performance_counters WHERE [object_name] = 'SQLServer:SQL Statistics' AND [counter_name] = 'SQL Compilations/sec') as "Ratio of Batch Compile" 
 
You can find the value of the SQL Compilations/Sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:SQL Statistics' AND [counter_name] = 'SQL Compilations/sec' 
 
5. SQLServer: SQL Statistics: SQL Re-Compilations/Sec 
When the execution plan is invalidated due to some significant event, SQL Server will re-compile it.  
The Re-compilations/Sec counter measures the number of time a re-compile event was triggered per second.  
 
Re-compiles, like compiles, are expensive operations so you want to minimize the number of re-compiles. Ideally you want to keep this counter less than 10% of the number of SQL Compilations/Sec. 
Major impact on Copile and Re-Compile events comes from lack of memory. 
 
You can find the value of the SQL Re-Compilations/Sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:SQL Statistics' AND [counter_name] = 'SQL Re-Compilations/sec' 
 
6. SQLServer: Locks: Lock Waits / Sec: _Total 
In order for SQL Server to manage concurrent users on the system, SQL Server needs to lock resources from time to time. The lock waits per second counter tracks the number of times per second that SQL Server is not able to retain a lock right away for a resource.  
 
Ideally you don't want any request to wait for a lock.  
Therefore you want to keep this counter at zero, or close to zero at all times. 
Major impact on Lock Waits comes from storage response and CPU power. 
 
You can find the value of the  Lock Waits / Sec: _Total by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] ='SQLServer:Locks' and [instance_name]='_Total' and [counter_name] = 'Lock Waits/sec' 
 
7. SQLServer: Access Methods: Page Splits / Sec 
This counter measures the number of times SQL Server had to split a page when updating or inserting data per second.  
 
Page splits are expensive, and cause your table to perform more poorly due to fragmentation.  
Therefore, the fewer page splits you have the better your system will perform.  
Ideally this counter should be less than 20% of the batch requests per second. 
 
You can find the value of the Page Splits / Sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name]='SQLServer:Access Methods' and [counter_name] =  'Page Splits/sec' 
 
8. SQLServer: General Statistic: Processes Block 
The processes blocked counter identifies the number of blocked processes. When one process is blocking another process, the blocked process cannot move forward with its execution plan until the resource that is causing it to wait is freed up.  
 
Ideally you don't want to see any blocked processes. When processes are being blocked you should investigate. 
 
You can find the value of the Processes Block by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:General Statistics'  and [counter_name] = 'Processes blocked' 
 
9. SQLServer:Buffer Manager:Free List Stalls/sec 
This counter tracks when SQL Server can't get data from the buffer because the buffer is out of space. If this value is greater than 2, the system would probably benefit from additional memory. 
 
You can find the value of the Processes Block by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:Buffer Manager' and [counter_name] = 'Free List Stalls/sec' 
 
10. SQLServer:Access Methods:Full Scans/sec 
This counter shows how many full table scans are being performed.  
 
This value should be low—less than 1. If it's higher, you probably need to add indexes. 
 
You can find the value of the Full Scans/sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:Access Methods' and [counter_name] = 'Full Scans/sec' 
 
12. SQLServer:Access Methods:Page Splits/sec 
This counter tracks when a new row is added to a full index page, which causes a page split.  
 
This value should be low—less than 25. If it's higher, consider increasing the fill factor on your indexes or rebuilding your indexes more frequently. 
 
You can find the value of the Page Splits/sec by running the following query: 
SELECT [object_name],[counter_name],[cntr_value]  
FROM sys.dm_os_performance_counters 
WHERE [object_name] = 'SQLServer:Access Methods' and [counter_name] = 'Page Splits/sec' 
 
13. Windows Server Logical Disk: % Idle Time 
This is a Windows OS specific counter.  
This counter tracks how much time the disk is in an idle state, which means all the requests from the OS to the disk have been completed.  
 
If this value is less than 50 percent, the disk might be experiencing high I/O, which means you might want to move some files off this drive. 
 
You can find the value of the % Disk Idle using Windows Performance Monitor counters. 
 
14. Windows Server Logical Disk: Avg. Disk Sec/Read & Logical Disk: Avg. Disk Sec/Write 
This is a Windows OS specific counter.  
These two counters display the average time the disk transfers took to complete. A value of more than 15ms might mean that you have a disk I/O performance problem. 
 
You can find the value of the Logical Disk: Avg. Disk Sec/Read & Logical Disk: Avg. Disk Sec/Write 
 using Windows Performance Monitor counters. 
 
15. SQLServer:Memory Manager - Memory Grants Pending:  
If you see memory grants pending in buffer pool your server is facing SQL Server memory crunch and increasing memory would be a good idea.  
 
For memory grants please read this article: http://blogs.msdn.com/b/sqlqueryprocessing/archive/2010/02/16/understanding-sql-server-memory-grant.aspx  
 
16. SQLServer:memory Manager -Target Server Memory:  
This is amount of memory SQL Server is trying to acquire.  
 
17. SQLServer:memory Manager -Total Server memory: 
This is current memory SQL Server has acquired.  
 
References  
-->  https://blogs.msdn.microsoft.com/sqlcat/2006/06/23/oltp-blueprint-a-performance-profile-of-oltp-applications/ 
  

