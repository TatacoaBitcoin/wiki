---
title: 'React'
metaTitle: 'Guías - React'
metaDescription: 'How-to guides'
---

# Deployment

server:

    vi /etc/nginx/sites-available/staging-btcpay-dashboard.tatacoabitcoin.com.conf

    server {
      server_name  staging-btcpay-dashboard.tatacoabitcoin.com;
      access_log   /var/www/logs/staging-btcpay-dashboard.tatacoabitcoin.com.access.log;
      error_log    /var/www/logs/staging-btcpay-dashboard.tatacoabitcoin.com.error.log error;
      root         /var/www/html/staging-btcpay-dashboard.tatacoabitcoin.com;
      index        index.html;

      location / {
    	try_files $uri $uri/ /index.html$is_args$args;

}
}

    ln -s /etc/nginx/sites-available/staging-btcpay-dashboard.tatacoabitcoin.com.conf /etc/nginx/sites-enabled/
    nginx -t && systemctl restart nginx
    certbot
    nginx -t && systemctl restart nginx
    mkdir /var/www/html/staging-btcpay-dashboard.tatacoabitcoin.com

local:

    git clone https://github.com/TatacoaBitcoin/btcpay-dashboard
    cd btcpay-dashboard
    grep -r process.env src/*
    vi .env
    npm install
    npm run build
    scp -r build/* user@IP:/var/www/html/staging-btcpay-dashboard.tatacoabitcoin.com/
