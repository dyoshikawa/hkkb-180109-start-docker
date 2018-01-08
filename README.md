# 2018/01/18 広島KKB Dockerハンズオン

## Docker導入

### Mac

- https://www.docker.com/docker-mac

### Windows

- https://www.docker.com/docker-windows

### Linux(CentOS7)

- https://qiita.com/zoe302/items/307327da5c75628daadb

## とりあえずDockerでWPを構築してみる

### Dockerコンテナ群の立ち上げと連携

~~~
$ cd ~/
$ git clone https://github.com/daioyoshikawa/hkkb-180109-start-docker.git start-docker-wp
$ cd start-docker-wp
$ docker-compose up -d
~~~

### 動作確認

#### ブラウザで以下をアドレスバーに入力

~~~
localhost:8000
~~~

- 初期設定画面の表示を確認

#### WordPress初期設定

- データベース名: wordpress
- ユーザー名: root
- パスワード: password
- データベースのホスト名: mysql
- テーブル接頭辞: wp_

### Dockerイメージ, コンテナ, ネットワーク全部消す

- PC内のDockerイメージ, コンテナ, ネットワークが全て消える
- すでにDockerユーザの方は注意して下さい

~~~
$ pwd
~/start-docker-wp
$ sh docker-destroy.sh
~~~

## 他にも色々試してみる――Dockerコマンド入門

### DockerHub

- https://hub.docker.com/
- 検索して自分の欲しいイメージを探す
  - 素のLinuxOSイメージ(CentOS, Ubuntu, Alpine...)
  - 言語やアプリケーション入りイメージ(PHP, Ruby, Nginx, MySQL...)
  - Docker社作成の公式イメージとそれ以外の人が作った非公式イメージがある(誰でもアップできる)

### よく使うDockerコマンド, オプション

#### (非常に)ざっくりとした説明

- まずイメージを取ってくる
- イメージからコンテナを作る
- たい焼きの例え(雑)
  - イメージ=たい焼き機
  - コンテナ=たい焼き

#### イメージを取ってくる

~~~
$ docker pull イメージ名
~~~

~~~
$ docker pull centos:7 
~~~

#### イメージからコンテナを作って動かす

- 単に作って動かす

~~~
$ docker run -d イメージ名
~~~

~~~
$ docker run -d -p 8000:80 httpd:alpine
~~~

- 作って動かしつつ中に入る

~~~
$ docker run -ti イメージ名 sh(or bash or etc...)
~~~

~~~
$ docker run -ti centos:7 bash
$ docker run -ti alpine:latest sh
~~~

- 動いてるコンテナを確認する

~~~
$ docker ps
~~~

- (動いてないコンテナを含めた)すべてのコンテナを確認する

~~~
$ docker ps -a
~~~

- すでに動いてるコンテナに入る

~~~
$ docker exec -ti コンテナ名(or コンテナID) sh(or bash or etc...)
~~~

~~~
$ docker run --name orenomysql -d -e MYSQL_ROOT_PASSWORD=password mysql:5.7
$ docker exec -ti orenomysql bash
# mysql -uroot -ppassword
~~~

~~~
$ docker run --name bokunomysql -d -e MYSQL_ROOT_PASSWORD=password mysql:5.7
$ docker exec -ti bokunomysql mysql -uroot -ppassword
~~~

#### コンテナの保存

- コンテナは保存できない(消えると原状復帰できない)
- docker commit コマンドでイメージにして保存する
  - docker push コマンドでクラウド(DockerHubなど)に上げることもできる(今日は触れません)

~~~
$ docker commit コンテナ名(or コンテナID) イメージ名:タグ(任意に命名)
~~~

- PCに入ってるイメージ一覧を確認する

~~~
$ docker images
~~~

#### 停止, 削除コマンド

- コンテナ停止

~~~
$ docker stop コンテナ名(or コンテナID)
~~~

- コンテナ削除

~~~
$ docker rm コンテナ名(or コンテナID)
~~~

- イメージ削除

~~~
$ docker rmi イメージ名:タグ
~~~

