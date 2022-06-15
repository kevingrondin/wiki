---
sidebar_position: 5
---

# Installer Gitlab

choisir un repertoire comme __var/www/gitlab__ creer __docker-compose.yml__

## Creation du docker

```yml
version: '3'

services:
    gitlab:
      container_name: gitlab
      image: gitlab/gitlab-ce:latest
      restart: always
      hostname: gitlab.monnomdomaine.fr
      environment:
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'https://gitlab.monnomdomaine.fr'
          nginx['listen_https'] = false
      ports:
        - '30443:443'
        - '3080:80'
      volumes:
        - /etc/gitlab/config:/etc/gitlab
        - /etc/gitlab/logs:/var/log/gitlab
        - /etc/gitlab/data:/var/opt/gitlab
```

```shell
docker-compose up
```

## Reverse proxy

__/etc/nginx/conf.d/gitlab.conf__

```yml
# bloc de rediction http => https
server {
  listen 80;
  server_name gitlab.corentindesfarges.fr;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name gitlab.corentindesfarges.fr;

  location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header HOST $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass http://127.0.0.1:30443;
    proxy_redirect off;
  }
}
```

```shell
sudo systemctl restart nginx
```

## certbot

```shell
sudo apt install certbot
```

```shell
sudo certbot --nginx
```

## Root

y'a un fichier password dans le container ici __etc/gitlab__

```shell
docker exec -it <id> bash
```