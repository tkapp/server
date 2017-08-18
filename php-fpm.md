# php-fpm

## 概要

nginxでphpを実行するために、php-fpmをインストールします。

## 前提

centos7
nginx構築済み

## 手順

### php-fpmインストール

````
yum -y install php-fpm
````

### 設定ファイル変更
````
sed -i -e "s|listen = 127.0.0.1:9000|listen = /var/run/php5-fpm.sock|g" /etc/php-fpm.d/www.conf
sed -i -e "s|;listen.owner = nobody|listen.owner = nginx|g" /etc/php-fpm.d/www.conf
sed -i -e "s|;listen.group = nobody|listen.group = nginx|g" /etc/php-fpm.d/www.conf

sed -i -e "s|user = apache|user = nginx|g" /etc/php-fpm.d/www.conf
sed -i -e "s|group = apache|group = nginx|g" /etc/php-fpm.d/www.conf
````

### セッションフォルダのパーミッション変更

````
chown root:nginx /var/lib/php/session
````

### timzezoneの設定
````
sed -i -e "s|;date.timezone =|date.timezone = Asia/Tokyo|" /etc/php.ini
````

### php-fpm起動
````
systemctl start php-fpm
systemctl enable php-fpm
````

### 起動確認
````
netstat -a --unix | grep php
````
