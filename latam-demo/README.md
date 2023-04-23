## Provision LATAM DB

Provision Docker server in QMI Cloud
- Provision Oracle Linux (Data Gateway + Databases)
- 8 proc / 32 Gb Ram / 1000 Disk
- Check only "Azure Data Lake Storage for QCDI staging"

```
sudo su -
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf -y install docker-ce docker-ce-cli containerd.io
systemctl enable docker
systemctl start docker
docker run --name hello-world hello-world
docker container rm -f hello-world
docker system prune -af --volumes
```

Internal docker network
```
docker network create demo-net
```
Databases
```
docker run --name sqlserver --restart always --net demo-net -v /media:/media -p 1433:1433 -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qlik123$" -e "MSSQL_AGENT_ENABLED=1" -d ivanyort/sqlserver-2019
docker run --name postgres --restart always --net demo-net -v /media:/media -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --restart always --net demo-net -v /media:/media -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --restart always --net demo-net -v /media:/media -d --privileged -p 1521:1521 ivanyort/oracle-12c
```
Portainer
```
docker volume create portainer_data
docker run -d -p 8000:8000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
Data Gateway
```
docker run -d -p 3552:3552 --name data-gateway --restart always --net demo-net -v /media:/media  -e "TENANT=changetoyourtenant.us.qlikcloud.com" ivanyort/data-gateway

Replicate interface available (for cheking purposes) with user root/Qlik123$
```

Retrieve data gateway registration key

```
docker logs data-gateway
```


Demo Apps
```
docker run --name demo-apps --restart always --net demo-net -v /media:/media -d -p 80:80 ivanyort/demo-apps
```