## 自分でイメージを作ってみる――Dockerfile, DockerCompose入門

### Dockerfile?

- (非常にざっくり説明すると)「動く手順書」
  - 「動く」「手順書」
  - 「動く」ので待ってるだけで環境構築が終わるので楽
  - 「手順書」なので後で見直した時にインフラ構成が分かる
    - Infrastracture as Code と言ったりするらしい

### その1 コンソールでHelloWorld

~~~
$ cd ~/
$ mkdir my-docker-test
$ cd my-docker-test
$ mkdir hello
$ touch hello/Dockerfile
~~~

- 作成したDockerfileを任意のエディタで編集
- 今回のハンズオンではAlpineイメージを使っていきます
  - Alpine?→超軽量のLinuxディストリビューション
  - 軽いのでpullが早い
  - bashじゃなくてsh(bashのパッケージインスールは可)
  - パッケージインストールは「apk」コマンド

~~~
FROM alpine:latest

ENTRYPOINT ["echo", "HelloWorld"]
~~~

- 編集して保存したらイメージビルドする
- docker build コマンド

~~~
$ docker build -t イメージ名:タグ(任意) Dockerfileの入ったディレクトリ名
~~~

~~~
$ pwd
~/my-docker-test
$ docker build -t hello:latest hello
$ docker run hello
~~~

- HelloWorldが出力されたら成功

### Dockerfileコマンド

- よく使うコマンド

|コマンド|概説|
|:---|:---|
|FROM|元にするイメージを指定|
|RUN|sh, bashコマンドを実行|
|ADD|ファイルをコンテナに注入|
|COPY|ほぼ同上|
|ENTRYPOINT|コンテナ起動時に実行するコマンドを指定|
|CMD|ほぼ同上|

- ほぼ同上が気になる方は調べてみて下さい
  - そして僕に教えて下さい・・・
  - 参考になりそうなリンク
    - https://qiita.com/hihihiroro/items/0956326d6731bc927166
    - https://qiita.com/YmgchiYt/items/ad7783ab0757f3ffd929

### その2 ブラウザでHelloWorld

~~~
$ pwd
~/my-docker-test
$ mkdir hello-nginx
$ touch hello-nginx/Dockerfile
$ touch hello-nginx/index.html
~~~

- Dockerfile

~~~
FROM nginx:alpine

ADD index.html /usr/share/nginx/index.html

ENTRYPOINT nginx
~~~

- index.html

~~~
<!DOCTYPE HTML>
<html>
    <head>
    </head>
    <body>
        HelloWorld
    </body>
</html>
~~~

- イメージビルドする

~~~
$ docker build -t hello-nginx hello-nginx
~~~

- コンテナ起動

~~~
$ docker run -d -p 8000:80 hello-nginx
~~~

- ボリュームマウントしてみる
  - ホスト側の編集がコンテナに反映される

~~~
$ docker run -v `pwd`/index.html:/usr/share/nginx/index.html -p 8000:80 hello-nginx
~~~

- docker-compose で起動してみる

~~~
$ pwd
~/my-docker-test
$ touch docker-compose.yml
~~~

- docker-compose.yml

~~~
version: '3'
services:
  nginx:
    build:
      context: ./hello-nginx
      dockerfile: Dockerfile
    container_name: hello-nginx
    volumes:
      - ./index.html:/usr/share/nginx/index.html
    ports:
      - "8000:80"
~~~

- docker-compose でコンテナ起動

~~~
$ docker-compose up -d
~~~

- docker-compose でコンテナ停止

~~~
$ docker-compose stop
~~~

- docker-compose でコンテナ削除

~~~
$ docker-compose down
~~~

## 参考

- https://tech.recruit-mp.co.jp/infrastructure/post-13086/
- https://tech.recruit-mp.co.jp/infrastructure/post-12795/
- https://joppot.info/2017/05/12/3977
- https://github.com/github/gitignore/blob/master/WordPress.gitignore
- https://qiita.com/ka215/items/a709665cb34c505ccf1f