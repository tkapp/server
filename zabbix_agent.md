

# zabbix agent

## 手順

### zabbixリポジトリ登録

````
rpm -ivh http://repo.zabbix.com/zabbix/3.2/rhel/7/x86_64/zabbix-release-3.2-1.el7.noarch.rpm
sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/zabbix.repo
````

### zabbix-agentインストール
````
yum -y --enablerepo=zabbix install zabbix-agent
````

### port開放
````
firewall-cmd --permanent --new-zone=zabbix
firewall-cmd --reload

firewall-cmd --permanent --zone=zabbix --set-target=ACCEPT
firewall-cmd --permanent --zone=zabbix --add-port=10050/tcp
firewall-cmd --permanent --zone=zabbix --add-source={{ zabbix-serverのIP }}
firewall-cmd --reload
````

### 設定ファイル修正
````
sed -i "s|^Server=.*|Server={{ zabbix-serverのIP }}|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|# ListenPort=10050|ListenPort=10050|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|^ServerActive=.*|ServerActive={{ zabbix-serverのIP }}|g" /etc/zabbix/zabbix_agentd.conf
sed -i "s|^Hostname=Zabbix server|Hostname={{ zabbix-serverのホスト名 }}|g" /etc/zabbix/zabbix_agentd.conf
````

### zabbix-agent起動
````
systemctl start zabbix-agent
systemctl enable zabbix-agent
````
