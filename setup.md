# Clean server setup
* get an Ubuntu/debian server running.

## Root commands
### Ubuntu
* `sudo -s` voor tijdelijke root of `sudo ` + command voor alleen een command.

### Debian
* `su` voor root toegang. (sudo werkt op het niet als er een root password is gemaakt, is dit niet het geval gebruik de Ubuntu methode).

## Install LEMP stack (Linux Nginx MySql Php)
* (root) apt update
* (root) apt install php php-mbstring php-xml composer unzip
* (root) apt install nginx
* (root) apt install mysql-server
* (root) mysql_secure_installation
* (root) apt install php-fpm php-mysql
* (root) nano /etc/php/7.1/fpm/php.ini
* Uncomment `cgi.fix_pathinfo` en zet de waarde naar 0. `cgi.fix_pathinfo=0`
* (root) systemctl restart php7.1-fpm
* (root) nano /etc/nginx/sites-available/default
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html/public;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name your_server_ip;

    location / {
        try_files $uri $uri/ /index.php?query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
* Test de Nginx config via (root) nginx -t
* (root) systemctl restart nginx
* (root) chmod -R 0777 /var/www/html

Nu kan je via ftp je laravel project (zonder vendor of node_module) overzetten op de server, plaats dit in `/var/www/html/`

Je moet wel de .env file nog goed configureren en op de server in `/var/www/html/` `composer install` 
