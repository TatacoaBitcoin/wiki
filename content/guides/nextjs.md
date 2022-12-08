---
title: 'NextJS'
metaTitle: 'Guías - NextJS
metaDescription: 'How-to guides'
---

## Server

    # apt update
    # apt install curl nginx

## nvm

    # curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    # source ~/.bashrc

## Node

    # nvm install node
    # node --version

# Deployment

nginx

    vi /etc/nginx/nginx.conf

    http {
    	....
    	##
        # Connection header for WebSocket reverse proxy
        ##
        map $http_upgrade $connection_upgrade {
                default upgrade;
                ''      close;
        }
    	....
    }


    vi /etc/nginx/sites-available/api-docs.tatacoabitcoin.com.conf

    server {
      server_name api-docs.tatacoabitcoin.com;
      access_log /var/log/nginx/api-docs.tatacoabitcoin.com-access.log;
      error_log /var/log/nginx/api-docs.tatacoabitcoin.com-error.log;

      location / {
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        proxy_redirect off;
        proxy_buffering off;

        proxy_pass http://127.0.0.1:8009;
      }
    }

    ln -s /etc/nginx/sites-available/api-docs.tatacoabitcoin.com.conf /etc/nginx/sites-enabled/

https://futurestud.io/tutorials/nginx-how-to-fix-unknown-connection_upgrade-variable

    nginx -t

SSL

    # apt install snapd
    # snap install core
    # snap refresh core
    # snap install --classic certbot
    # ln -s /snap/bin/certbot /usr/bin/certbot
    # certbot
    # systemctl restart nginx

App

    # apt install git
    # npm install -g pm2
    git clone https://github.com/TatacoaBitcoin/api-docs-next.git api-docs.tatacoabitcoin.com
    cd api-docs.tatacoabitcoin.com

    vi package.json

    "start": "next start -p 8009",

    openssl rand -base64 32
    echo NEXTAUTH_SECRET=secret > .env.local

    npm install
    npm run build
    pm2 start -n api-docs "npm start"

Update

    git pull
    npm install
    npm run build
    pm2 restart
