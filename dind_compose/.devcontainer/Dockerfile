# Debian
ARG VERSION="20-bullseye"
FROM mcr.microsoft.com/devcontainers/typescript-node:${VERSION}

# docker-composeやdevcotainer.jsonから渡される値（FROM前に書くと渡せないhttps://qiita.com/siruku6/items/37ae25845336145b822b）
ARG KIND_VERSION
ARG KUBECTL_VERSION

# OSSインストール：バイナリダウンロード時は/usr/local/bin/直下配置でPATHが通るので、権限付与すればCLIで使用可能
# Docker (https://matsuand.github.io/docs.docker.jp.onthefly/engine/install/debian/)
## リポジトリ
RUN apt-get update && apt-get -y install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
## Docker Engine
RUN apt-get update && apt-get -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
## Docker Daemon起動用shell準備
COPY dockerd-entrypoint.sh /usr/local/bin/dockerd-entrypoint.sh
RUN chmod +x /usr/local/bin/dockerd-entrypoint.sh

# Kind (https://kind.sigs.k8s.io/docs/user/quick-start/)：クラスタの作成
RUN curl -Lo /usr/local/bin/kind https://kind.sigs.k8s.io/dl/v${KIND_VERSION}/kind-linux-amd64 && \
    chmod +x /usr/local/bin/kind

# kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)：クラスタの操作
RUN curl -Lo /usr/local/bin/kubectl https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
    chmod +x /usr/local/bin/kubectl
