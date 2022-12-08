---
title: 'LNBits'
metaTitle: 'Guías - LNBits'
metaDescription: 'How-to guides'
---

# Debian 11.5

Actualizar servidor

    # apt update
    # apt upgrade

## Python

Primero se instala pyenv para poder instalar la versión de Python que se necesite.

    # apt install \
        make \
        build-essential \
        libssl-dev \
        zlib1g-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
        wget \
        curl \
        llvm \
        libncurses5-dev \
        libncursesw5-dev \
        xz-utils \
        tk-dev \
        libffi-dev \
        liblzma-dev \
        git
    # curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash

Agregarlo al path

    # vi .profile

    export PYENV_ROOT="$HOME/.pyenv"
    command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
    eval "$(pyenv init -)"

    # source .profile
    # pyenv -v

Instalar Python3.9 usando pyenv

    # pyenv install 3.9
    # python3 --version

Instalar poetry, a dependency and package manager for Python.

    # vi .profile

    export PATH="/root/.local/bin:$PATH"

    # source .profile
    # poetry --version

## Postgres

    # apt install postgresql
    # service postgresql status
    # su postgres
    $ psql
    =# ALTER USER postgres PASSWORD passwordd;
    =# \q
    $ createdb lnbits
    $ exit

## LNbits

    # git clone https://github.com/lnbits/lnbits-legend.git
    # cd lnbits-legend/
    # poetry env use python3
    # poetry env info
    # poetry install --only main
    # mkdir data
    # cp .env.example .env
    # vi .env

    LNBITS_DATABASE_URL="postgres://postgres:passwordd@localhost/lnbits"
    LNBITS_SITE_TITLE="Staging LNbits"
    LNBITS_BACKEND_WALLET_CLASS=LndRestWallet
    LND_REST_ENDPOINT="https://btcpay.tatacoabitcoin.com/lnd-rest/btc/"
    LND_REST_CERT=""
    LND_REST_MACAROON="hexmacaroon"

    # /root/.cache/pypoetry/virtualenvs/lnbits-tijLzCFv-py3.9/bin/uvicorn lnbits.__main__:app --host 127.0.0.1 --port 5000

### Service

    # vi /etc/systemd/system/lnbits.service

    [Unit]
    Description=Uvicorn instance to serve Lnbits
    After=network.target

    [Service]
    User=root
    Group=root
    WorkingDirectory=/root/lnbits-legend
    Environment="PATH=/root/.cache/pypoetry/virtualenvs/lnbits-tijLzCFv-py3.9/bin/"
    ExecStart=/root/.cache/pypoetry/virtualenvs/lnbits-tijLzCFv-py3.9//bin/uvicorn lnbits.__main__:app --host 127.0.0.1 --port 5000

    [Install]
    WantedBy=multi-user.target

    # service lnbits status

## Nginx

    # apt install nginx
    # service nginx status

    # vi /etc/nginx/sites-available/staging-lnbits.tatacoabitcoin.com.conf

    server {
        client_max_body_size 4G;
        server_name lnbits.tatacoabitcoin.com;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
            proxy_redirect off;
            proxy_buffering off;
            proxy_pass http://127.0.0.1:8001;
        }

        location /static {
            # path for static files
            root /var/www/html/lnbits.tatacoabitcoin.com/lnbits/;
        }

        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/lnbits.tatacoabitcoin.com/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/lnbits.tatacoabitcoin.com/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    }

    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }

    upstream uvicorn {
        server unix:/tmp/uvicorn.sock;
    }
    server {
        if ($host = lnbits.tatacoabitcoin.com) {
        return 301 https://$host$request_uri;
        } # managed by Certbot


        listen 80;

        server_name lnbits.tatacoabitcoin.com;
        return 404; # managed by Certbot
    }

# Ubuntu 20.04

    # apt -y update

## postgres

    # apt -y install postgresql
    # su postgres
    $ psql
    =# ALTER USER postgres PASSWORD 'passsss_ss';
    =# \q
    $ createdb lnbits
    $ exit

## pyenv

    # apt update -y
    # apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
    # git clone https://github.com/pyenv/pyenv.git .pyenv
    # echo 'export PYENV_ROOT="$HOME/.pyenv"' >> .bashrc
    # echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> .bashrc
    # echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> .bashrc
    # exec "$SHELL"
    # pyenv install 3.7.12 # pyenv install 3.9.10 # latest

## pipenv

    # add-apt-repository universe
    # apt -y update
    # apt -y install software-properties-common python3-pip
    # python3 -m pip install --user pipenv
    # echo export PATH="/root/.local/bin:$PATH" >> .bashrc
    # source .bashrc

## lnbits

    # cd /var/www/html
    # git clone https://github.com/lnbits/lnbits-legend.git lnbits.tatacoabitcoin.com
    # cd lnbits.tatacoabitcoin.com
    # git reset --hard b122debd8c3912be3709ff0cc2d42b74896cfe1b
    # pipenv --python /root/.pyenv/versions/3.7.12/bin/python3 shell
    # pipenv install
    # cp .env.example .env

    LNBITS_BACKEND_WALLET_CLASS=LndRestWallet
    LND_REST_ENDPOINT="https://btcpay.tatacoabitcoin.com/lnd-rest/btc/"
    LND_REST_CERT=""
    LND_REST_MACAROON="asdas"
    LNBITS_DATABASE_URL="postgres://postgres:myPassword@localhost/lnbits"

    # echo LNBITS_DATABASE_URL="postgres://postgres:myPassword@localhost/lnbits" >> .env

    # cd ..
    # chown -R www-data.www-data lnbits.tatacoabitcoin.com
    # cd
    # chown -R www-data.www-data .local/share/virtualenvs/lnbits.tatacoabitcoin.com-7Sj1_cMO

    # vi /etc/systemd/system/lnbits.service

    [Unit]
    Description=Uvicorn instance to serve Lnbits
    After=network.target

    [Service]
    User=root
    Group=root
    WorkingDirectory=/var/www/html/lnbits.tatacoabitcoin.com
    Environment="PATH=/root/.local/share/virtualenvs/lnbits.tatacoabitcoin.com-7Sj1_cMO/bin/"
    ExecStart=/root/.cache/pypoetry/virtualenvs/lnbits-tijLzCFv-py3.9/bin/uvicorn lnbits.__main__:app --host 127.0.0.1 --port 5000

    [Install]
    WantedBy=multi-user.target

    # pipenv run python -m uvicorn lnbits.__main__:app --host 127.0.0.1 --port 8001

