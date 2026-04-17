# <center> Consistency in System Design</center>

Consistency in system design refers all nodes or clients see the same state of data, even in the presence of concurrent operations, replication, and network delays. 
* All users and system nodes read the same data value after an update.
* Prevents conflicts or mismatched data across different parts of the system.

<p>This distribution brings scalability and resilience, but it also introduces a fascinating challenge: how do we ensure that all users, and all parts of the system, see a coherent and correct view of the data, especially when things are changing rapidly?</p>

* On a single machine, consistency is simple. There is one copy of the data, and every read sees the most recent write. But the moment you replicate data across multiple machines, everything changes. 

**In an online banking system, if money is transferred from one account to another, all servers should immediately show the updated balance so that users always see the correct information.**
