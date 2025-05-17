# 概要
kindを用いたローカルでのマルチノードkubernetes開発のために、devcontainersを作成。
一応nodeのイメージを使用しているので、内部で適当なnodeserverの作成も可能

# 中身
三種類のdevcontainerを準備。コンテナ構成の学習用に、それぞれ少し書き方を変えて作成。
- ./dind/.devcontainer
    - docker in dockerを構成。
- ./dood/.devcontainer
    - docker outside of dockerを構成。
- ./dood_compose/.devcontainer
    - docker outside of dockerを構成。docker-compose.ymlを使用。

# 補足
## dind (docker in docker)
コンテナ内部でdockerをインストールし、ホストのdockerとは独立して動作する構成。
## dood (docker outside of docker)
コンテナ内部ではdockerをインストールせずに、ホストのdockerを使用する構成。
