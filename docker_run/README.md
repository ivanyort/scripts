# Iniciar ambientes para testes

Rede entre os containers
```
docker network create demo-net
```
Somente Bancos de dados
```
docker run --name sqlserver --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1433:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022
docker run --name sqlserverr --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1444:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022_msreplication
docker run --name postgres --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -e ORACLE_PASSWORD=Qlik123$ -p 1521:1521 ivanyort/oracle-21c
```
Todos
```
docker run --name sqlserver --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1433:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022
docker run --name sqlserverr --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 1444:1433 -e "MSSQL_SA_PASSWORD=Qlik123$" -d ivanyort/sqlserver-2022_msreplication
docker run --name postgres --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 5432:5432 -e "POSTGRES_PASSWORD=Qlik123$" -d ivanyort/postgres-13
docker run --name mysql --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 3306:3306 -e MARIADB_ROOT_PASSWORD=Qlik123$ -d ivanyort/mariadb-10
docker run --name oracle --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -e ORACLE_PASSWORD=Qlik123$ -p 1521:1521 ivanyort/oracle-21c
docker run --name data-gateway --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3553:3552 -e "TENANT=yort.us.qlikcloud.com" ivanyort/data-gateway
docker run --name demo-apps --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 80:80 ivanyort/demo-apps
docker run --name replicate --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3552:3552 ivanyort/replicate
docker run --name it-tools --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 8080:80 -it corentinth/it-tools
export LOCALHOSTNAME=demo.labsp.com; curl https://raw.githubusercontent.com/ivanyort/scripts/refs/heads/main/docker_run/kafka-compose.yml -o kafka-compose.yml; docker compose -f kafka-compose.yml rm -fsv; docker compose -f kafka-compose.yml create; docker compose -f kafka-compose.yml start
```
Portainer
```
docker volume create portainer_data
docker run -d -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Talend
```
docker run --name remote-engine --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 5070-5079:5070-5079 -d -e "PREAUTHKEY=XXXXXXXXXXXXXXXX" ivanyort/remote-engine
docker run --name tdc --add-host=demo.labsp.com:host-gateway --mac-address="12:34:de:b0:6b:61" --restart always --net demo-net --volume="$HOME/.Xauthority:/root/.Xauthority:rw" -p 11481:11481 -p 11480:11480 -p 4432:4432 -p 7070:81 -d ivanyort/tdc
```

Dockur
```
docker run -it --name win1 --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 8006:8006 -p 4389:3389 --device=/dev/kvm --cap-add NET_ADMIN --stop-timeout 120 -e "VERSION=2022" -e "RAM_SIZE=16G" -e "CPU_CORES=8" -e "DISK_SIZE=256G" -e "USERNAME=root" -e "PASSWORD=Qlik123$" dockurr/windows
```

Correção para o netbeans
```
-J-Dnetbeans.slow.system.clipboard.hack=false -J-DTopSecurityManager.disable=true -J-Dorg.netbeans.NbClipboard.level=FINEST
```

Upgrade do data-gateway
```
docker exec data-gateway /opt/upgrade.sh
```

Enterprise Manager
```
docker run -it --name qem --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 8006:8006 -p 4389:3389 -p 8443:8443 -p 8002:8002 --device=/dev/kvm --cap-add NET_ADMIN --stop-timeout 120 -e "VERSION=2022" -e "RAM_SIZE=16G" -e "CPU_CORES=8" -e "DISK_SIZE=256G" -e "USERNAME=root" -e "PASSWORD=Qlik123$" dockurr/windows
```
