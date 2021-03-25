---
title: "使用Docker部署Caddy"
date: 2021-03-24T18:32:49+08:00
draft: false
---
## docker-compose.yml

```yaml
version: "3.7"

services:
  caddy:
    image: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /data/caddy/Caddyfile:/etc/caddy/Caddyfile
      - /data/caddy/stie:/srv
      - caddy_data:/data
      - caddy_config:/config
    container_name: caddy

volumes:
  caddy_data:
  caddy_config:
```
```shell
docker-compose up -d
```
注意一点 /data/caddy/Caddyfile 需要提前创建好

## Caddyfile

Caddy有两种配置格式：1. json 2. Caddyfile，由于Caddyfile较为简洁，因此选用了这种方式。

### Example

```
localhost

respond "Hello, world!"
```

#### 反向代理

```
your_domain {
    reverse_proxy your_server_ip:port
}
```

### Compare
以下是Json格式和Caddyfile格式的区别：
| JSON                                  | Caddyfile                                |
| ------------------------------------- | ---------------------------------------- |
| Full range of Caddy functionality     | Most common parts of Caddy functionality |
| Easy to generate                      | Easy to craft by hand                    |
| Easily programmable                   | Difficult to automate                    |
| Extremely expressive                  | Moderately expressive                    |
| Allows config traversal               | Cannot traverse within Caddyfile         |
| Partial config changes                | Whole config changes only                |
| Can be exported                       | Cannot be exported                       |
| Compatible with all API endpoints     | Compatible with some API endpoints       |
| Documentation generated automatically | Documentation is hand-written            |
| Ubiquitous                            | Niche                                    |
| More efficient                        | More computational                       |
| Kind of boring                        | Kind of fun                              |
| Learn more: JSON structure            | Learn more: Caddyfile docs               |

