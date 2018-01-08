# 2018/01/18 広島KKB Dockerハンズオン

## Docker導入

### Mac

https://www.docker.com/docker-mac

### Windows

https://www.docker.com/docker-windows

### Linux(CentOS7)

https://qiita.com/zoe302/items/307327da5c75628daadb

## とりあえず動かしてみる

### Dockerコンテナ群の立ち上げと連携

~~~
$ cd ~/
$ git clone https://github.com/daioyoshikawa/hkkb-180109-start-docker.git start-docker-wp
$ cd start-docker-wp
$ docker-compose up -d
~~~

### 動作確認

1. ブラウザで以下をアドレスバーに入力

~~~
localhost:8000
~~~

- 初期設定画面の表示を確認

2. WordPress初期設定

- データベース名: wordpress
- ユーザー名: root
- パスワード: password
- データベースのホスト名: mysql
- テーブル接頭辞: wp_

## 参考
- https://tech.recruit-mp.co.jp/infrastructure/post-13086/
- https://tech.recruit-mp.co.jp/infrastructure/post-12795/
- https://joppot.info/2017/05/12/3977
- https://github.com/github/gitignore/blob/master/WordPress.gitignore