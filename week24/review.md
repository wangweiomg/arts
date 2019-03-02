## ARTS - Review 补12.17
## 1.4 What Is New in MySQL 8.0

* summarizes v 总结
* deprecated v 弃用
* companion n 伴侣，伙伴
* incorporates v 合并
* associated adj. 相关
* enhancements 增强
* accompanying adj. 随同
* latency 等待时间
* restrictions 限制


This section summarizes what has been added to, deprecated in, and removed from MySQL 8.0. A companion section lists MySQL server options and variables that have been added, deprecated, or removed in MySQL 8.0. See Section 1.5, “Server and Status Variables and Options Added, Deprecated, or Removed in MySQL 8.0”.

这一章总结了MySQL8.0 添加的和弃用的，和删除的。附属部分列出了MySQL服务器被增加、弃用或删除的选项和变量。


### Features Added in MySQL 8.0
The following features have been added to MySQL 8.0:

**Data dictionary**.  MySQL now incorporates a transactional data dictionary that stores information about database objects. In previous MySQL releases, dictionary data was stored in metadata files and nontransactional tables. For more information, see Chapter 14, MySQL Data Dictionary.

**Atomic Data Definition Statements (Atomic DDL)**.  An atomic DDL statement combines the data dictionary updates, storage engine operations, and binary log writes associated with a DDL operation into a single, atomic transaction. For more information, see Section 13.1.1, “Atomic Data Definition Statement Support”.

**Security and account management.**  These enhancements were added to improve security and enable greater DBA flexibility in account management:

以下特性被添加到MySQL8.0

数据字典。MySQL现在合并一个事务型的数据字典来存储数据库对象信息。在之前的MySQL发布版里，字典数据是存储在没有事务的表和元数据文件中。

原子数据定义语句 (Atomic DDL). 一个原子DDL语句将与DDL操作相关的数据字典更新，存储引擎操作，和二进制日志写入组合到单个原子事务中。

安全和账号管理。这些增强被添加到提升安全和在账户管理中实现更大灵活性：

* The grant tables in the mysql system database are now InnoDB (transactional) tables. Previously, these were MyISAM (nontransactional) tables. The change of grant table storage engine underlies an accompanying change to the behavior of account-management statements. Previously, an account-management statement (such as CREATE USER or DROP USER) that named multiple users could succeed for some users and fail for others. Now, each statement is transactional and either succeeds for all named users or rolls back and has no effect if any error occurs. The statement is written to the binary log if it succeeds, but not if it fails; in that case, rollback occurs and no changes are made. For more information, see Section 13.1.1, “Atomic Data Definition Statement Support”.

授权表在MySQL系统数据库是InnoDB(事务型)表。之前是MyISAM(无事务)表。授权表底层存储引擎的改变让账户管理语句也随着变。。之前，一个账户管理语句(例如  CREATE USER or DROP USer)明明多个用户可能对某些用户成功，对另外用户失败。现在，每个语句是事务的，对所有用户要么全成功，要么全回滚，不再出错。语句是被泄劲二进制文件如果成功的话，失败的话就不会。那样的话，回滚发生时不会有任何改变。

* A new caching_sha2_password authentication plugin is available. Like the sha256_password plugin, caching_sha2_password implements SHA-256 password hashing, but uses caching to address latency issues at connect time. It also supports more connection protocols and does not require linking against OpenSSL for RSA key pair-based password-exchange capabilities. See Section 6.5.1.3, “Caching SHA-2 Pluggable Authentication”.

一个新的 caching_sha2_password 授权插件被应用。像sha256_password 插件， caching_sha2_password 实现了 SHA-256密码三列，但是在连接时间使用缓存来定位延迟问题时间。它也支持更多链接协议，不要求链接openSSL RAS key 基于对的密码交换的容量。

