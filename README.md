# KİŞİSEL BULUT DEPOLAMA
## Projenin Amaci

Raspberry pi 3 uzerinde Owncloud 10 yazilimi kullanilarak kisisel bulut depolama alani olusturmak.


## Gerekli Donanım Listesi
Raspbian Stretch yuklu Raspberry pi 3
Micro SD Card(En az 8 GB)
Power Supply
Harddisk

## KURULUM AŞAMALARI

root kullanici yetkisi almak icin
```
sudo su
```
Rasperry pi 3 guncellestirmelerinin yapilmasi
```
apt update  && apt  upgrade
```
Güvenlik için rasperry pi 3 şifresinin değiştirilmesi

## Gerekli paketlerin kurulumu
Owncloud 10 yazılımı için gerekli paketlerin kurulması
```
apt install apache2 -y
```
```
apt install -y apache2 mariadb-server libapache2-mod-php7.0 \
    php7.0-gd php7.0-json php7.0-mysql php7.0-curl \
    php7.0-intl php7.0-mcrypt php-imagick \
    php7.0-zip php7.0-xml php7.0-mbstring
```

## owncloud 10 yazılımını indirilmesi ve kurulma aşamaları
/tmp konumuna Owncloud  yazılımının indirilmesi
```
cd /tmp

wget https://download.owncloud.org/community/owncloud-10.0.10.tar.bz2
```
İndirilen Owncloud yazılımının arşivden cıkarılması
```
tar -xvf owncloud-10.0.10.tar.bz2

chown -R www-data:www-data owncloud
```
Olusturulan Owncloud klasörünün taşınması
```
mv owncloud /var/www/html/
```

Ana dizinde geri donus
```
cd
```

Apache sunucusunun baslatilmasive onyuklenebilir hale getirilmesi
```
systemctl start apache2

systemctl enable apache2
```


## Apache Web Server Yapılandırma
Aşagıda ki komut ile yeni bir yapılandırma dosyası oluşturma
```
sudo nano /etc/apache2/sites-available/owncloud.conf
```
Oluşturulan yeni yapılandırma dosyasına aşağıdaki satırları yapıştırarak kaydedin
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
