***Follow the link***
* https://www.zabbix.com/integrations/oracle#tab:official1



**Create user**
CREATE USER zabbix_mon IDENTIFIED BY <PASSWORD>;
-- Grant access to the zabbix_mon user.
GRANT CONNECT, CREATE SESSION TO zabbix_mon;
GRANT SELECT ON DBA_TABLESPACE_USAGE_METRICS TO zabbix_mon;
GRANT SELECT ON DBA_TABLESPACES TO zabbix_mon;
GRANT SELECT ON DBA_USERS TO zabbix_mon;
GRANT SELECT ON SYS.DBA_DATA_FILES TO zabbix_mon;
GRANT SELECT ON V$ACTIVE_SESSION_HISTORY TO zabbix_mon;
GRANT SELECT ON V$ARCHIVE_DEST TO zabbix_mon;
GRANT SELECT ON V$ASM_DISKGROUP TO zabbix_mon;
GRANT SELECT ON V$DATABASE TO zabbix_mon;
GRANT SELECT ON V$DATAFILE TO zabbix_mon;
GRANT SELECT ON V$INSTANCE TO zabbix_mon;
GRANT SELECT ON V$LOG TO zabbix_mon;
GRANT SELECT ON V$OSSTAT TO zabbix_mon;
GRANT SELECT ON V$PGASTAT TO zabbix_mon;
GRANT SELECT ON V$PROCESS TO zabbix_mon;
GRANT SELECT ON V$RECOVERY_FILE_DEST TO zabbix_mon;
GRANT SELECT ON V$RESTORE_POINT TO zabbix_mon;
GRANT SELECT ON V$SESSION TO zabbix_mon;
GRANT SELECT ON V$SGASTAT TO zabbix_mon;
GRANT SELECT ON V$SYSMETRIC TO zabbix_mon;
GRANT SELECT ON V$SYSTEM_PARAMETER TO zabbix_mon;




***CHECKS via AGENT2***

**Template DB Oracle by Zabbix Agent 2**
/usr/lib/systemd/system/zabbix-agent2.service
[Unit]
Description=Zabbix Agent 2
After=syslog.target
After=network.target

[Service]
Environment="CONFFILE=/etc/zabbix/zabbix_agent2.conf"
EnvironmentFile=-/etc/sysconfig/zabbix-agent2
Type=simple
Restart=on-failure
PIDFile=/run/zabbix/zabbix_agent2.pid
KillMode=control-group
ExecStart=/usr/sbin/zabbix_agent2 -c $CONFFILE
ExecStop=/bin/kill -SIGTERM $MAINPID
RestartSec=10s
User=zabbix
Group=zabbix
Environment="ORACLE_HOME=/u01/app/oracle/product/11g/dbhome01"
Environment="LD_LIBRARY_PATH=/u01/app/oracle/product/11g/dbhome01/lib"
Environment="TNS_ADMIN=/u01/app/oracle/product/11g/dbhome01/network/admin"

[Install]
WantedBy=multi-user.target


notas:
2020/11/19 16:09:46.740920 [Oracle] Cannot fetch data: dpiStmt_execute: ORA-00907: missing right parenthesis.
https://www.zabbix.com/forum/zabbix-troubleshooting-and-problems/28165-zabbix-2-with-oracle-ora-00907-missing-right-parenthesis


**Template DB Oracle by Zabbix ODBC**


