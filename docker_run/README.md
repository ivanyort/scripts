# Iniciar ambientes para testes

Rede entre os containers
```
docker network create demo-net
```
Somente Bancos de dados
```
docker run --name sqlserver --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 1433:1433 -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qlik123$" -e "MSSQL_AGENT_ENABLED=1" -d ivanyort/sqlserver-2019
docker run --name postgres --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d --privileged -p 1521:1521 ivanyort/oracle-12c
```
Todos
```
docker run --name sqlserver --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 1433:1433 -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Qlik123$" -e "MSSQL_AGENT_ENABLED=1" -d ivanyort/sqlserver-2019
docker run --name postgres --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d --privileged -p 1521:1521 ivanyort/oracle-12c
docker run --name data-gateway --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d -e "TENANT=yort.us.qlikcloud.com" ivanyort/data-gateway
docker run --name demo-apps --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d -p 8080:80 ivanyort/demo-apps
docker run --name replicate --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d -p 3552:3552 ivanyort/replicate
docker run --name kafka --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d --net demo-net -p 2181:2181 -p 3030:3030 -p 8081-8083:8081-8083 -p 9581-9585:9581-9585 -p 9092:9092 lensesio/fast-data-dev:latest
```
Portainer
```
docker volume create portainer_data
docker run -d -p 8000:8000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Não Utilizar, somente historico
```
docker run --detach --name mariadb --env MARIADB_USER=root --env MARIADB_PASSWORD=Qlik123$ --env MARIADB_ROOT_PASSWORD=Qlik123$  mariadb:10.2.6
docker run --name sap --restart always --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media -d esjewett/nwabap750
docker run --name sap --net demo-net -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media sunpizza/nwabap
docker run -d --name firebird -p 3050:3050 --restart unless-stopped	-v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media mladenp87/firebird-1.5.6-ss
docker run -d --name firebird -p 3050:3050 --restart unless-stopped	-v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media ihsahn/firebird-docker
docker pull ihsahn/firebird-docker
docker run -d --name interbase -p 3050:3050 --restart unless-stopped -v /mnt/c/Users/gpl/OneDrive/github/qlik-data-integration/replicate/ivayt:/media docker pull sybilla/interbase:6.0.1
```