#gnuboard7 install Guide

##
mkdir gunboard7
cd gnuboard7
git clone https://github.com/gnuboard/g7.git src

##
mkdir docker
cd docker
mkdir php
mkdir nginx

##
cd nginx
touch default.conf
cd ../php
touch Dockerfile
touch php.ini

## cp setting file
touch docker-compose.yml

## installation 
docker compose up -d --build
docker compose exec app composer install
docker compose exec app cp .env.example .env
docker compose exec app php artisan key:generate
docker compose exec app chown www-data:www-data /var/www/html/.env
docker compose exec app chown -R www-data:www-data /var/www/html/storage
docker compose exec app chown -R www-data:www-data /var/www/html/bootstrap/cache
docker compose exec app chmod -R 775 /var/www/html/storage
docker compose exec app chmod -R 775 /var/www/html/bootstrap/cache
docker compose exec app npm install
docker compose exec app npm run build
docker compose exec app npm run build:core
docker compose exec app npm run build:core-editor
docker compose exec app npm run build:core-devtools
docker compose exec app php artisan storage:link

## db refresh
docker compose exec app php artisan config:cache

