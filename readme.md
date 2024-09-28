# Overview

Instruction for installation fresh magento instance utilising Magento Open Source and simple docker environment setup. Tested on apple silicon M1.

# Pre-requisitions

* Available MySql database (there will be an update with mysql in setup)
* Installed docker environment on host 

# Installation

## prepare host machine
```bash
echo "127.0.0.1 magento2-native.docker" | sudo tee -a /etc/hosts
```
Create directory where you need to install magento 
```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition magento
mkdir docker
cd docker
-- install this repository in this catalog ---
```

## build docker environment

```bash
docker compose up -d
```

## Installing fresh magento
enter to php containder
```bash
docker compose exec -ti php /bin/bash
```
execute installation script (remember to update YOUR_ prefixed with correct values)
```bash
bin/magento setup:install \
--base-url=http://magento2-native.docker/ \
--db-host=YOUR_DB_HOST \
--db-name=YOUR_DB_NAME \
--db-user=YOUR_DB_USER \
--db-password=YOUR_DB_PASSWORD \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=YOUR_EMAIL \
--admin-user=admin \
--admin-password=YOUR_PASSWORD \
--language=en_US \
--currency=USD \
--timezone=Europe/Warsaw \
--use-rewrites=1 \
--search-engine=opensearch \
--opensearch-host=opensearch.magento2-native.docker \
--opensearch-port=9200 \
--opensearch-index-prefix=magento2 \
--opensearch-timeout=15

```

# Tips and tricks
To change url you need to modify entry in /etc/hosts (mentioned in **prepare host machine** step) and `image/nginx/conf.d/default.conf`. Not forgetting about database url entries according ot magento documentation. `docker compose down && docker compose up -d` might be required after changes to refresh conf.d.  
