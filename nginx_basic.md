# nginxでbasic認証

## 概要

特定のディレクトリに対し、basic認証をかけ、特定のユーザーのみアクセスできるようにします。

## 前提

nginxのインストールが完了していること。

## 手順

###

.htpasswd ファイルを作るためのツールをインストールします。

````
yum install -y httpd-tools
````

### .htpasswd ファイルの作成

認証用のファイルを作成します。

````
htpasswd -c /etc/nginx/.htpasswd {{ ユーザー名 }}
New password: {{ パスワード }}
Re-type new password: {{ パスワード }}
Adding password for user username
````

### nginx設定変更

下記の設定をnginxの設定ファイルに追加します。
http、server、locationディレクティブのどこでも利用できるため、適用したい範囲に応じて追加する場所を決めます。

````
auth_basic "Restricted";
auth_basic_user_file /etc/nginx/.htpasswd;
````

Restricted部分は任意に変更可能です。basic認証の画面で表示されます。
