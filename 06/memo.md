# 06

## Dockerfile

Dockerfile : docker image の設計図みたいなもの

テキストファイルで書かれており、どういう image になるのかわかりやすい。可視化されている。  
そのため Dockerfile を作ってそこから image を作るパターンが多い。(コンテナから commit して image を更新するパターンは image の中身がわからないため、少ないらしい)

|![](image/dockerfile.png)
|:-:|

## Dockerfile 作成

|![](image/dockerfile-create.png)
|:-:|

```sh
# フォルダ名は何でもいい
$ mkdir docker
$ cd docker
# Dockerfile という名前でなければならない
$ touch Dockerfile
```

### Dockerfile の中身

```Dockerfile
FROM ubuntu:latest
RUN touch test
```
`ubuntu:latest` という image を取ってきて、そのコンテナの中で `touch test` というコマンドを行う
