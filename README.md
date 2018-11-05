# personelCloud
## Projenin Amaci
Raspberry pi 3 ve Owncloud kullanarak kisisel bulut depolama alani olusturmak<br/>
root kullanici yetkisi almak icin
```
sudo su
```

## Gerekli paketlerin kurulumu
```
apt install apache2 -y
```
```
apt install -y apache2 mariadb-server libapache2-mod-php7.0 \
    php7.0-gd php7.0-json php7.0-mysql php7.0-curl \
    php7.0-intl php7.0-mcrypt php-imagick \
    php7.0-zip php7.0-xml php7.0-mbstring
```
```
systemctl start apache2

systemctl enable apache2
```
```
tar -xvf owncloud-10.0.10.tar.bz2

chown -R www-data:www-data owncloud
```
```
mv owncloud /var/www/html/

```
```
cd
```

```
cd /tmp

wget https://download.owncloud.org/community/owncloud-10.0.10.tar.bz2
```
## Owncloud indirilmesi ve kurulmasi
```
cd /tmp

wget https://download.owncloud.org/community/owncloud-10.0.10.tar.bz2
```
```
tar -xvf owncloud-10.0.10.tar.bz2

chown -R www-data:www-data owncloud
```
```
mv owncloud /var/www/html/

cd
```
## Apache Server Configurasyonu
```
sudo nano /etc/apache2/sites-available/owncloud.conf
```
```
Alias /owncloud "/var/www/html/owncloud/"

<Directory /var/www/html/owncloud/>
 Options +FollowSymlinks
 AllowOverride All

<IfModule mod_dav.c>
 Dav off
 </IfModule>

SetEnv HOME /var/www/html/owncloud
SetEnv HTTP_HOME /var/www/html/owncloud

</Directory>
```
```
sudo su
ln -s /etc/apache2/sites-available/owncloud.conf /etc/apache2/sites-enabled/owncloud.conf
```
```
a2enmod headers
systemctl restart apache2
a2enmod env
a2enmod dir
a2enmod mime
```
```
mysql -u root -p
```
```
MariaDB [(none)]> create database owncloud;
 Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create user owncloud@localhost identified by '12345';
 Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on owncloud.* to owncloud@localhost identified by '12345';
 Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges;
 Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit;
 Bye
```
