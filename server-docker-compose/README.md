## Installation 

```
sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce git
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker omixmon
```

## Clone

`git clone https://github.com/rodrigocora/zomixmon.git`

## SSL Configuration

### Certbot installation

[Install snap](https://certbot.eff.org/lets-encrypt/centosrhel8-nginx)



### Create dhparam file

`openssl dhparam -out ~/zomixmon/server-docker-compose/certificates/dhparam.pem 2048`

### Allow connection on port 80


### Create the signed certificates

```
echo "export zabbix_url=<zabbix-url-example.com>" > ~/.bash_profile
source ~/.bash_profile
```

```
sudo certbot certonly --standalone --preferred-challenges http -d ${zabbix_url}
sudo chmod o+r /etc/letsencrypt/live/${zabbix_url}/fullchain.pem
sudo chmod o+r /etc/letsencrypt/live/${zabbix_url}/privkey.pem

``` 
#### update the service (if deployed)

`docker service update --force zabbix_zabbix-frontend`

### Schedule the renewing of the certificate
#### crontab configuration example:

`00 02 * * * sudo certbot renew --quiet; docker service update --force zabbix_zabbix-frontend`

```
docker stack deploy zabbix-server --prune --compose-file ~/zomixmon/server-docker-compose/docker-compose.yml
```

## Install zabbix-agent

### centOS 8

rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/8/x86_64/zabbix-release-5.0-1.el8.noarch.rpm
dnf install zabbix-agent2
