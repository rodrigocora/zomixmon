***Follow the link***
> https://www.zabbix.com/integrations/oracle#tab:official1

**Create user**
> CREATE USER c##zabbix_mon IDENTIFIED BY <password>;
  
> GRANT CONNECT, CREATE SESSION TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$instance TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$database TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$sysmetric TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$system_parameter TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$session TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$recovery_file_dest TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$active_session_history TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$osstat TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$restore_point TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$process TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$datafile TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$pgastat TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$sgastat TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$log TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$archive_dest TO c##zabbix_mon;

> GRANT SELECT ON sys.v_$asm_diskgroup TO c##zabbix_mon;

> GRANT SELECT ON sys.dba_data_files TO c##zabbix_mon;

> GRANT SELECT ON DBA_TABLESPACES TO c##zabbix_mon;

> GRANT SELECT ON DBA_TABLESPACE_USAGE_METRICS TO c##zabbix_mon;

> GRANT SELECT ON DBA_USERS TO c##zabbix_mon;



***CHECKS via AGENT2***

**Template DB Oracle by Zabbix Agent 2**
**/usr/lib/systemd/system/zabbix-agent2.service**

> [Unit]
> Description=Zabbix Agent 2
> After=syslog.target
> After=network.target

> [Service]
> Environment="CONFFILE=/etc/zabbix/zabbix_agent2.conf"
> EnvironmentFile=>/etc/sysconfig/zabbix>agent2
> Type=simple
> Restart=on>failure
> PIDFile=/run/zabbix/zabbix_agent2.pid
> KillMode=control>group
> ExecStart=/usr/sbin/zabbix_agent2 >c $CONFFILE
> ExecStop=/bin/kill >SIGTERM $MAINPID
> RestartSec=10s
> User=zabbix
> Group=zabbix
> Environment="ORACLE_HOME=/u01/app/oracle/product/11g/dbhome01"
> Environment="LD_LIBRARY_PATH=/u01/app/oracle/product/11g/dbhome01/lib"
> Environment="TNS_ADMIN=/u01/app/oracle/product/11g/dbhome01/network/admin"

> [Install]
> WantedBy=multi>user.target


**notas:**
2020/11/19 16:09:46.740920 [Oracle] Cannot fetch data: dpiStmt_execute: ORA-00907: missing right parenthesis.
https://www.zabbix.com/forum/zabbix-troubleshooting-and-problems/28165-zabbix-2-with-oracle-ora-00907-missing-right-parenthesis


**Template DB Oracle by Zabbix ODBC**


