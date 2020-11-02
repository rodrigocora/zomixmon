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

## SSL Configuration

### Create dhparam file

`openssl dhparam -out /home/opc/zabbix-db-drivers/docker-compose-server/certificates/dhparam.pem 2048`


### Create the signed certificates

```
sudo certbot certonly --standalone --preferred-challenges https -d zabbix-test.duckdns.org
sudo chmod +r /home/opc/zabbix-db-drivers/docker-compose-server/certificates/ssl.key
sudo chmod +r /home/opc/zabbix-db-drivers/docker-compose-server/certificates/ssl.crt
sudo chmod +r /home/opc/zabbix-db-drivers/docker-compose-server/certificates/dhparam.pem
``` 
#### update the service

`docker service update --force zabbix_zabbix-frontend`

### Schedule the renewing of the certificate
#### crontab configuration example:

`00 02 * * * sudo certbot renew --quiet; docker service update --force zabbix_zabbix-frontend`

