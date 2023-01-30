Dockerfileを使わずDockerHubのイメージをそのまま使うパターン
python環境を構築し、ホストからソース編集できるようにします。
https://hub.docker.com/_/python

1. フォルダ構成
src/
src/main.py
docker-compose.yml
requirements.txt

2. 起動方法
docker-compose.ymlのディレクトリに移動
docker-compose up -d

3. ymlの解説
version: "3.7"

# Dockerfileを使わないパターン
services:
  python3.9:
    # DockerHubのイメージ
    image: python:3.9
    # requirements.txtからパッケージインストール
    command: bash -c "pip install --no-cache-dir -r requirements.txt && /bin/bash"
    working_dir: /usr/src
    # ホストのカレントをコンテナの/usr/srcと同期する
    volumes:
      - ./:/usr/src
    # ttyとcommandを共存するには/bin/bashが必要？
    tty: true


4. 使い方
srcにファイルを作成していきます。
docker exec -it [コンテナ名] python src/main.pyで実行できます。
もしくは
docker exec -it [コンテナ名] bashでコンテナに入って実行します。
docker-compose stop で終了します。

今回はあくまで最低限の環境なのでOSの設定などを細かくする場合は、
Dockerfileを使うほうが良いかもしれません。
