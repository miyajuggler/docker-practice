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
# src ... ファイルのこと？
# dest ... 階層
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

- 単純にファイルやフォルダをコピーする場合は COPY
- tar の圧縮ファイルをコピーして解凍したいときは ADD

大きいファイルを image に入れたいとき、普通にファイルを build context に置いて COPY する場合、docker build 時に時間がかかってしまう。そういうときはファイルを圧縮しておいて ADD を使用する。
ADD は自動で解凍もしてくれる。

|![](image/add.png)
|:-:|

準備

```sh
$ mkdir sample_folder
$ cd sample_folder
$ echo "hello world" > hello
$ cd ..
$ tar -cvf sample.tar sample_folder
```

Dockerfile 準備

```Dockerfile
FROM ubuntu:latest
ADD sample.tar /
```

docker build

```
$ docker build .
[+] Building 2.4s (8/8) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 78B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/ubuntu:latest
 => [auth] library/ubuntu:pull token for registry-1.docker.io
 => [internal] load build context
 => => transferring context: 2.60kB
 => CACHED [1/2] FROM docker.io/library/ubuntu:latest@sha256:669e010b58baf5beb2836b253c1fd5768333f0d1dbcb834f7c07a4dc93f474be
 => [2/2] ADD sample.tar /
 => exporting to image
 => => exporting layers
 => => writing image sha256:18049651cb168b186afb3a04030fd215e448d8b5b735027abd57a4757dd1101c

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```

確認

```
$ docker run -it --rm 18049651cb168b186afb3a04030fd215e448d8b5b735027abd57a4757dd1101c bash
root@ea050b308d3c:/# cat /sample_folder/hello
hello world
root@ea050b308d3c:/# ls
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sample_folder  sbin  srv  sys  tmp  usr  var
root@ea050b308d3c:/# cd sample_folder/
root@ea050b308d3c:/sample_folder# ls
hello
```

## Dockerfile が build context にない場合

-f で Dockerfile を指定することはよくある。  
これにより Dockerfile と build context を分けて管理できる。

```
$ docker build -f <Dockerfile 名> <build context>
```

|![](image/docker.png)
|:-:|

このように Dockerfile が複数存在し、開発用、テスト用、本番用などの Dockerfile があって使い分けたい場合は、-f で Dockerfile を指定して build してあげるケースがある。

準備

```sh
$ cd sample_folder
$ ls
hello
$ mv ../Dockerfile ../Dockerfile.dev
```

Dockerfile

```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
    curl \
    cvs \
    nginx
CMD [ "/bin/bash" ]
```

docker build

```
$ docker build -f ../Dockerfile.dev .
[+] Building 2.1s (7/7) FINISHED
 => [internal] load build definition from Dockerfile.dev
 => => transferring dockerfile: 159B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/ubuntu:latest
 => [auth] library/ubuntu:pull token for registry-1.docker.io
 => [1/2] FROM docker.io/library/ubuntu:latest@sha256:669e010b58baf5beb2836b253c1fd5768333f0d1dbcb834f7c07a4dc93f474be
 => CACHED [2/2] RUN apt-get update && apt-get install -y     curl     cvs     nginx
 => exporting to image
 => => exporting layers
 => => writing image sha256:0bb607faf74b7dccb45f64540044aa8847d21a2035d63b8126bc698e7646989e
```
