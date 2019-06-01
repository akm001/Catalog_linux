## Linux Configuration for hosting Catalog application

#### * Access the server by the reviewers:



#### * The hosted web application URL:
https://udacity.francecentral.cloudapp.azure.com

#### * Summary of software installation and configuration:
After upgrade (sudo apt update && apt upgrade), install the following software:

apt install postgresql apache2 python3-flask libapache2-mod-wsgi-py3 python3-sqlalchemy python3-oauth2client

vim /etc/apache2/sites-enabled/000-default.conf

<VirtualHost *:443>
        ServerName udacity.francecentral.cloudapp.azure.com
          <Directory /var/www/udacity>
                 WSGIProcessGroup catalog
                 WSGIApplicationGroup %{GLOBAL}
                 WSGIScriptReloading On
                 Order allow,deny
                 Allow from all
                 Require all granted
          </Directory>

        ServerAdmin akm.01@hotmail.com
        DocumentRoot /var/www/udacity

        WSGIDaemonProcess catalog user=www-data group=www-data threads=5 home=/var/www/udacity/
        WSGIScriptAlias / /var/www/udacity/catalog.wsgi
        
        SSLEngine On

        ErrorLog ${APACHE_LOG_DIR}/MAC_error.log
        CustomLog ${APACHE_LOG_DIR}/MAC_access.log combined

        SSLCertificateFile /etc/letsencrypt/live/udacity.francecentral.cloudapp.azure.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/udacity.francecentral.cloudapp.azure.com/privkey.pem
        Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>


#### * Third-party resources:
I used Let's Encrypt to get public signed certificate,
with help of this tutorial https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-18-04

#### Thanks
##### By A.K.M for Udacity FSND 1MAC