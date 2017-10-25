# epelのyumリポジトリ登録

## epelとは

CentOS標準にはないパッケージを提供するリポジトリです。。
エンタープライズ向けのリポジトリなため、他のリポジトリと比べると信頼性が高いため、よく利用されています。

## 手順

###  epelのyumリポジトリ登録
````
yum -y install epel-release
````

### デフォルトでepelリポジトリを見に行かないよう修正。※任意
````
sed -i "s/enabled=0/enabled=1/g" /etc/yum.repo.d/etc/yum.repos.d/epel.repo
````

上記を行った場合は、下記の要領でepelリポジトリからパッケージを取得できます。
````
yum -y --enablerepo=epel install sensors
````
