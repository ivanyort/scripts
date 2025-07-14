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
docker run --name data-gateway --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3553:3552 -e "TENANT=yort.us.qlikcloud.com" ivanyort/data-gateway-public
docker run --name demo-apps --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 80:80 ivanyort/demo-apps
docker run --name replicate --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 3552:3552 ivanyort/replicate
docker run --name it-tools --add-host=demo.labsp.com:host-gateway --restart always --net demo-net  -d -p 8080:80 -it corentinth/it-tools
docker rm -f zookeeper broker schema-registry rest-proxy kafka-ui; export LOCALHOSTNAME=demo.labsp.com; curl https://raw.githubusercontent.com/ivanyort/scripts/refs/heads/main/docker_run/kafka-compose.yml -o kafka-compose.yml; docker compose -f kafka-compose.yml rm -fsv; docker compose -f kafka-compose.yml create; docker compose -f kafka-compose.yml start
docker run -d --name minio --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 9000:9000 -p 9001:9001 -e MINIO_ROOT_USER=qlik -e MINIO_ROOT_PASSWORD=Qlik123$ minio/minio server /data --console-address ":9001"
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
docker run -p 389:389 -p 636:636 --name openldap --add-host=demo.labsp.com:host-gateway --net demo-net --env LDAP_ORGANISATION="Demo Lab" --env LDAP_DOMAIN="demolab.com.br" --env LDAP_ADMIN_PASSWORD="Qlik123$" --detach osixia/openldap:1.3.0
docker run -p 6443:443 --name phpldapadmin --hostname phpldapadmin --add-host=demo.labsp.com:host-gateway --net demo-net --link openldap:rescue --env PHPLDAPADMIN_LDAP_HOSTS=rescue --detach osixia/phpldapadmin:0.9.0
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
docker run -it --name qem --add-host=demo.labsp.com:host-gateway --restart always --net demo-net -p 8006:8006 -p 4389:3389 -p 8443:8443 -p 8083:8083 --volume ./w25.iso:/boot.iso --device=/dev/kvm --cap-add NET_ADMIN --stop-timeout 120 -e "VERSION=2025" -e "RAM_SIZE=16G" -e "CPU_CORES=8" -e "DISK_SIZE=256G" -e "USERNAME=root" -e "PASSWORD=Qlik123$" dockurr/windows
```
Ajustar o Relógio
Instalar Google Chrome Canary
Instalar o 7-zip
Instalar o QEM
```
cd "\Program Files\Attunity\Enterprise Manager\bin"
aemctl.exe configuration set --address demo.labsp.com
aemctl.exe configuration set --http_port 8083
aemctl.exe configuration set --https_port 8443
```

Criar tabelas de Clientes em massa (postgres)
```
DROP TABLE IF EXISTS customers_10k;
DROP TABLE IF EXISTS customers_100k;
DROP TABLE IF EXISTS customers_1M;
DROP TABLE IF EXISTS customers_10M;
DROP TABLE IF EXISTS customers_100M;

CREATE TABLE customers_10k (
  id                   BIGSERIAL PRIMARY KEY,
  name                 TEXT       NOT NULL,
  email                TEXT       NOT NULL UNIQUE,
  social_security_num  CHAR(11)   NOT NULL UNIQUE,
  registration_date    TIMESTAMP  NOT NULL DEFAULT now(),
  balance              NUMERIC(10,2) NOT NULL DEFAULT 0
);
CREATE TABLE customers_100k (
  id                   BIGSERIAL PRIMARY KEY,
  name                 TEXT       NOT NULL,
  email                TEXT       NOT NULL UNIQUE,
  social_security_num  CHAR(11)   NOT NULL UNIQUE,
  registration_date    TIMESTAMP  NOT NULL DEFAULT now(),
  balance              NUMERIC(10,2) NOT NULL DEFAULT 0
);
CREATE TABLE customers_1M (
  id                   BIGSERIAL PRIMARY KEY,
  name                 TEXT       NOT NULL,
  email                TEXT       NOT NULL UNIQUE,
  social_security_num  CHAR(11)   NOT NULL UNIQUE,
  registration_date    TIMESTAMP  NOT NULL DEFAULT now(),
  balance              NUMERIC(10,2) NOT NULL DEFAULT 0
);
CREATE TABLE customers_10M (
  id                   BIGSERIAL PRIMARY KEY,
  name                 TEXT       NOT NULL,
  email                TEXT       NOT NULL UNIQUE,
  social_security_num  CHAR(11)   NOT NULL UNIQUE,
  registration_date    TIMESTAMP  NOT NULL DEFAULT now(),
  balance              NUMERIC(10,2) NOT NULL DEFAULT 0
);
CREATE TABLE customers_100M (
  id                   BIGSERIAL PRIMARY KEY,
  name                 TEXT       NOT NULL,
  email                TEXT       NOT NULL UNIQUE,
  social_security_num  CHAR(11)   NOT NULL UNIQUE,
  registration_date    TIMESTAMP  NOT NULL DEFAULT now(),
  balance              NUMERIC(10,2) NOT NULL DEFAULT 0
);

