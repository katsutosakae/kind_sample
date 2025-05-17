# 概要
kindを用いたローカルでのマルチノードkubernetes開発のために、devcontainersを作成。\
一応nodeのイメージを使用しているので、内部で適当なnodeserverの作成も可能

# 中身
三種類のdevcontainerを準備。コンテナ構成の学習用に、それぞれ少し書き方を変えて作成。
- ./dind/.devcontainer（推奨）
    - docker-in-dockerを構成。
- ./dind_compose/.devcontainer（現在使用不可）
    - docker-in-dockerを構成。docker-compose.ymlを使用。
    - 手動でdocker環境構築しているが、docker daemonの起動＋docker.socketの作成で詰まり中。
- ./dood_compose/.devcontainer（kind使用不可）
    - docker-outside-of-dockerを構成。docker-compose.ymlを使用。

# 補足
## dind (docker-in-docker)
コンテナ内部でdockerをインストールし、ホストのdockerとは独立して動作する構成。
## dood (docker-outside-of-docker)
コンテナ内部ではdockerをインストールせずに、ホストのdockerを使用する構成。\
今回doodを使用しなかった理由は、 `kind create cluster`実行時に下記エラーが発生するため。
```
node ➜ /workspaces $ kind create cluster --image kindest/node:v1.32.0
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.32.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✗ Starting control-plane 🕹️ 
Deleted nodes: ["kind-control-plane"]
ERROR: failed to create cluster: failed to remove control plane taint: command "docker exec --privileged kind-control-plane kubectl --kubeconfig=/etc/kubernetes/admin.conf taint nodes --all node-role.kubernetes.io/control-plane-" failed with error: exit status 1
Command Output: E0517 02:25:28.067661     518 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"https://kind-control-plane:6443/api?timeout=32s\": dial tcp 172.18.0.2:6443: connect: connection refused"
E0517 02:25:28.069300     518 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"https://kind-control-plane:6443/api?timeout=32s\": dial tcp 172.18.0.2:6443: connect: connection refused"
The connection to the server kind-control-plane:6443 was refused - did you specify the right host or port?
```
原因はあんまり理解できてないが、何故かdocker-outside-of-dockerの場合のみDNS解決が遅延しコントロールプレーンのtaintが削除されるためらしい。([参考ISSUE](https://github.com/kubernetes-sigs/kind/issues/2867))\
上記ISSUEでは、コード内部でtaint削除直前にsleepを追加することによって、DNS解決の遅延による影響に対処していたが推奨されない方法。