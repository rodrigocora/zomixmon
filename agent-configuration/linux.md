***install zabbix agent***

rpm -Uvh http://repo.zabbix.com/zabbix/5.2/rhel/7/x86_64/zabbix-release-5.2-1.el7.noarch.rpm
yum install -y zabbix-agent2

**copy attached zabbix-agent2.conf**

systemctl enable zabbix-agent2
systemctl start zabbix-agent2

**for testing purposes only:**
systemctl stop firewalld
systemctl disable firewalld


