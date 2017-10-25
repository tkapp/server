# nginxでvitualhostを設定するときのサンプル

http://{{ server_name }}/ でアクセスされたときに有効となる設定例です。

````
server {
    listen       80;
    server_name  {{ server_name }};

#    access_log  /var/log/nginx/log/{{ server_name }}/access.log  main;

    location / {
        root   {{ document_root }}
        index  index.html index.htm;
    }
}   
````
