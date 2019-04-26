# ARTS - Review 补2019.1.23
# [1.5 MySQL8.0服务状态变量和可选性的增加、过时或删除](https://dev.mysql.com/doc/refman/8.0/en/added-deprecated-removed.html)

* rotated v. 回转
*  activate vt. 激活

### Options and Variables Introduced in MySQL 8.0

he following system variables, status variables, and options are new in MySQL 8.0, and have not been included in any previous release series.
以下系统变量，状态变量，和可选项是MySQL8.0新增的，在之前的的发布系列中是不包含的。
 
*  Acl_cache_items_count: Number of cached privilege objects. Added in MySQL 8.0.0.Each object is the privilege combination of a user and its active roles.
*  缓存的权限对象数量。MySQL8.0.0添加。 每个对象都是连接一个用户和它的角色的权限。
* Audit_log_current_size: Audit log file current size. Added in MySQL 8.0.11.The value increases when an event is written to the log and is reset to 0 when the log is rotated.
*  审批日志文件当前的大小。MySQL8.0.11添加的。当一个写入日志事件发生值会增加，当日志回转会重置为0
* 
*  Audit_log_event_max_drop_size: Size of largest dropped audited event. Added in MySQL 8.0.11.The size of the largest dropped event in performance logging mode
*  审批时间最大删除大小。MySQL8.0.11添加。在日志记录性能模式下的最大删除大小。
* Audit_log_events: Number of handled audited events. Added in MySQL 8.0.11. The number of events handled by the audit log plugin, whether or not they were written to the log based on filtering policy (see Section 6.4.5.5, “Audit Log Logging Configuration”).
*  处理审批时间的数量。MySQL8.0.11添加。通过审批日志插件处理时间数量，不论是否写入日志，基于过滤机制。 
* Audit_log_events_filtered: Number of filtered audited events. Added in MySQL 8.0.11.The number of events handled by the audit log plugin that were filtered (not written to the log) based on filtering policy
*  过滤审批文时间数量。MySQL8.0.11添加。审批日志插件处理的过滤时间的数量。
* Audit_log_events_lost: Number of dropped audited events. Added in MySQL 8.0.11.
*  丢掉的审批事件数量。
* Audit_log_events_written: Number of written audited events. Added in MySQL 8.0.11.
*  写入的审批时间数量。
* Audit_log_total_size: Combined size of written audited events. Added in MySQL 8.0.11.
*  写入的审批时间数量汇总。
* Audit_log_write_waits: Number of write-delayed audited events. Added in MySQL 8.0.11.
*  延迟写入的审批事件数量。
* Caching_sha2_password_rsa_public_key: caching_sha2_password authentication plugin RSA public key value. Added in MySQL 8.0.4.
*  加密插件RAS 公钥值。
* Com_alter_resource_group: Count of ALTER RESOURCE GROUP statements. Added in MySQL 8.0.3.
*  ALTER RESOURCE GROUP 语句数量。
* Com_alter_user_default_role: Count of ALTER USER ... DEFAULT ROLE statements. Added in MySQL 8.0.0.
*  ALTER USER .. DEFAULT ROLE 语句数量。
* Com_create_resource_group: Count of CREATE RESOURCE GROUP statements. Added in MySQL 8.0.3.
*  CREATE RESOURCE GROUP 语句数量。
* Com_create_role: Count of CREATE ROLE statements. Added in MySQL 8.0.0.
*  CREATE ROLE 语句数量。
* Com_drop_resource_group: Count of DROP RESOURCE GROUP statements. Added in MySQL 8.0.3.
*  DROP RESOURCE GROUP 语句数量。
* Com_drop_role: Count of DROP ROLE statements. Added in MySQL 8.0.0.
*  DROP ROLE 语句的数量。
* Com_grant_roles: Count of GRANT ROLE statements. Added in MySQL 8.0.0.
*  GRANT ROLE 语句的数量。
* Com_install_component: Count of INSTALL COMPONENT statements. Added in MySQL 8.0.0.
*  INSTALL COMPONENT 语句数量。
* Com_revoke_roles: Count of REVOKE ROLES statements. Added in MySQL 8.0.0.
*  REVOKE ROLE是语句数量。
* Com_set_resource_group: Count of SET RESOURCE GROUP statements. Added in MySQL 8.0.3.
*  SET RESOURCE GROUP 语句数量。
* Com_set_role: Count of SET ROLE statements. Added in MySQL 8.0.0.
* Com_uninstall_component: Count of UINSTALL COMPONENT statements. Added in MySQL 8.0.0.
* Connection_control_delay_generated: How many times the server delayed a connection request. Added in MySQL 8.0.1.
*  一次连接服务器延迟的次数。
* Current_tls_ca: Current value of ssl_ca system variable. Added in MySQL 8.0.16.
*  ssl_ca 系统变量当前值
* Current_tls_capath: Current value of ssl_capath system variable. Added in MySQL 8.0.16.
*  系统变量 ssl_capath 当前值。
* Current_tls_cert: Current value of ssl_cert system variable. Added in MySQL 8.0.16.
*  系统变量 ssl_cert 当前值。
* Current_tls_cipher: Current value of ssl_cipher system variable. Added in MySQL 8.0.16.
* Current_tls_ciphersuites: Current value of tsl_ciphersuites system variable. Added in MySQL 8.0.16.
* Current_tls_crl: Current value of ssl_crl system variable. Added in MySQL 8.0.16.
* Current_tls_crlpath: Current value of ssl_crlpath system variable. Added in MySQL 8.0.16.
* Current_tls_key: Current value of ssl_key system variable. Added in MySQL 8.0.16.
* Current_tls_version: Current value of tls_version system variable. Added in MySQL 8.0.16.
* Firewall_access_denied: Number of statements rejected by MySQL Enterprise Firewall. Added in MySQL 8.0.11.
*  MySQL企业防火墙拒绝的语句数量
* Firewall_access_granted: Number of statements accepted by MySQL Enterprise Firewall. Added in MySQL 8.0.11.
*  MySQL企业级防火墙被通过的语句数量。
* Firewall_cached_entries: Number of statements recorded by MySQL Enterprise Firewall. Added in MySQL 8.0.11.
*  MySQL企业防火墙记录的语句数量。
* Secondary_engine_execution_count: For future use. Added in MySQL 8.0.13.
*  未来使用。
* activate_all_roles_on_login: Whether to activate all user roles at connect time. Added in MySQL 8.0.2.
*  连接时激活所有用户角色。
* admin_address: IP address to bind to for connections on administrative interface. Added in MySQL 8.0.14.
*  管理接口上的连接要绑定到的Ip地址。
* admin_port: TCP/IP number to use for connections on administrative interface. Added in MySQL 8.0.14.
*  管理接口的连接使用TCP/IP的数量。
* audit-log: Whether to activate the audit log plugin. Added in MySQL 8.0.11.
*  是否激活审批日志插件。
* audit_log_buffer_size: The size of the audit log buffer. Added in MySQL 8.0.11.
*  审批日志缓冲大小。
* audit_log_compression: Audit log file compression method. Added in MySQL 8.0.11.
*  审批日志文件压缩方法。
* audit_log_connection_policy: Audit logging policy for connection-related events. Added in MySQL 8.0.11.
*  审批日志连接事件策略。
* audit_log_current_session: Whether to audit current session. Added in MySQL 8.0.11.
*  是否审批当前会话。
* audit_log_encryption: Audit log file encryption method. Added in MySQL 8.0.11. 审批日志文件加密方法
* audit_log_exclude_accounts: Accounts not to audit. Added in MySQL 8.0.11.
* 不审批的账户。
* audit_log_file: The name of the audit log file. Added in MySQL 8.0.11.
* 审批文件名称。
* audit_log_filter_id: ID of current audit log filter. Added in MySQL 8.0.11.
* 当前审批日志过利器ID。
* audit_log_flush: Close and reopen the audit log file. Added in MySQL 8.0.11.
* 关闭和重新打开审批日志文件。
* audit_log_format: The audit log file format. Added in MySQL 8.0.11.
* 审批文件日志格式。
* audit_log_include_accounts: Accounts to audit. Added in MySQL 8.0.11.
* 审批账户。
* audit_log_policy: Audit logging policy. Added in MySQL 8.0.11.
* audit_log_read_buffer_size: Audit log file read buffer size. Added in MySQL 8.0.11.
* audit_log_rotate_on_size: Close and reopen the audit log file at a certain size. Added in MySQL 8.0.11.
* audit_log_statement_policy: Audit logging policy for statement-related events. Added in MySQL 8.0.11.
* audit_log_strategy: The audit logging strategy. Added in MySQL 8.0.11.
* authentication_ldap_sasl_auth_method_name: Authentication method name. Added in MySQL 8.0.11.
* authentication_ldap_sasl_bind_base_dn: LDAP server base distinguished name. Added in MySQL 8.0.11.
* authentication_ldap_sasl_bind_root_dn: LDAP server root distinguished name. Added in MySQL 8.0.11.
* authentication_ldap_sasl_bind_root_pwd: LDAP server root bind password. Added in MySQL 8.0.11.
* authentication_ldap_sasl_ca_path: LDAP server certificate authority file name. Added in MySQL 8.0.11.
* authentication_ldap_sasl_group_search_attr: LDAP server group search attribute. Added in MySQL 8.0.11.
* authentication_ldap_sasl_group_search_filter: LDAP custom group search filter. Added in MySQL 8.0.11.
* authentication_ldap_sasl_init_pool_size: LDAP server initial connection pool size. Added in MySQL 8.0.11.
* authentication_ldap_sasl_log_status: LDAP server log level. Added in MySQL 8.0.11.
* authentication_ldap_sasl_max_pool_size: LDAP server maximum connection pool size. Added in MySQL 8.0.11.
* authentication_ldap_sasl_server_host: LDAP server host name or IP address. Added in MySQL 8.0.11.
* authentication_ldap_sasl_server_port: LDAP server port number. Added in MySQL 8.0.11.
* authentication_ldap_sasl_tls: Whether to use encrypted connections to LDAP server. Added in MySQL 8.0.11.
* authentication_ldap_sasl_user_search_attr: LDAP server user search attribute. Added in MySQL 8.0.11.
* authentication_ldap_simple_auth_method_name: Authentication method name. Added in MySQL 8.0.11.
* authentication_ldap_simple_bind_base_dn: LDAP server base distinguished name. Added in MySQL 8.0.11.
* authentication_ldap_simple_bind_root_dn: LDAP server root distinguished name. Added in MySQL 8.0.11.
* authentication_ldap_simple_bind_root_pwd: LDAP server root bind password. Added in MySQL 8.0.11.
* authentication_ldap_simple_ca_path: LDAP server certificate authority file name. Added in MySQL 8.0.11.
* authentication_ldap_simple_group_search_attr: LDAP server group search attribute. Added in MySQL 8.0.11.
* authentication_ldap_simple_group_search_filter: LDAP custom group search filter. Added in MySQL 8.0.11.
* authentication_ldap_simple_init_pool_size: LDAP server initial connection pool size. Added in MySQL 8.0.11.
* authentication_ldap_simple_log_status: LDAP server log level. Added in MySQL 8.0.11.
* authentication_ldap_simple_max_pool_size: LDAP server maximum connection pool size. Added in MySQL 8.0.11.
* authentication_ldap_simple_server_host: LDAP server host name or IP address. Added in MySQL 8.0.11.
* authentication_ldap_simple_server_port: LDAP server port number. Added in MySQL 8.0.11.
* authentication_ldap_simple_tls: Whether to use encrypted connections to LDAP server. Added in MySQL 8.0.11.
* authentication_ldap_simple_user_search_attr: LDAP server user search attribute. Added in MySQL 8.0.11.
* authentication_windows_log_level: Windows authentication plugin logging level. Added in MySQL 8.0.11.
* authentication_windows_use_principal_name: Whether to use Windows authentication plugin principal name. Added in MySQL 8.0.11.
* binlog_encryption: Enable encryption for binary log files and relay log files on this server. Added in MySQL 8.0.14.
* binlog_expire_logs_seconds: Purge binary logs after this many seconds. Added in MySQL 8.0.1.
* binlog_rotate_encryption_master_key_at_startup: Rotate the binary log master key at server startup. Added in MySQL 8.0.14.
* binlog_row_event_max_size: Binary log max event size. Added in MySQL 8.0.14.
* binlog_row_metadata: Configures the amount of table related metadata binary logged when using row-based logging.. Added in MySQL 8.0.1.
* binlog_row_value_options: Enables binary logging of partial JSON updates for row-based replication.. Added in MySQL 8.0.3.
* binlog_transaction_dependency_history_size: Number of row hashes kept for looking up transaction that last updated some row.. Added in MySQL 8.0.1.
* binlog_transaction_dependency_tracking: Source of dependency information (commit timestamps or transaction write sets) from which to assess which transactions can be executed in parallel by slave's multithreaded applier.. Added in MySQL 8.0.1.
* caching_sha2_password_auto_generate_rsa_keys: Whether to autogenerate RSA key-pair files. Added in MySQL 8.0.4.
* caching_sha2_password_private_key_path: SHA2 authentication plugin private key path name. Added in MySQL 8.0.3.
* caching_sha2_password_public_key_path: SHA2 authentication plugin public key path name. Added in MySQL 8.0.3.
* connection_control_failed_connections_threshold: Consecutive failed connection attempts before delays occur. Added in MySQL 8.0.1.
* connection_control_max_connection_delay: Maximum delay (milliseconds) for server response to failed connection attempts. Added in MySQL 8.0.1.
* connection_control_min_connection_delay: Minimum delay (milliseconds) for server response to failed connection attempts. Added in MySQL 8.0.1.
* create_admin_listener_thread: Whether to use dedicated listening thread for connections on administrative interface. Added in MySQL 8.0.14.
* cte_max_recursion_depth: Common table expression maximum recursion depth. Added in MySQL 8.0.3.
* ddl-rewriter: Whether to activate the ddl_rewriter plugin. Added in MySQL 8.0.16.
* default_collation_for_utf8mb4: Default collation for utf8mb4 character set. Added in MySQL 8.0.11.
* default_table_encryption: The default schema and tablespace encryption setting. Added in MySQL 8.0.16.
* dragnet.Status: Result of most recent assignment to dragnet.log_error_filter_rules. Added in MySQL 8.0.12.
* dragnet.log_error_filter_rules: Filter rules for error logging. Added in MySQL 8.0.4.
* early-plugin-load: Specify plugins to load before loading mandatory built-in plugins and before storage engine initialization. Added in MySQL 8.0.0.
* group_replication_autorejoin_tries: Number of tries that a member makes to automatically rejoin the group. Added in MySQL 8.0.16.
* group_replication_communication_debug_options: The level of debugging messages for Group Replication components.. Added in MySQL 8.0.3.
* group_replication_communication_max_message_size: Maximum message size for Group Replication communications, larger messages are fragmented. Added in MySQL 8.0.16.
* group_replication_consistency: The type of transaction consistency guarantee which the group provides. Added in MySQL 8.0.14.
* group_replication_exit_state_action: How the instance behaves when it leaves the group involuntarily. Added in MySQL 8.0.12.
* group_replication_flow_control_hold_percent: Defines what percentage of the group quota remains unused. Added in MySQL 8.0.2.
* group_replication_flow_control_max_commit_quota: Defines the maximum flow control quota of the group. Added in MySQL 8.0.2.
* group_replication_flow_control_member_quota_percent: Defines the percentage of the quota that a member should assume is available for itself when calculating the quotas. Added in MySQL 8.0.2.
* group_replication_flow_control_min_quota: Controls the lowest flow control quota that can be assigned to a member. Added in MySQL 8.0.2.
* group_replication_flow_control_min_recovery_quota: Controls the lowest quota that can be assigned to a member because of another recovering member in the group. Added in MySQL 8.0.2.
* group_replication_flow_control_period: Defines how many seconds to wait between flow control iterations. Added in MySQL 8.0.2.
* group_replication_flow_control_release_percent: Defines how the group quota should be released when flow control no longer needs to throttle the writer members. Added in MySQL 8.0.2.
* group_replication_member_expel_timeout: The time between a suspected failure of a group member and it being expelled from the group, causing a group membership reconfiguration.. Added in MySQL 8.0.13.
* group_replication_member_weight: Chance of this member being elected as primary. Added in MySQL 8.0.2.
* group_replication_message_cache_size: Maximum memory for the message cache in the group communication engine (XCom). Added in MySQL 8.0.16.
* group_replication_recovery_get_public_key: Whether to accept preference about fetching public key from master. Added in MySQL 8.0.4.
* group_replication_recovery_public_key_path: To accept public key information. Added in MySQL 8.0.4.
* group_replication_unreachable_majority_timeout: How long to wait for network partitions that result in a minority to leave the group. Added in MySQL 8.0.2.
* histogram_generation_max_mem_size: Maximum memory for creating histogram statistics. Added in MySQL 8.0.2.
* immediate_server_version: The MySQL Server release number of the server that is the immediate master in a replication topology. Added in MySQL 8.0.14.
* information_schema_stats_expiry: Expiration setting for cached table statistics. Added in MySQL 8.0.3.
* innodb_buffer_pool_debug: Permits multiple buffer pool instances when the buffer pool is less than 1GB in size. Added in MySQL 8.0.0.
* innodb_buffer_pool_in_core_file: Controls writing of buffer pool pages to core files. Added in MySQL 8.0.14.
* innodb_checkpoint_disabled: Disables checkpoints so that a deliberate server exit always initiates recovery. Added in MySQL 8.0.2.
* innodb_ddl_log_crash_reset_debug: A debug option that resets DDL log crash injection counters. Added in MySQL 8.0.3.
* innodb_deadlock_detect: Enables or disables deadlock detection. Added in MySQL 8.0.0.
* innodb_dedicated_server: Enables automatic configuration of buffer pool size, log file size, and flush method. Added in MySQL 8.0.3.
* innodb_directories: Defines directories to scan at startup for tablespace data files. Added in MySQL 8.0.4.
* innodb_fsync_threshold: Controls how often InnoDB calls fsync when creating a new file. Added in MySQL 8.0.13.
* innodb_log_checkpoint_fuzzy_now: A debug option that forces InnoDB to write a fuzzy checkpoint. Added in MySQL 8.0.13.
* innodb_log_spin_cpu_abs_lwm: Minimum amount of CPU usage below which user threads no longer spin while waiting for flushed redo. Added in MySQL 8.0.11.
* innodb_log_spin_cpu_pct_hwm: Maximum amount of CPU usage above which user threads no longer spin while waiting for flushed redo. Added in MySQL 8.0.11.
* innodb_log_wait_for_flush_spin_hwm: The maximum average log flush time beyond which user threads no longer spin while waiting for flushed redo. Added in MySQL 8.0.11.
* innodb_parallel_read_threads: Defines the number of threads for parallel index reads. Added in MySQL 8.0.14.
* innodb_print_ddl_logs: Whether or not to print DDL logs to the error log. Added in MySQL 8.0.3.
* innodb_redo_log_encrypt: Controls encryption of redo log data for encrypted tablespaces. Added in MySQL 8.0.1.
* innodb_scan_directories: Defines directories to scan for tablespace files during InnoDB recovery. Added in MySQL 8.0.2.
* innodb_spin_wait_pause_multiplier: Defines a multiplier value used to determine the number of PAUSE instructions in spin-wait loops.. Added in MySQL 8.0.16.
* innodb_stats_include_delete_marked: Include delete-marked records when calculating persistent InnoDB statistics. Added in MySQL 8.0.1.
* innodb_temp_tablespaces_dir: Session temporary tablespaces path. Added in MySQL 8.0.13.
* innodb_tmpdir: The directory location for the temporary table files created during online ALTER TABLE operations. Added in MySQL 8.0.0.
* innodb_undo_log_encrypt: Controls encryption of undo log data for encrypted tablespaces. Added in MySQL 8.0.1.
* internal_tmp_mem_storage_engine: Defines the storage to use for internal in-memory temporary tables. Added in MySQL 8.0.2.
* keyring-migration-destination: Key migration destination keyring plugin. Added in MySQL 8.0.4.
* keyring-migration-host: Host name for connecting to running server for key migration. Added in MySQL 8.0.4.
* keyring-migration-password: Password for connecting to running server for key migration. Added in MySQL 8.0.4.
* keyring-migration-port: TCP/IP port number for connecting to running server for key migration. Added in MySQL 8.0.4.
* keyring-migration-socket: Unix socket file or Windows named pipe for connecting to running server for key migration. Added in MySQL 8.0.4.
* keyring-migration-source: Key migration source keyring plugin. Added in MySQL 8.0.4.
* keyring-migration-user: User name for connecting to running server for key migration. Added in MySQL 8.0.4.
* keyring_aws_cmk_id: AWS keyring plugin customer master key ID value. Added in MySQL 8.0.11.
* keyring_aws_conf_file: AWS keyring plugin configuration file location. Added in MySQL 8.0.11.
* keyring_aws_data_file: AWS keyring plugin storage file location. Added in MySQL 8.0.11.
* keyring_aws_region: AWS keyring plugin region. Added in MySQL 8.0.11.
* keyring_encrypted_file_data: keyring_encrypted_file plugin data file. Added in MySQL 8.0.11.
* keyring_encrypted_file_password: keyring_encrypted_file plugin password. Added in MySQL 8.0.11.
* keyring_okv_conf_dir: Oracle Key Vault keyring plugin configuration directory. Added in MySQL 8.0.11.
* keyring_operations: Whether keyring operations are enabled. Added in MySQL 8.0.4.
* log_error_filter_rules: Filter rules for error logging. Added in MySQL 8.0.2.
* log_error_services: Components to use for error logging. Added in MySQL 8.0.2.
* log_error_suppression_list: Warning/information error log messages to suppress. Added in MySQL 8.0.13.
* log_slow_extra: Whether to write extra information to the slow query log file. Added in MySQL 8.0.14.
* mandatory_roles: Automatically granted roles for all users. Added in MySQL 8.0.2.
* mysql_firewall_mode: Whether MySQL Enterprise Firewall is operational. Added in MySQL 8.0.11.
* mysql_firewall_trace: Whether to enable firewall trace. Added in MySQL 8.0.11.
* mysqlx: Whether X Plugin is initialized. Added in MySQL 8.0.11.
* mysqlx_interactive_timeout: Number of seconds to wait for interactive clients to timeout. Added in MySQL 8.0.4.
* mysqlx_read_timeout: Number of seconds to wait for blocking read operations to complete. Added in MySQL 8.0.4.
* mysqlx_wait_timeout: Number of seconds to wait for activity on a connection. Added in MySQL 8.0.4.
* mysqlx_write_timeout: Number of seconds to wait for blocking write operations to complete. Added in MySQL 8.0.4.
* named_pipe_full_access_group: Name of Windows group granted full access to the named pipe. Added in MySQL 8.0.14.
* no-dd-upgrade: Prevent automatic upgrade of data dictionary tables at startup. Added in MySQL 8.0.4.
* no-monitor: Do not fork monitor process required for RESTART. Added in MySQL 8.0.12.
* original_commit_timestamp: The time when a transaction was committed on the original master. Added in MySQL 8.0.1.
* original_server_version: The MySQL Server release number of the server where a transaction was originally committed. Added in MySQL 8.0.14.
* partial_revokes: Whether partial revocation is enabled. Added in MySQL 8.0.16.
* password_history: Number of password changes required before password reuse. Added in MySQL 8.0.3.
* password_require_current: Whether password changes require current password verification. Added in MySQL 8.0.13.
* password_reuse_interval: Number of days elapsed required before password reuse. Added in MySQL 8.0.3.
* performance_schema_max_digest_sample_age: The query resample age in seconds. Added in MySQL 8.0.3.
* persist_only_admin_x509_subject: SSL certificate X.509 Subject that enables persisting persist-restricted system variables. Added in MySQL 8.0.14.
* persisted_globals_load: Whether to load persisted configuration settings. Added in MySQL 8.0.0.
* print_identified_with_as_hex: For SHOW CREATE USER, print hash values containing unprintable characters in hex. Added in MySQL 8.0.17.
* regexp_stack_limit: Regular expression match stack size limit. Added in MySQL 8.0.4.
* regexp_time_limit: Regular expression match timeout. Added in MySQL 8.0.4.
* resultset_metadata: Whether the server returns result set metadata. Added in MySQL 8.0.3.
* rpl_read_size: Set the minimum amount of data in bytes that is read from the binary log files and relay log files. Added in MySQL 8.0.11.
* secondary_engine_cost_threshold: For future use. Added in MySQL 8.0.16.
* show_create_table_verbosity: Whether to display ROW_FORMAT in SHOW CREATE TABLE even if it has the default value. Added in MySQL 8.0.11.
* sql_require_primary_key: Whether tables must have a primary key. Added in MySQL 8.0.13.
* ssl_fips_mode: Whether to enable FIPS mode on server side. Added in MySQL 8.0.11.
* syseventlog.facility: Facility for syslog messages. Added in MySQL 8.0.13.
* syseventlog.include_pid: Whether to include server PID in syslog messages. Added in MySQL 8.0.13.
* syseventlog.tag: Tag for server identifier in syslog messages. Added in MySQL 8.0.13.
* table_encryption_privilege_check: Enables the TABLE_ENCRYPTION_ADMIN privilege check. Added in MySQL 8.0.16.
* temptable_max_ram: Defines the maximum amount of memory that can occupied by the TempTable storage engine before data is stored on disk. Added in MySQL 8.0.2.
* temptable_use_mmap: Defines whether the TempTable storage engine allocates memory-mapped files when the temptable_max_ram threshold is reached.. Added in MySQL 8.0.16.
* thread_pool_algorithm: The thread pool algorithm. Added in MySQL 8.0.11.
* thread_pool_high_priority_connection: Whether the current session is high priority. Added in MySQL 8.0.11.
* thread_pool_max_unused_threads: The maximum permitted number of unused threads. Added in MySQL 8.0.11.
* thread_pool_prio_kickup_timer: How long before a statement is moved to high-priority execution. Added in MySQL 8.0.11.
* thread_pool_size: Number of thread groups in the thread pool. Added in MySQL 8.0.11.
* thread_pool_stall_limit: How long before a statement is defined as stalled. Added in MySQL 8.0.11.
* tls_ciphersuites: TLSv1.3 ciphersuites permitted for encrypted connections. Added in MySQL 8.0.16.
* upgrade: Control automatic upgrade at startup. Added in MySQL 8.0.16.
* use_secondary_engine: For future use. Added in MySQL 8.0.13.
* validate-config: Validate server configuration. Added in MySQL 8.0.16.
* validate_password.check_user_name: Whether to check passwords against user name. Added in MySQL 8.0.4.
* validate_password.dictionary_file: validate_password dictionary file. Added in MySQL 8.0.4.
* validate_password.dictionary_file_last_parsed: When the dictionary file was last parsed. Added in MySQL 8.0.4.
* validate_password.dictionary_file_words_count: Number of words in dictionary file. Added in MySQL 8.0.4.
* validate_password.length: validate_password required password length. Added in MySQL 8.0.4.
* validate_password.mixed_case_count: validate_password required number of uppercase/lowercase characters. Added in MySQL 8.0.4.
* validate_password.number_count: validate_password required number of digit characters. Added in MySQL 8.0.4.
* validate_password.policy: validate_password password policy. Added in MySQL 8.0.4.
* validate_password.special_char_count: validate_password required number of special characters. Added in MySQL 8.0.4.
* version_compile_zlib: Version of compiled-in zlib library. Added in MySQL 8.0.11.
* windowing_use_high_precision: Whether to compute window functions to high precision. Added in MySQL 8.0.2.
