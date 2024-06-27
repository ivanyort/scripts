# Iniciar ambientes para testes

Rede entre os containers
```
docker network create demo-net
```
Somente Bancos de dados
```
docker run --name sqlserver --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1433:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022
docker run --name postgres --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -e ORACLE_PASSWORD=Qlik123$ -p 1521:1521 ivanyort/oracle-21c
```
Todos
```
docker run --name sqlserver --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1433:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022
docker run --name postgres --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -e ORACLE_PASSWORD=Qlik123$ -p 1521:1521 ivanyort/oracle-21c
docker run --name data-gateway --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3553:3552 -e "TENANT=yort.us.qlikcloud.com" ivanyort/data-gateway
docker run --name demo-apps --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 80:80 ivanyort/demo-apps
docker run --name replicate --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3552:3552 ivanyort/replicate
docker run --name it-tools --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 8080:80 -it corentinth/it-tools
docker run --name kafka --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 2181:2181 -p 3030:3030 -p 8081-8083:8081-8083 -p 9581-9585:9581-9585 -p 9092:9092 lensesio/fast-data-dev:latest
```
Portainer
```
docker volume create portainer_data
docker run -d -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Talend
```
docker run --name remote-engine --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -d -e "PREAUTHKEY=XXXXXXXXXXXXXXXX" ivanyort/remote-engine
docker run --name tdc --add-host=demo.labsp.com:host-gateway --mac-address="12:34:de:b0:6b:61" --restart always --net demo-net -p 11480:11480 -p 4432:4432 -d ivanyort/tdc
```

