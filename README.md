http://127.0.0.1/admin_tbs

http://indeshop.nl.local/

#Commands
sudo systemctl restart php8.2-fpm
sudo systemctl restart nginx
tail -20 var/log/exception.log
tail -20 var/log/system.log
sudo tail -20 /var/log/nginx/error.log

sudo chgrp -R www-data var
sudo find var -type d -exec chmod 775 {} \;
sudo find var -type f -exec chmod 664 {} \;

ls -ld var/cache

sudo chgrp -R www-data generated pub/static pub/media
sudo chmod -R 775 generated pub/static pub/media

sudo find var -type d -exec chmod 775 {} \;
sudo find var -type f -exec chmod 664 {} \;
namei -l /var/www/indeshop/var/cache
grep "^user =" /etc/php/8.2/fpm/pool.d/www.conf
grep "^group =" /etc/php/8.2/fpm/pool.d/www.conf


# Indeshop Magento Project

## Project Location

```bash
/var/www/indeshop
```

## Open the Project

```bash
cd /var/www/indeshop
```

---

# Services

## Nginx

### Start

```bash
sudo systemctl start nginx
```

### Stop

```bash
sudo systemctl stop nginx
```

### Restart

```bash
sudo systemctl restart nginx
```

### Status

```bash
sudo systemctl status nginx
```

---

## PHP-FPM (PHP 8.2)

### Restart

```bash
sudo systemctl restart php8.2-fpm
```

### Status

```bash
sudo systemctl status php8.2-fpm
```

---

## OpenSearch

### Check Status

```bash
curl http://127.0.0.1:9200
```

Expected response:

```json
{
  "cluster_name": "opensearch"
}
```

---

# Website URLs

## Frontend

```
http://127.0.0.1/
```

or

```
http://localhost/
```

## Admin URL

Find the admin URL:

```bash
php bin/magento info:adminuri
```

Example:

```
http://127.0.0.1/admin_xxxxx
```

---

# Magento CLI

## Magento Version

```bash
php bin/magento --version
```

## Deployment Mode

```bash
php bin/magento deploy:mode:show
```

---

# Cache Commands

## Flush Cache

```bash
php bin/magento cache:flush
```

## Clean Cache

```bash
php bin/magento cache:clean
```

## Disable Cache

```bash
php bin/magento cache:disable
```

## Enable Cache

```bash
php bin/magento cache:enable
```

---

# Upgrade

```bash
php bin/magento setup:upgrade
```

---

# Dependency Injection Compilation

```bash
php -d memory_limit=-1 bin/magento setup:di:compile
```

---

# Static Content Deployment

## Deploy

```bash
php bin/magento setup:static-content:deploy -f en_US nl_NL
```

## Remove Old Static Files

```bash
rm -rf pub/static/frontend
rm -rf pub/static/adminhtml
rm -rf var/view_preprocessed/*
```

---

# Reindex

## Run Reindex

```bash
php bin/magento indexer:reindex
```

## Check Status

```bash
php bin/magento indexer:status
```

---

# Module Commands

## List Modules

```bash
php bin/magento module:status
```

## Enable Module

```bash
php bin/magento module:enable Vendor_Module
```

## Disable Module

```bash
php bin/magento module:disable Vendor_Module
```

---

# Logs

## System Log

Location:

```
var/log/system.log
```

Live View:

```bash
tail -f var/log/system.log
```

## Exception Log

```bash
tail -f var/log/exception.log
```

## Debug Log

```bash
tail -f var/log/debug.log
```

---

# Configuration

## Base URL

```bash
php bin/magento config:show web/unsecure/base_url
```

## Secure URL

```bash
php bin/magento config:show web/secure/base_url
```

## Cookie Domain

```bash
php bin/magento config:show web/cookie/cookie_domain
```

---

# Admin User

## Create Admin User

```bash
php bin/magento admin:user:create
```

## Reset Password

```bash
php bin/magento admin:user:change-password
```

---

# Theme

## Current Theme

```bash
php bin/magento config:show design/theme/theme_id
```

## Theme Locations

```
app/design/
```

or

```
vendor/smartwave/
```

---

# Custom Modules

Location:

```
app/code/
```

Example:

```
app/code/Smartwave/
```

---

# Template Files

```
*.phtml
```

Example:

```
app/design/frontend/...
```

---

# CSS

```
web/css/
```

or

```
web/css/source/
```

---

# JavaScript

```
web/js/
```

---

# Layout XML

```
layout/*.xml
```

---

# Email Templates

```
email/
```

---

# Database

## Login

```bash
mysql -u root -p
```

---

# Composer

## Install Vendor Packages

```bash
composer install
```

---

# Useful Directories

| Directory | Description |
|-----------|-------------|
| `app/` | Application code |
| `vendor/` | Magento Core & Composer packages |
| `generated/` | Generated classes |
| `var/` | Cache & Logs |
| `pub/` | Public web root |
| `app/etc/` | Environment & configuration |

---

# Important Files

| File | Description |
|------|-------------|
| `app/etc/env.php` | Environment configuration |
| `app/etc/config.php` | Module configuration |
| `composer.json` | Composer dependencies |

---

# Nginx Configuration

Configuration file:

```
/etc/nginx/sites-available/indeshop
```

Enabled site:

```
/etc/nginx/sites-enabled/
```

---

# PHP Version

```bash
php -v
```

---

# File Permissions

```bash
sudo chown -R qasim:www-data /var/www/indeshop

find var generated pub/static app/etc -type d -exec chmod 775 {} \;

find var generated pub/static app/etc -type f -exec chmod 664 {} \;
```

---

# Full Deployment

```bash
cd /var/www/indeshop

php bin/magento setup:upgrade

php -d memory_limit=-1 bin/magento setup:di:compile

php bin/magento setup:static-content:deploy -f en_US nl_NL

php bin/magento cache:flush

php bin/magento indexer:reindex

sudo systemctl restart php8.2-fpm

sudo systemctl restart nginx
```

---

# Quick Deployment (Copy & Paste)

```bash
cd /var/www/indeshop && \
php bin/magento setup:upgrade && \
php -d memory_limit=-1 bin/magento setup:di:compile && \
php bin/magento setup:static-content:deploy -f en_US nl_NL && \
php bin/magento cache:flush && \
php bin/magento indexer:reindex && \
sudo systemctl restart php8.2-fpm && \
sudo systemctl restart nginx
```
