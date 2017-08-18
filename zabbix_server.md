# zabbix server

## 前提条件条件

centos7
nginx構築済み
php-fpm構築済み
mariadb server構築済み

## 手順

### schema作成

mariadbにログインし、zabbix用のスキーマを作成しておきます。
````
create database zabbix default character set 'utf8';
grant all on zabbix.* to zabbix@"{{ mariadb serverのIP }}" identified by '{{ zabbix_DB_パスワード }}';
````

### firewalld穴あけ

mysql側でzabbix-serverからアクセスするための穴あけを行います。

mariadb構築#firewalld穴あけを参照


### zabbix serverインストール

yumリポジトリ追加
````
rpm -ivh http://repo.zabbix.com/zabbix/3.2/rhel/7/x86_64/zabbix-release-3.2-1.el7.noarch.rpm
````

デフォルトでは利用しないように設定変更
````
sed -i "s/enabled=0/enabled=1/g" /etc/yum.repo.d//etc/yum.repos.d/zabbix.repo
````

yumでインストール
````
yum install zabbix-server-mysql zabbix-web-mysql
````

### テーブル登録
````
zcat /usr/share/doc/zabbix-server-mysql-3.2.7/create.sql.gz | mysql -uzabbix -{{ zabbix_DB_パスワード }} zabbix
````

### 設定ファイル編集

````
sed -i -e "s|# DBHost=localhost|DBHost={{ mariadb serverのIP }}|g" /etc/zabbix/zabbix_server.conf
sed -i -e "s|# DBSchema=|DBSchema=zabbix|g" /etc/zabbix/zabbix_server.conf
sed -i -e "s|# DBUser=|DBUser=zabbix|g" /etc/zabbix/zabbix_server.conf
sed -i -e "s|# DBPassword=|DBPassword={{ zabbix_DB_パスワード }}|g" /etc/zabbix/zabbix_server.conf
sed -i -e "s|# DBPort=3306|DBPort=3306|g" /etc/zabbix/zabbix_server.conf
````

### zabbix server 起動
````
systemctl start zabbix-server
systemctl enable zabbix-server
````

### phpの設定変更

zabbixが要求する設定を入れます。

````
sed -i -e "s|post_max_size = 8M|post_max_size = 16M|" /etc/php.ini
sed -i -e "s|max_execution_time = 30|max_execution_time = 300|" /etc/php.ini
sed -i -e "s|max_input_time = 60|max_input_time = 300|" /etc/php.ini
````

### nginxとの連携設定


### nginx再起動

````
systemctl restart nginx
````

### ログイン確認

初期のID、パスワード
````
admin/zabbix
````
