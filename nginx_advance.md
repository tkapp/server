# nginx advance

## 概要

nginxを運用するための

## セキュリティ設定

### バージョン情報を表示させない

ヘッダーなどに表示されるバージョン情報を非表示にします。

````
sed -i "/include/i \    server_tokens off;" /etc/nginx/nginx.conf
systemctl reload nginx
````

### TRACEメソッドの無効化

デフォルトで無効なので不要です。


### ファイル一覧の無効化

インデックスページを表示したときに、ディレクトリ内のファイル一覧の表示を無効にします。
デフォルトで無効なので不要です。

有効にする場合は nginx_autoindex.md を参照してください。


## 基本運用設定

### ログのローテーション

logrotateを利用してログをローテーションさせます。
下記は1日毎にローテーション、過去ファイルを圧縮、1週間以上前のファイルは削除する設定です。

````
cat << 'EOF' > /etc/logrotate.d/nginx
"/var/log/nginx/access.log" "/var/log/nginx/error.log" {
  missingok
  notifempty
  daily
  rotate 7
  compress
  notifempty
  sharedscripts
  postrotate
      [ ! -f /var/run/nginx.pid ] || kill -USR1 `cat /var/run/nginx.pid`
  endscript
}
EOF
````


### ログフォーマット変更

TODO

## パフォーマンスチューニング

### worker_process

workerの数を設定します。デフォルト値は1です。
基本はcore数を設定します。

````
sed -i "s/worker_processes  1;/worker_processes  {{ core数 }};/g" /etc/nginx/nginx.conf
systemctl reload nginx
````

## キャッシュ

TODO


### コンテンツのgzip圧縮

通信コストを減らすためにレスポンスデータをgzip圧縮します。

````
sed -i "s/#gzip  on/gzip  on/g" /etc/nginx/nginx.conf
systemctl reload nginx
````


このままだとhtmlしか圧縮されないため、css、jsも圧縮するよう設定を追加します。
````
sed -i "/gzip  on/a \    gzip_types text/javascript text/css application/json application/javascript;" /etc/nginx/nginx.conf
systemctl reload nginx
````

TODO
