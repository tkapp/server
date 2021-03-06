# nginxのインストール

## 概要

nginxをインストールし、webサーバーを構築します。

## 前提条件

centos7系

## 手順

ポート開放
````
firewall-cmd --permanent --zone=public --add-interface=enp0s8
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --reload
````

nginxのyumリポジトリ登録
````
cat << EOF > /etc/yum.repos.d/nginx.repo
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/rhel/\$releasever/\$basearch/
gpgcheck=0
enabled=0
EOF
````

nginxインストール
````
yum -y install --enablerepo=nginx nginx
````

サービス起動
````
systemctl start nginx
systemctl enable nginx
````

## 基本情報

基本設定ファイル: /etc/nginx/nginx.conf
自動で読み込まれる設定ファイルフォルダー: /etc/nginx/conf.d/
ドキュメントルート: /usr/share/nginx/html

続いてnginx advanceのセキュリティ対応も行うことを推奨します。
