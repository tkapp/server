# zabbix agent構築

## 概要

zabbix agentをインストールし、ホスト機の監視を行えるようにします。

## 前提

centos7
zabbix serverのインストール

## 手順

### zabbixリポジトリ登録

yumでインストールするためのリポジトリを登録します。

````
rpm -ivh http://repo.zabbix.com/zabbix/3.2/rhel/7/x86_64/zabbix-release-3.2-1.el7.noarch.rpm
sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/zabbix.repo
````

### zabbix-agentインストール

yumコマンドでzabbix-agentをインストールします。

````
yum -y --enablerepo=zabbix install zabbix-agent
````

### port開放

firewalldの設定を行い、zabibx-serverと通信できるようにします。

````
firewall-cmd --permanent --new-zone=zabbix
firewall-cmd --reload

firewall-cmd --permanent --zone=zabbix --set-target=ACCEPT
firewall-cmd --permanent --zone=zabbix --add-port=10050/tcp
firewall-cmd --permanent --zone=zabbix --add-source={{ zabbix-serverのIP }}
firewall-cmd --reload
````

### 設定ファイル修正

zabbix-serverとの通信用設定を行います。

````
sed -i "s|^Server=.*|Server={{ zabbix-serverのIP }}|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|# ListenPort=10050|ListenPort=10050|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|^ServerActive=.*|ServerActive={{ zabbix-serverのIP }}|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|^Hostname=Zabbix server|Hostname={{ zabbix-serverのホスト名 }}|g" /etc/zabbix/zabbix_agentd.conf
````

### zabbix-agent起動

zabbix-agentの起動と、自動起動設定を行います。

````
systemctl start zabbix-agent
systemctl enable zabbix-agent
````
