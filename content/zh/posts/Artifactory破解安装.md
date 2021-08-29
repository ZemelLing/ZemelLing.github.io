---
title: "Artifactory破解安装"
date: 2021-07-02T12:39:58+08:00
draft: false
---

1. 使用Docker安装Artifactory
    export JFROG_HOME="/data/jfrog"

    mkdir -p $JFROG_HOME/artifactory/var/etc/
    cd $JFROG_HOME/artifactory/var/etc/
    touch ./system.yaml
    chown -R $UID:$GID $JFROG_HOME/artifactory/var
    chmod -R 777 $JFROG_HOME/artifactory/var

    docker run --name artifactory --restart always \
    -v $JFROG_HOME/artifactory/var/:/var/opt/jfrog/artifactory \
    -d -p 8081:8081 -p 8082:8082 \
    releases-docker.jfrog.io/jfrog/artifactory-pro:latest
3. 进入Artifatory容器
4. 下载破解jar并运行

/opt/jfrog/artifactory/app/third-party/java/bin/java -jar artifactory-injector-1.1.jar
先破解后生成许可,复制许可

/opt/jfrog/artifactory/app/artifactory/tomcat

eyJhcnRpZmFjdG9yeSI6eyJpZCI6IiIsIm93bmVyIjoicjRwMyIsInZhbGlkRnJvbSI6MTYxMzIwMDgxOTUwNiwiZXhwaXJlcyI6NDc2ODg3NDQxOTUwMywidHlwZSI6IkVOVEVSUFJJU0VfUExVUyIsInRyaWFsIjpmYWxzZSwicHJvcGVydGllcyI6e319fQ==

4. 重启容器
5. 访问 http://ip:8082

sudo nano  /etc/docker/daemon.json

{ "insecure-registries":["hub.lab.lan:8081"] }

sudo service docker restart