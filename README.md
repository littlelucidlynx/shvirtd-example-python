# Домашнее задание к занятию 5. «Практическое применение Docker»

## Задача 0

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image000.png)

## Задача 1

**Fork** https://github.com/littlelucidlynx/shvirtd-example-python.git

**Dockerfile.python** https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/Dockerfile.python

Создание тестового стенда, запуск контейнеров, проверка результата. Много писанины, надо переходить к Compose

```
docker network create --subnet=172.20.0.0/16 test-subnet

docker run -d --network='test-subnet' --ip 172.20.0.10 --hostname='db' -e 'MYSQL_ROOT_PASSWORD=very_strong' -e 'MYSQL_DATABASE=example' -e 'MYSQL_USER=app' -e 'MYSQL_PASSWORD=very_strong' --name db mysql:8

docker build -f Dockerfile.python -t web:1.0.0 .

docker run -d --network='test-subnet' --ip 172.20.0.5 --hostname='web' -e 'DB_HOST=db' -e 'DB_USER=app' -e 'DB_PASSWORD=very_strong' -e 'DB_NAME=example' -p 8090:5000 --name web web:1.0.0

curl -L localhost:8090
```

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image001.png)

## Задача 2

Создал реестр через yc cli, запушил образ

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image002.png)

Запустил проверку через yc cli

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image003.png)

## Задача 3

Контейнер с БД долго запускается из-за инициализации. Нужно выставить зависимость запуска веб от БД (depends_on) и проверку состояния готовности БД (healthcheck)

**compose.yaml** https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/compose.yaml

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image004.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image005.png)

## Задача 4

**Bash** https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/deploy-cloud.sh

```
#!/bin/bash
git clone https://github.com/littlelucidlynx/shvirtd-example-python.git /opt/shvirtd-example-python
cd /opt/shvirtd-example-python
docker compose up -d
```

**Fork** https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/README.md

```
docker exec -it shvirtd-example-python-db-1 /bin/sh
mysql -uroot -p${MYSQL_ROOT_PASSWORD}

show databases;
use virtd;
show tables;
SELECT * from requests LIMIT 10;
```

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image006.png)

## Задача 6

Копаюсь в образе через dive, docker save и tar

Скачиваю образ

```
docker pull hashicorp/terraform:latest
dive hashicorp/terraform:latest
```

Нужный слой **9861417e72dc021d24afce363d6a3f0e9fed05399f1a7c6610012fc74b1bb346**

Распаковка и извлечение

```
docker image save -o /tmp/image.tar.gz hashicorp/terraform:latest
cd /tmp
tar xf /tmp/image.tar.gz
ls
cd /tmp/blobs/sha256/
ls
tar xf 9861417e72dc021d24afce363d6a3f0e9fed05399f1a7c6610012fc74b1bb346
ls
cd ./bin
ls
cp terraform ~/terraform
cd ~
ls
```

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image007.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image008.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image009.png)

### Задача 6.1

Копаюсь в образе через docker cp

Запускаю контейнер с терраформом и копирую файл

```
docker run -d --name extraction-test hashicorp/terraform:latest
docker cp extraction-test:/bin/terraform ~/terraform2.0
ls ~
```

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image010.png)
