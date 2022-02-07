# 05

## docker run とは

### run = create + start という認識

### もう一度、`docker run` が何をやっているのかを確認

```
$ docker run hello-world

Hello from Docker!
This message shows...
```

このコマンドで、hello-world の image からコンテナを立てて、デフォルトコマンドを出力した。

```
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS      PORTS   NAMES
11ae01faec57   hello-world    "/hello"   2 minutes ago    Exited (0) 2 minutes ago           dreamy_antonelli
```

status は exit

### docker create <image> を確認

```
$ docker create hello-world
af909cd13b3523bdbda6491200c646f86af87b3d23acab7dcaa9577303a06086

$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS    PORTS   NAMES
af909cd13b35   hello-world   "/hello"   4 seconds ago   Created           loving_mclaren
```

af909cd13b35 で始まる container id のコンテナが作られただけ。status も created

### docker start <container>

```
$ docker start af909cd13b35
af909cd13b35

$ docker ps -a
CONTAINER ID   IMAGE          COMMAND   CREATED          STATUS                    PORTS    NAMES
af909cd13b35   hello-world    "/hello"  4 minutes ago    Exited (0) 1 second ago            loving_mclaren
```

裏では status は up になってからデフォルトコマンドが実行されて exit になった。
デフォルトコマンドの出力結果を見るためには `-a` のオプションが必要。(業務ではこんなコマンド使わない)

```
$ docker start af909cd13b35 -a

Hello from Docker!
This message shows...
```

ちなみに `COMMAND` がデフォルトのコマンド
これは上書きできる。

ただ、hello-world の image には Ubuntu とか入ってないので bash とかは使えない。
エラーメッセージもそんな漢字のことが書いてある。

```
$ docker run -it hello-world bash
docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: exec: "bash": executable file not found in $PATH: unknown.
```

上書きに対しては以下へ。

## コマンドの上書き