## Nginx

    # vi /etc/nginx/sites-available/lnbits.tatacoabitcoin.com.conf

    server {
        client_max_body_size 4G;
        server_name lnbits.tatacoabitcoin.com;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
            proxy_redirect off;
            proxy_buffering off;
            proxy_pass http://127.0.0.1:8001;
        }

        location /static {
            # path for static files
            root /var/www/html/lnbits.tatacoabitcoin.com/lnbits/;
        }

        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/lnbits.tatacoabitcoin.com/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/lnbits.tatacoabitcoin.com/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    }

    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }

    upstream uvicorn {
        server unix:/tmp/uvicorn.sock;
    }
    server {
        if ($host = lnbits.tatacoabitcoin.com) {
        return 301 https://$host$request_uri;
        } # managed by Certbot


        listen 80;

        server_name lnbits.tatacoabitcoin.com;
        return 404; # managed by Certbot
    }


    # ln -s /etc/nginx/sites-available/lnbits.tatacoabitcoin.com.conf /etc/nginx/sites-enabled
    # nginx -t
    # systemctl restart nginx
    # certbot
    # systemctl restart nginx

## Update

    cd /var/www/html/lnbits-main.tatacoabitcoin.com
    git pull
    -- check possible changes not included in upstream repo
    systemctl restart lnbitsmain

6b5aaa442d01fd3e64f8e20c924af1959a39ad3c
Dec 22
404 Not Found

c87abef20fe7aeba8a29f4180eae94c92ff1cc14
Dec 21
404 Not Found

6b1c1af148541fa1655e413a0f543fe3e7cbcf0c
Dec 19
NameError: name 'hmac' is not defined

31c131e2ab1787a2bd39ad2ba3bcfee99d843029
Dec 19
NameError: name 'hmac' is not defined

a86dcb95868999348b51a49a0375cdcadfab5cff
Dec 19
NameError: name 'hmac' is not defined

---

1609280f53dbe369b22df8688a0b4296bacfd349
Nov 28
OK

---

e3c648590362b3fa527529e4d01b86a4a4707e91
Nov 28
No txs

3fd66f38dad185c9b895beab66118cc7e2b1a2f4
Nov 27
OK - rename endpoint to avoid collision - not showing pos list

bc0b64a3fbfc35652ebb036250be1ea5c936165d
Nov 23
Ok - Fixed lnurlpos links not showing - not working transactions endpoint

4e6c30a909dee3d2c54f65fec6ff568f9b3265d6
Nov 17
OK - not showing pos list

Withdrawal
URL: https://lnbits.tatacoabitcoin.com/bleskomat/u
ID: 73be7ff09ffd8150
Key: f75465aca6e45a0a346531cea75d1bb7498249467a13fa06f6143144bc252b13

Payment
URL: https://lnbits.tatacoabitcoin.com/lnurlpos/api/v1/lnurl
ID: LdrfYv4Cj6dV69Yyp93CCL
Key: 3kNK2KeysoGk3ayG6EFzNu

# Installation

commit b122debd8c3912be3709ff0cc2d42b74896cfe1b

## postgres

    # apt -y install postgresql
    # su postgres
    $ psql
    =# ALTER USER postgres PASSWORD 'myPassword';
    =# \q
    $ createdb lnbits
    $ exit

## pyenv

    # apt update -y
    # apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
    # git clone https://github.com/pyenv/pyenv.git .pyenv
    # echo 'export PYENV_ROOT="$HOME/.pyenv"' >> .bashrc
    # echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> .bashrc
    # echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> .bashrc
    # exec "$SHELL"
    # pyenv global 3.7.12
    # pyenv which python3

## pipenv

    # apt install software-properties-common
    # apt-add-repository universe
    # apt update
    # apt install python3-pip
    # python3 -m pip install --user pipenv
    # echo export PATH="/root/.local/bin:$PATH" >> .bashrc
    # source .bashrc

    # exit
    # deactivate

## lnbits

    # git clone https://github.com/lnbits/lnbits-legend.git
    # cd lnbits-legend/
    # git reset --hard b122debd8c3912be3709ff0cc2d42b74896cfe1b
    # pipenv --python /root/.pyenv/versions/3.7.12/bin/python3 shell
    # pipenv install
    # mkdir data
    # cp .env.example .env
    # echo LNBITS_DATABASE_URL="postgres://postgres:myPassword@localhost/lnbits" >> .env
    # pipenv run python -m uvicorn lnbits.__main__:app --host 0.0.0.0

## Nginx

You are using the uwsgi module of nginx. Uvicorn exposes an asgi API. Therefore you should use a "reverse proxy" configuration instead of an uwsgi configuration.

You can get more info on the uvicorn documentation: https://www.uvicorn.org/deployment/#running-behind-nginx (see the proxy_pass line)