* The caching_sha2_password and sha256_password authentication plugins provide more secure password encryption than the mysql_native_password plugin, and caching_sha2_password provides better performance than sha256_password. Due to these superior security and performance characteristics of caching_sha2_password, it is now the preferred authentication plugin, and is also the default authentication plugin rather than mysql_native_password. For information about the implications of this change of default plugin for server operation and compatibility of the server with clients and connectors, see caching_sha2_password as the Preferred Authentication Plugin.

caching_sha2_password 和 sha256_password 授权插件提供比mysql_native_password插件更多安全密码加密，caching_sha2_password提供比mysql_native_password更好的性能。由于caching_sha2_password优越的安全和性能特点，它限制是更首选的身份验证插件，也是默认授权插件，而不是mysql_native_password.


* MySQL now supports roles, which are named collections of privileges. Roles can be created and dropped. Roles can have privileges granted to and revoked from them. Roles can be granted to and revoked from user accounts. The active applicable roles for an account can be selected from among those granted to the account, and can be changed during sessions for that account. For more information, see Section 6.3.4, “Using Roles”.

MySQL现在还支持角色，可以批量分配权限。角色可以创建和删除。橘色可以自己获得授权，移除权限。可以被用户账户授权、移除授权。活跃的应用橘色对一个账户来说是可选的，在账号会话期间可以改变。

* MySQL now maintains information about password history, enabling restrictions on reuse of previous passwords. DBAs can require that new passwords not be selected from previous passwords for some number of password changes or period of time. It is possible to establish password-reuse policy globally as well as on a per-account basis.

MySQL维护密码历史，能够管制使用之前的密码。DBA们可以要求新密码不被老密码选中正在几次或一段时间内。可以在全局和每个账户上建立密码重用策略。

* It is now possible to require that attempts to change account passwords be verified by specifying the current password to be replaced. This enables DBAs to prevent users from changing password without proving that they know the current password. It is possible to establish password-verification policy globally as well as on a per-account basis.

现在可以要求通过指定要替换当前密码来验证更改账户密码尝试。这使得DBA能够在不证明用户知道密码的情况下，组织用户修改密码。可以在全局和每个账户上建立密码验证策略。


* Accounts are now permitted to have dual passwords, which enables phased password changes to be performed seamlessly in complex multiple-server systems, without downtime.

* These new capabilities provide DBAs more complete control over password management. For more information, see Section 6.3.8, “Password Management”.

* MySQL now supports FIPS mode, if compiled using OpenSSL, and an OpenSSL library and FIPS Object Module are available at runtime. FIPS mode imposes conditions on cryptographic operations such as restrictions on acceptable encryption algorithms or requirements for longer key lengths. See Section 6.6, “FIPS Support”.

* MySQL now sets the access control granted to clients on the named pipe to the minimum necessary for successful communication on Windows. Newer MySQL client software can open named pipe connections without any additional configuration. If older client software cannot be upgraded immediately, the new named_pipe_full_access_group server system variable can be used to give a Windows group the necessary permissions to open a named pipe connection. Membership in the full-access group should be restricted and temporary.


现在允许帐户具有双重密码，这使得在复杂的多服务器系统中可以无缝地执行阶段性密码更改，而无需停机。

这些新功能使DBA能够更全面地控制密码管理。有关更多信息，请参见第6.3.8节“密码管理”。


如果使用openssl编译，mysql现在支持fips模式，并且openssl库和fips对象模块在运行时可用。FIPS模式对加密操作施加条件，例如对可接受的加密算法的限制或对较长密钥长度的要求。参见第6.6节“FIPS支持”。


MySQL现在将在命名管道上授予客户端的访问控制设置为在Windows上成功通信所必需的最小值。较新的MySQL客户端软件可以打开命名管道连接，而无需任何附加配置。如果无法立即升级旧的客户端软件，则可以使用新的命名管道完整访问组服务器系统变量授予Windows组打开命名管道连接所需的权限。完全访问组中的成员资格应受到限制并是临时的。



