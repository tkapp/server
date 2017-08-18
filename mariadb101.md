# mariadb

## 前提条件

centos7
rootユーザー

## 手順

### firewalld穴あけ

特定のサーバーからしかアクセスされたくないので、個別のzoneを作成してアクセス設定を行います。

参考：
http://qiita.com/tomoki1207/items/44c7d19f984ae6a8df09

````
firewall-cmd --permanent --new-zone=mysql
firewall-cmd --reload

firewall-cmd --permanent --zone=mysql --set-target=ACCEPT
firewall-cmd --permanent --zone=mysql --add-service=mysql
firewall-cmd --permanent --zone=mysql --add-source=192.168.51.0/24
firewall-cmd --reload
````

### mariadbのyumリポジトリ追加

````
cat << EOF > /etc/yum.repos.d/mariadb.repo
# MariaDB 10.2 CentOS repository list - created 2017-07-20 09:28 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
enable=0
EOF
````

### インストール
````
yum -y install --enablerepo=mariadb MariaDB-server MariaDB-client
````

### 起動
````
systemctl start mysql
systemctl enable mysql
````

### 初期設定 ※任意

rootのパスワード設定、sampleスキーマ削除など行います。
````
mysql_secure_installation
````

### ログイン確認

````
mysql -uroot -p{{ パスワード }}
````
