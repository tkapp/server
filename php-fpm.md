# php-fpmのインストール

## 概要

php-fpmをインストールし、nginxでphpを実行できるようにします。

## 前提

centos7
nginx構築済み

## 手順

### php-fpmインストール

yumでphp-fpmをインストールします。
````
yum -y install php-fpm
````

### 設定ファイル変更

ローカルポートではなく、socketで通信できるように変更します。
````
sed -i -e "s|listen = 127.0.0.1:9000|listen = /var/run/php5-fpm.sock|g" /etc/php-fpm.d/www.conf
````

nginxで利用するための設定を行います。
````
sed -i -e "s|;listen.owner = nobody|listen.owner = nginx|g" /etc/php-fpm.d/www.conf
sed -i -e "s|;listen.group = nobody|listen.group = nginx|g" /etc/php-fpm.d/www.conf
sed -i -e "s|user = apache|user = nginx|g" /etc/php-fpm.d/www.conf
sed -i -e "s|group = apache|group = nginx|g" /etc/php-fpm.d/www.conf
````

### セッションフォルダのパーミッション変更

sessionデータを保存するフォルダがnginxユーザーでも利用できるようにします。
````
chown root:nginx /var/lib/php/session
````

### timzezoneの設定

timezoneを東京（-9時間）に設定します。
````
sed -i -e "s|;date.timezone =|date.timezone = Asia/Tokyo|" /etc/php.ini
````

### php-fpm起動

php-fpmの起動と、自動起動設定を行います。
````
systemctl start php-fpm
systemctl enable php-fpm
````

### 起動確認

php-fpmが起動していることを確認します。
````
netstat -a --unix | grep php
````

### セキュリティ設定　※任意

デフォルトだとヘッダーにPHPを利用しているという情報が出力されてしまうため、表示しない設定を行います。

TODO
````
"expose_php = Off" TODO
systemctl restart php-fpm
````
