# Домашнее задание к занятию 5. «Практическое применение Docker»

## Задача 0

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image000.png)

## Задача 1

Dockerfile.python https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/Dockerfile.python

Fork https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/README.md

```
docker network create --subnet=172.20.0.0/16 test-subnet

docker run -d --network='test-subnet' --ip 172.20.0.10 --hostname='db' -e 'MYSQL_ROOT_PASSWORD=very_strong' -e 'MYSQL_DATABASE=example' -e 'MYSQL_USER=app' -e 'MYSQL_PASSWORD=very_strong' --name db mysql:8

docker build -f Dockerfile.python -t web:1.0.0 .

docker run -d --network='test-subnet' --ip 172.20.0.5 --hostname='web' -e 'DB_HOST=db' -e 'DB_USER=app' -e 'DB_PASSWORD=very_strong' -e 'DB_NAME=example' -p 8090:5000 --name web web:1.0.0

curl -L localhost:8090
```

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image001.png)

## Задача 2

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image002.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image003.png)

## Задача 3

Контейнер с БД долго запускается из-за инициализации. Нужно выставить зависимость запуска веб (depends_on) и проверку состояния готовности БД (healthcheck)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image004.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image005.png)

## Задача 4

Bash https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/deploy-cloud.sh

```
#!/bin/bash
git clone https://github.com/littlelucidlynx/shvirtd-example-python.git /opt/shvirtd-example-python
cd /opt/shvirtd-example-python
docker compose up -d
```

Fork https://github.com/littlelucidlynx/shvirtd-example-python/blob/main/README.md
![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image006.png)

## Задача 6

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image007.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image008.png)

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image009.png)

### Задача 6.1

![Image alt](https://github.com/littlelucidlynx/shvirtd-example-python/raw/main/Screen/Image010.png)