---
title: "使用Docker运行MongoDB"
date: 2021-04-23T12:37:29+08:00
draft: false
tags: ["docker", "mongodb"]
categories: ["docker",]
---

## Use Docker-Compose

```
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
    volumes:
      - db_store:/data/db
    ports:
      - 27017:27017
volumes:
  db_store: {}
```