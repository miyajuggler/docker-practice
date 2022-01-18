## `dockerhub` から image を pull

### コマンド

```
$ docker login

$ docker pull <image>

$ docker images
```

### ログイン

```
docker login
```

ユーザーネームとパスワードを聞かれるので記入

### hello-world を pull する

```
$ docker pull hello-world
```

<details>

```
$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
93288797bd35: Pull complete
Digest: sha256:975f4b14f326b05db86e16de00144f9c12257553bba9484fed41f9b6f2257800
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

</details>

### 確認

`docker image` だけだと全件取得してくる。

もしローカルにたくさん image がある場合、`hello-world` をつけてフィルタリングする。

```
$ docker images hello-world
REPOSITORY      TAG         IMAGE ID       CREATED         SIZE
hello-world     latest      18e5af790473   3 months ago    9.14kB
```

`REPOSITORY` はリポジトリの名前。

`TAG` は image のバージョンみたいなもの。指定しないと latest をとってくる

まとめると、今回は `library/hello-world` というリポジトリから `latest` というタグの image をとってきた

https://hub.docker.com/u/library

https://hub.docker.com/_/hello-world

## image からコンテナ作成

### コマンド

```
$ docker run <image>

$ docker ps -a
```

`hello-world` の `image` を起動

```
$ docker run hello-world
```

<details>

```
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

</details>

### 確認

`docker ps` だと稼働中のコンテナを全件取得してくる。

`docker ps -a` だと終了したコンテナも含めて全件取得してくる。

もしローカル環境にたくさんのコンテナがある場合は、以下のようなコマンドでフィルタリングする。

```
$ docker ps -a -f "ancestor=hello-world"
CONTAINER ID  IMAGE        COMMAND   CREATED         STATUS                     PORTS     NAMES
dff3f6a0f582  hello-world  "/hello"  12 minutes ago  Exited (0) 12 minutes ago            friendly_wright
```

`hello-world` にはテキストを出力するプログラムが入ってるらしい。

`docker run` をすることで `hello-world` の image からコンテナが作られて、テキスト出力が実行されて、exit した感じ。
