## ARTS - Review 补12.24
##  1.4 What Is New in MySQL 8.0(下)

* assigning 分配
* enhancements 增强
* persistent adj 持久化
* specific 具体的
* subsequent 后来的
* encountering 遭遇
* corruption 腐败
* checkpoint 检查点

*	**Resource management.**

MySQL now supports creation and management of resource groups, and permits assigning threads running within the server to particular groups so that threads execute according to the resources available to the group. Group attributes enable control over its resources, to enable or restrict resource consumption by threads in the group. DBAs can modify these attributes as appropriate for different workloads. Currently, CPU time is a manageable resource, represented by the concept of “virtual CPU” as a term that includes CPU cores, hyperthreads, hardware threads, and so forth. The server determines at startup how many virtual CPUs are available, and database administrators with appropriate privileges can associate these CPUs with resource groups and assign threads to groups. For more information, see Section 8.12.5, “Resource Groups”.

资源管理。

MySQL现在支持资源组的创建和管理，并允许将服务器内运行的线程分配给特定组，以便线程根据组可用的资源执行。组属性可以控制其资源，以启用或限制组中线程的资源消耗。 DBA可以根据不同的工作负载修改这些属性。 目前，CPU时间是可管理的资源，由“虚拟CPU”的概念表示为包括CPU核心，超线程，硬件线程等的术语。 服务器在启动时确定可用的虚拟CPU数量，具有适当权限的数据库管理员可以将这些CPU与资源组关联，并将线程分配给组。 有关更多信息，请参见第8.12.5节“资源组”。

InnoDb增强
These InnoDB enhancements were added:

The current maximum auto-increment counter value is written to the redo log each time the value changes, and saved to an engine-private system table on each checkpoint. These changes make the current maximum auto-increment counter value persistent across server restarts. Additionally:
当前最大自动增长计数器值每次更改后被写入重做日志，保存在引擎私有的系统表。这些改变是的当前最大自动增长计数器在服务器重启时会保持不变。

* A server restart no longer cancels the effect of the AUTO_INCREMENT = N table option. If you initialize the auto-increment counter to a specific value, or if you alter the auto-increment counter value to a larger value, the new value is persisted across server restarts.

一个服务器重启不再取消AUTO_INCREMENT = N 影响这个的表选项。如果你初始化胡自动增长计数器为一个具体的值，或者如果你修改 自动增长计数器的值为一个更大的值，新的值就会随着服务器重启持久化。

* A server restart immediately following a ROLLBACK operation no longer results in the reuse of auto-increment values that were allocated to the rolled-back transaction.

一个服务器突然重启随着一个回滚操作不再使用被分配给回滚事务的自动增长的值。

* If you modify an AUTO_INCREMENT column value to a value larger than the current maximum auto-increment value (in an UPDATE operation, for example), the new value is persisted, and subsequent INSERT operations allocate auto-increment values starting from the new, larger value.

如果你修改了 AUTO_INCREMENT 的一个列的值比当前最大自动增长值大(例如，在一个UPDATE操作里)，新的值就会持久化，后来的 INSERT 操作分配自动增长值开始是新的，大的值。

* For more information, see Section 15.6.1.4, “AUTO_INCREMENT Handling in InnoDB”, and InnoDB AUTO_INCREMENT Counter Initialization.

更多信息看 15.6.1.4章节。


When encountering index tree corruption, InnoDB writes a corruption flag to the redo log, which makes the corruption flag crash safe. InnoDB also writes in-memory corruption flag data to an engine-private system table on each checkpoint. During recovery, InnoDB reads corruption flags from both locations and merges results before marking in-memory table and index objects as corrupt.

当遭遇索引树腐败，InnoDB 写一个腐败的标签给重做日志，会让腐败标签危害安全。InnoDB 也写在内存腐败标签数据到一个引擎私有的系统表在每次检查。回复期间InnoDB 读腐败的标志从地址，和合并结果在标记内存表索引对象腐败之前。



