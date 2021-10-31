---
title: "使用Docker部署Nexus3"
date: 2021-05-01T18:35:28+08:00
draft: false
tags: ["docker", "nexus3"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

```yml
version: "3.7"

services:
  nexus3:
    image: sonatype/nexus3
    restart: unless-stopped
    ports:
      - "8081:8081"
    volumes:
      - nexus_data:/nexus-data
    container_name: nexus3

volumes:
  nexus_data:
```