#   Angular-Nodejs-Application
##  Ec-2 Instance Deployment
apt update
### Nodejs Installation

curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt install -y nodejs

### Angular Installation

npm install -g @angular/cli

npm install

npm run build

### Mysql Installation

sudo apt install mysql-server

sudo systemctl start mysql

CREATE DATABASE hws;

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin@123!';

flush privileges;

sudo systemctl restart mysql

### Apache2 Installation

apt install apache2

systemctl start apache2

systemctl status apache2

sudo a2enmod proxy

sudo a2enmod proxy_http

sudo a2enmod rewrite

sudo a2enmod ssl

a2dissite 000-default.conf

a2ensite newzyanmonster.conf

a2ensite zyanmonster-ssl.conf

systemctl reload apache2

apachectl configtest

systemctl restart apache2

### SSL CERT Installation

sudo apt install certbot python3-certbot-apache

sudo certbot --apache -d zyanmonster.live

cd /etc/letsencrypt/live/zyanmonster.live/

### newzyanmonster.conf (80)
```
<VirtualHost *:80>
  ServerAdmin vipulsawalw
  DocumentRoot /var/www/html/client
  ServerName zyanmonster.live

  ErrorLog ${APACHE_LOG_DIR}/3milesdev.bridgera.com-error_log
  CustomLog ${APACHE_LOG_DIR}/3milesdev.bridgera.com-access_log common

  KeepAlive On
  # ProxyRequests Off  # Remove or comment out this line
  ProxyPreserveHost On
  ProxyPass /node http://127.0.0.1:3000
  ProxyPassReverse /node http://127.0.0.1:3000

  RewriteEngine On
  RewriteCond %{SERVER_PORT} 80

  ServerName zyanmonster.live
  Redirect permanent / https://zyanmonster.live/

  <Directory "/var/www/html/client/">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>
```
### zyanmonster-ssl.conf (443)
```
<VirtualHost *:443>
  ServerAdmin vipulsawalw
  DocumentRoot /var/www/html/client
  ServerName zyanmonster.live

  # SSL Configuration
  SSLEngine on
  SSLCertificateFile /etc/letsencrypt/live/zyanmonster.live/fullchain.pem
  SSLCertificateKeyFile /etc/letsencrypt/live/zyanmonster.live/privkey.pem

  # Logs
  ErrorLog ${APACHE_LOG_DIR}/zyanmonster-ssl-error.log
  CustomLog ${APACHE_LOG_DIR}/zyanmonster-ssl-access.log common

  # Proxy Configuration
  KeepAlive On
  ProxyPreserveHost On
  ProxyPass /node http://127.0.0.1:3000
  ProxyPassReverse /node http://127.0.0.1:3000

  # Directory Permissions
  <Directory "/var/www/html/client/">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>
```




