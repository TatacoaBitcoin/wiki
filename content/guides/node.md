---
title: 'Node'
metaTitle: 'Guías - Node'
metaDescription: 'How-to guides'
---

# First Deployment

## nvm

    # curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    # source ~/.bashrc

## Node

    # nvm install node
    # node --version

## Code

    # git clone git@github.com:tatacoa/tatacoa-api.git /var/www/html/api.tatacoabitcoin.com
    # cd /var/www/html/api.tatacoabitcoin.com
    # npm install
    # grep "process.env" app.js controllers/* middleware/* models/* routes/* utils/*
    # vi .env

## Api service

    # npm install pm2 -g
    # pm2 start -n api index.js -p 8009
    # pm2 ls
    # pm2 restart 0
    # pm2 startup systemd
    # systemctl status pm2-root

## Nginx

    # vi /etc/nginx/sites-available/api.tatacoabitcoin.com.conf

    server {
        server_name api.tatacoabitcoin.com;
        access_log /var/log/nginx/api.tatacoabitcoin.com-access.log;
        error_log /var/log/nginx/api.tatacoabitcoin.com-error.log;

        location / {
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header Host $http_host;
          proxy_set_header X-NginX-Proxy true;

          proxy_set_header X-Forwarded-Proto $scheme;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection $connection_upgrade;

          proxy_redirect off;
          proxy_buffering off;

          proxy_pass http://127.0.0.1:8003;
        }
    }

    # ln -s /etc/nginx/sites-available/api.tatacoabitcoin.com.conf /etc/nginx/sites-enabled/
    # nginx -t
    # systemctl restart nginx

## SSL

    # certbot
    # systemctl restart nginx

# Update

    # ssh
    # cd /var/www/html/api.tatacoabitcoin.com
    # git pull
    # pm2 ls
    # pm2 restart
