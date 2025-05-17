# Debian
ARG VERSION="20-bullseye"
FROM mcr.microsoft.com/devcontainers/typescript-node:${VERSION}

# docker-composeやdevcotainer.jsonから渡される値（FROM前に書くと渡せないhttps://qiita.com/siruku6/items/37ae25845336145b822b）
ARG KIND_VERSION
ARG KUBECTL_VERSION

# OSSインストール：バイナリダウンロード時は/usr/local/bin/直下配置でPATHが通るので、権限付与すればCLIで使用可能
# Kind (https://kind.sigs.k8s.io/docs/user/quick-start/)：クラスタの作成
RUN curl -Lo /usr/local/bin/kind https://kind.sigs.k8s.io/dl/v${KIND_VERSION}/kind-linux-amd64 && \
    chmod +x /usr/local/bin/kind

# kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)：クラスタの操作
RUN curl -Lo /usr/local/bin/kubectl https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
    chmod +x /usr/local/bin/kubectl
