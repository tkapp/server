# proxy環境の設定

## 前提

centos7
bash

## OS設定

````
echo "export http_proxy=http://{{ IP }}:{{ port }}" >> /etc/profile
echo "export https_proxy=http://{{ IP }}:{{ port }}" >> /etc/profile
source /etc/profile
````

## yum

````
echo "proxy={{ IP }}:{{ port }}" >> /etc/yum.conf
````

## git

````
git config --global http.proxy http://{{ IP }}:{{ port }}:8080
git config --global https.proxy http://{{ IP }}:{{ port }}:8080
````
