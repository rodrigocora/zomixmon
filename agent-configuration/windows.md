**Download Zabbix Agent**
- https://www.zabbix.com/br/download_agents

**MSI Installer**
- Install
  	Server=zomixmon-server.northeurope.cloudapp.azure.com
	  ServerActive=zomixmon-server.northeurope.cloudapp.azure.com
	  Hostname=<The FQDN of the Windows server>

**Manual Install/Unzip:**

c:\zabbix\conf\zabbix.agentd.conf: 
	Server=zomixmon-server.northeurope.cloudapp.azure.com
	ServerActive=zomixmon-server.northeurope.cloudapp.azure.com
	Hostname=<The FQDN of the Windows server>

zabbix_agentd.exe --config C:\zabbix\conf\zabbix_agentd.conf --install

c:\zabbix_agentd.exe --start


**Post-Install Actions: Windows Firewall**

netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="Open Port 10050" dir=in action=allow protocol=TCP localport=10050

