---
title: "使用Docker部署Caddy"
date: 2021-03-24T18:32:49+08:00
draft: true
---

## Caddyfile

```
```

## docker-compose.yml

```yaml
version: "3.7"

services:
  caddy:
    image: caddy:2.3.0-alpine
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /home/ubuntu/docker/caddy/Caddyfile:/etc/caddy/Caddyfile
      - /home/ubuntu/docker/caddy/site:/srv
      - caddy_data:/data
      - caddy_config:/config

volumes:
  caddy_data:
  caddy_config:
```

## 