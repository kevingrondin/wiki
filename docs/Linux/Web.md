---
sidebar_position: 3
---

# 🌐 Web

Faire un serveur web

## SSL

Dans la partie __Installation__ on a vue comment sécuriser et rediriger les applications

```Shell
certbot --nginx -d kevingrondin.fr -d www.kevingrondin.fr -d api.kevingrondin.fr
```

### Nginx

deux dossiers :
* /etc/nginx/sites-available (nos fichier configuration)
* /etc/nginx/sites-enable (va avoir que nos lien symbolique des fichiers configuration)

Ici de simple dossier static sont diposé dans __/var/www__

```md
server {
    listen 443 ssl;
    server_name kevingrondin.fr www.kevingrondin.fr

    location / {
        root /var/www/home;
        index index.html index.htm;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri $uri/ /index.html;
    }

    ssl_certificate /etc/letsencrypt/live/kevingrondin.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/kevingrondin.fr/privkey.pem;
}

server {
    listen 443 ssl;
    server_name api.kevingrondin.fr

    location / {
        root /var/www/api;
        index index.html index.htm;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        try_files $uri $uri/ /index.html;
    }

    ssl_certificate /etc/letsencrypt/live/kevingrondin.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/kevingrondin.fr/privkey.pem;
}
```