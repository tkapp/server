# ruby 2.4

## 前提条件

centos7

## 手順

ソースを取得し、configureしてmakeしてmake installします。
````
mkdir ~/work
cd ~/work

wget https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.gz
tar -zxvf ruby-2.4.1.tar.gz

cd ruby-2.4.1

./configure
make
make install

ruby -v
````

bundlerもインストール
````
gem install bundler --no-rdoc --no-ri
````
