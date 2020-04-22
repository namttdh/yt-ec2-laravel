### Youtube
Part 1 - https://www.youtube.com/watch?v=xI9UW6VET08

### CONNECT
ssh -i pemfile user@ip

### ROOT
sudo su

### UPDATE DENPENCY
apt-get update

### INSTALL NGINX
add-apt-repository ppa:nginx/stable

apt-get update

apt-get install nginx

### INSTALL PHP-FPM7.2
add-apt-repository ppa:ondrej/php

apt-get update

apt-get install php7.2 php7.2-msql php7.2-mysql php7.2-fpm php7.2-xml php7.2-gd php7.2-opcache php7.2-mbstring

### INSTALL COMPOSER
wget https://getcomposer.org/download/1.10.5/composer.phar

### CHECK FREE MEMORY / ADD SWAP
free -m

/bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024

/sbin/mkswap /var/swap.1

/sbin/swapon /var/swap.1


### INSTALL LARAVEL FROM GIT
git clone -b 5.7 https://github.com/laravel/laravel.git

cp .env.example .env

php composer.phar install

php artisan key:generate

### PERMISSION FOLDER LARAVEL
chown -R www-data:www-data storage/

### NGINX CONFIG FOR LARAVEL
```
server {
    listen 80;
    server_name example.com;
    root /example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
### NGINX CHECK STATUS CONFIG
nginx -t

### NGINX RESTART SERVICE AND CHECK STATUS
service nginx restart

service nginx status
