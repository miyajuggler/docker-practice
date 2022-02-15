# docker build の詳細と、その他の instruction

## docker build は何をしているのか

docker build をすると Dockerfile のあるフォルダを build context として docker daemon に渡し、それをもとに image を作ってくれる。