INSERT INTO customers_10k (name, email, social_security_num, registration_date, balance)
SELECT
  'Customer ' || g                            AS name,
  'customer' || g || '@example.com'           AS email,
  lpad((10000000000 + g)::TEXT, 11, '0')      AS social_security_num,
  now() - (random() * interval '365 days')    AS registration_date,
  round((random()*10000)::numeric, 2)         AS balance
FROM generate_series(1, 10000) AS g;

INSERT INTO customers_100k (name, email, social_security_num, registration_date, balance)
SELECT
  'Customer ' || g,
  'customer' || g || '@example.com',
  lpad((20000000000 + g)::TEXT, 11, '0'),
  now() - (random() * interval '365 days'),
  round((random()*10000)::numeric, 2)
FROM generate_series(1, 100000) AS g;

INSERT INTO customers_1M (name, email, social_security_num, registration_date, balance)
SELECT
  'Customer ' || g,
  'customer' || g || '@example.com',
  lpad((30000000000 + g)::TEXT, 11, '0'),
  now() - (random() * interval '365 days'),
  round((random()*10000)::numeric, 2)
FROM generate_series(1, 1000000) AS g;

DO $$
DECLARE
  batch_size  CONSTANT INT := 1000000;
  i           INT;
BEGIN
  FOR i IN 0 .. 9 LOOP
    INSERT INTO customers_10m (name, email, social_security_num, registration_date, balance)
    SELECT
      'Customer ' || (i * batch_size + g),
      'customer' || (i * batch_size + g) || '@example.com',
      lpad((40000000000 + i * batch_size + g)::TEXT, 11, '0'),
      now() - (random() * interval '365 days'),
      round((random()*10000)::numeric, 2)
    FROM generate_series(1, batch_size) AS g;
    COMMIT;
  END LOOP;
END
$$;

DO $$
DECLARE
  batch_size  CONSTANT INT := 1000000;
  i           INT;
BEGIN
  FOR i IN 0 .. 99 LOOP
    INSERT INTO customers_100m (name, email, social_security_num, registration_date, balance)
    SELECT
      'Customer ' || (i * batch_size + g),
      'customer' || (i * batch_size + g) || '@example.com',
      lpad((40000000000 + i * batch_size + g)::TEXT, 11, '0'),
      now() - (random() * interval '365 days'),
      round((random()*10000)::numeric, 2)
    FROM generate_series(1, batch_size) AS g;
    COMMIT;
  END LOOP;
END
$$;
```
Criar tabela com controle de criado em e atualizado em (MariaDB)

```
-- 1. Cria a tabela orders com AUTO_INCREMENT e timestamps automáticos
DROP TABLE IF EXISTS orders;
CREATE TABLE orders (
  order_id      INT             NOT NULL AUTO_INCREMENT PRIMARY KEY,
  customer_id   INT             NOT NULL,
  product_id    INT             NOT NULL,
  quantity      INT             NOT NULL,
  total_amount  DECIMAL(10,2)   NOT NULL,
  status        VARCHAR(20)     NOT NULL DEFAULT 'pending',
  created_at    DATETIME        NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at    DATETIME        NOT NULL DEFAULT CURRENT_TIMESTAMP
                  ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB;

INSERT INTO orders (
  customer_id,
  product_id,
  quantity,
  total_amount,
  status,
  created_at,
  updated_at
)
SELECT
  FLOOR(RAND() * 1000) + 1,                                           -- customer_id entre 1 e 1000
  FLOOR(RAND() * 100) + 1,                                            -- product_id entre 1 e 100
  FLOOR(RAND() * 10) + 1,                                             -- quantity entre 1 e 10
  ROUND(RAND() * 1000 + 20, 2),                                       -- total_amount entre 20.00 e ~1020.00
  ELT(
    FLOOR(RAND() * 5) + 1,
    'pending','processing','shipped','delivered','cancelled'
  ),                                                                  -- status aleatório
  DATE_SUB(NOW(), INTERVAL FLOOR(RAND() * 365) DAY),                  -- created_at até 365 dias atrás
  DATE_SUB(NOW(), INTERVAL FLOOR(RAND() * 365) DAY)                   -- updated_at até 365 dias atrás
FROM mysql.help_topic AS a
CROSS JOIN mysql.help_topic AS b
-- (se precisar de mais linhas, adicione mais um CROSS JOIN c)
LIMIT 100000;
```



