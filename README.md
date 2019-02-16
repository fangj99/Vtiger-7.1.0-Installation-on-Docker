# Vtiger-7.1.0-Installation-on-Docker
Install Vtiger 7.1.0 on Docker

## Docker image alexcomu/docker-nginx-ssh-mysql-php7
```
https://hub.docker.com/r/alexcomu/docker-nginx-ssh-mysql-php7/
```

## Create container
```
sudo docker run -p 7011:22 -p 8011:80  --name vtiger -h vtiger -d alexcomu/docker-nginx-ssh-mysql-php7:latest

```

## Setup Nginx
```
server {
      listen 80;
      root /var/www/html/vtigercrm;
      index  index.php index.html index.htm;
      server_name  <your server ip>;
      client_max_body_size 100M;

      location / {
                   try_files $uri $uri/ /index.php?$args;
                   if (!-e $request_filename){ 
                   rewrite ^/(.*) /index.php?r=$1 last;
                   }
                 }
                 
      location ~ \.php$ {
                   fastcgi_split_path_info ^(.+\.php)(/.+)$;
                   fastcgi_pass  127.0.0.1:9000;
                   fastcgi_index index.php;
                   include fastcgi.conf;
                   }
}
```

## Setup Mysql
```
CREATE DATABASE vtigerdb;
CREATE USER 'vtigeruser'@'localhost' IDENTIFIED BY '<your_password>';
GRANT ALL ON vtigerdb.* TO 'vtigeruser'@'localhost' IDENTIFIED BY '<your_password>' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;

```
Run following command to avoid error "result is not an object"
```
ALTER DATABASE vtigerdb  CHARACTER SET utf8 COLLATE utf8_general_ci;

```

## Download Vtiger 7.1.0
Need download the version 7.1.0 or it will not woring with all kinds of errors
```
https://cytranet.dl.sourceforge.net/project/vtigercrm/vtiger%20CRM%207.1.0/Core%20Product/vtigercrm7.1.0.tar.gz
```


## Then you can find the Vtiger 7.1.0 GUI on http://serverip:8011

## Other problems
### Vtiger7-Import-Manual-Installation
```
https://github.com/fangj99/Vtiger7-Import-Manual-Installation
```
### Vtiger7-Language-Pack-Installation
```
https://github.com/fangj99/Vtiger7-Language-Pack-Installation
```

Reference:
```
https://websiteforstudents.com/configure-vtiger-crm-on-ubuntu-16-04-lts-with-nginx-mariadb-and-php-7-1-support/

```
