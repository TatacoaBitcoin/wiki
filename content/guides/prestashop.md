---
title: 'Prestashop'
metaTitle: 'Guías - Prestashop'
metaDescription: 'How-to guides'
---

# Install

    php --version
    mysql --version

    apt install php-zip php-intl
    systemctl restart nginx

## Download and upload files

https://www.prestashop.com/en/download

    unzip prestashop_1.7.8.3.zip
    scp index.php prestashop.zip root@ip:/var/www/html/prestashop.com/
    chown -R www-data.www-data /var/www/html/prestashop.tatacoabitcoin.com/

## Nginx site

    ln -s /etc/nginx/sites-available/prestashop.tatacoabitcoin.com.conf /etc/nginx/sites-enabled/prestashop.tatacoabitcoin.com.conf
    nginx -t
    systemctl restart nginx
    certbot

## Database

    mysql
    > CREATE DATABASE prestashop COLLATE utf8mb4_general_ci;
    > CREATE USER "prestashopuser"@"localhost" IDENTIFIED BY "ooo_eben_evans_meld";
    > GRANT ALL PRIVILEGES ON prestashop.* TO "prestashopuser"@"localhost";
    > FLUSH PRIVILEGES;

https://prestashop.tatacoabitcoin.com/prestashop
https://prestashop.tatacoabitcoin.com/install/

https://prestashop.tatacoabitcoin.com/admin145hpxd8d/index.php
