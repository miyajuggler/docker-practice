# docker build の詳細と、その他の instruction

## docker build は何をしているのか

docker build をすると Dockerfile のあるフォルダを build context として docker daemon に渡し、それをもとに image を作ってくれる。

## docker daemon

docker daemon は docker object(container, image, network, volume など)を管理していて、我々は docker CLI を用いて docker daemon とコミュニケーションを取っていた。

## build context

|![](image/buildcontext.png)
|:-:|

## COPY

|![](image/copy.png)
|:-:|

build context の中にあるファイルを image の中に持っていける。  
Dockerfile の書き方は以下。

```Dockerfile
# src
# dest
COPY <src> <dest>
```

/new_dir というディレクトリを作り、そこにホストにある something というファイルを image の中に持っていきたい。

Dockerfile 例  
どうやら `/new_dir` でも `new_dir` でもいけるみたい

```Dockerfile
FROM ubuntu:latest
RUN mkdir /new_dir
COPY something /new_dir
```

docker build する

```sh
$ docker build .
[+] Building 2.4s (9/9) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 104B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/ubuntu:latest
 => [auth] library/ubuntu:pull token for registry-1.docker.io
 => CACHED [1/3] FROM docker.io/library/ubuntu:latest@sha256:669e010b58baf5beb2836b253c1fd5768333f0d1dbcb834f7c07a4dc93f474be
 => [internal] load build context
 => => transferring context: 30B
 => [2/3] RUN mkdir /new_dir
 => [3/3] COPY something /new_dir
 => exporting to image
 => => exporting layers
 => => writing image sha256:23aa3ebb74b05865e3e428cd47143547f3210623f3d870879ce68df9cd1492fe
```

コンテナを立てて中身を確認

```
$ docker run -it 23aa3ebb74b0 bash
root@54a67bcac53b:/# ls
bin  boot  dev  etc  home  lib  media  mnt  new_dir  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@54a67bcac53b:/# ls /new_dir
something
```

## ADD vs COPY
