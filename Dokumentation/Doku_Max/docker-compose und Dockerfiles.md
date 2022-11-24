## docker-compose
```yml
version: '2.12'
services:
        mysql:
                image: mysql
                ports:
                        - 3306:3306
                environment:
                        - MYSQL_ROOT_PASSWORD=adminadmin
                volumes:
                        - ~/data/docker/container_mysql/database:/var/lib/mysql
                command: --init-file init.sql
                container_name: mysql
                networks:
                        - AzubiDevOps-Netz
        java:
                ports:
                        - 8081:8080
                build:
                        context: /root/AzubiDevOps/Dockerfiles/JAVA
                        dockerfile: Dockerfile
                container_name: java
                depends_on:
                        - mysql
                networks:
                        - AzubiDevOps-Netz
networks:
        AzubiDevOps-Netz:
                ipam:
                        driver: default
                        config:
                                - subnet: "192.168.0.0/24"
```
___

## Java
```bash
FROM openjdk:11

ADD customerapi.jar AzubiDevOps-App.jar

ENTRYPOINT ["java","-jar","AzubiDevOps-App.jar"]
```

im Ordner des JAVA-Dockerfiles
```bash
docker build .
```

Container ausführen
```bash
docker run --net Netz1 --name java -d [build-ID]
```
---

## MySQL
```bash
FROM mysql

LABEL maintainer="database"

ENV MYSQL_ROOT_PASSWORD adminadmin

EXPOSE 3306
```

im Ordner des SQL-Dockerfiles
```bash
docker build .
```

Container ausführen
```bash
docker run --net Netz1 --name mysql -d [build-ID]
```

init.sql in ~/data/docker/container_mysql/database (Ordner des Containers) einfügen um Datenbank und Tabelle im MySql zu erstellen 
```bash
CREATE DATABASE IF NOT EXISTS customerapi;
USE customerapi;
CREATE TABLE IF NOT EXISTS Customer (id int, Vorname varchar(255), Nachname varchar(255)
);
```