````
yum -y install epel-release
````

````
sed -i "s/enabled=0/enabled=1/g" /etc/yum.repo.d//etc/yum.repos.d/epel.repo
````

````
yum -y --enablerepo=epel install sensors
````
