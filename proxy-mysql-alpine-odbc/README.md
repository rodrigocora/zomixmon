## CentOS 7
### Install git v2

```
sudo yum -y install https://packages.endpoint.com/rhel/7/os/x86_64/endpoint-repo-1.7-1.x86_64.rpm
sudo yum install git
```
Check the version installed

`git version`


## Build
`docker build https://github.com/rodrigocora/zabbix-db-drivers.git\#main:proxy-mysql-alpine-odbc -t zabbix-proxy-mysql-odbc`



