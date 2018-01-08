# 2018/01/18 広島KKB Dockerハンズオン

## Docker導入

### Mac

https://www.docker.com/docker-mac

### Windows

https://www.docker.com/docker-windows

### Linux(CentOS7)

https://qiita.com/zoe302/items/307327da5c75628daadb

## とりあえず動かしてみる

~~~
$ cd ~/
$ git clone https://github.com/daioyoshikawa/hkkb-180109-start-docker.git start-docker-wp
$ cd start-docker-wp
$ docker-compose up -d
~~~

### 動作確認

[localhost:8000]()


## 参考
- https://tech.recruit-mp.co.jp/infrastructure/post-13086/#ampshare=https://tech.recruit-mp.co.jp/infrastructure/post-13086/
- https://tech.recruit-mp.co.jp/infrastructure/post-12795/
- https://joppot.info/2017/05/12/3977
- https://github.com/github/gitignore/blob/master/WordPress.gitignore