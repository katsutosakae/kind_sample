# Debian
ARG VERSION="20-bullseye"
FROM mcr.microsoft.com/devcontainers/typescript-node:${VERSION}

# Kubernetes公式バージョン（https://kubernetes.io/ja/releases/）＋それに対応するOSSのバージョンを確認
# 20250515最新のKubernetes 1.33.1に合わせて実装
ARG KIND_VERSION="0.28.0"
ARG KUBECTL_VERSION="1.33.0"

# OSSインストール：バイナリダウンロード時は/usr/local/bin/直下配置でPATHが通るので、権限付与すればCLIで使用可能
# Kind (https://kind.sigs.k8s.io/docs/user/quick-start/)：クラスタの作成
RUN curl -Lo /usr/local/bin/kind https://kind.sigs.k8s.io/dl/v${KIND_VERSION}/kind-linux-amd64 && \
    chmod +x /usr/local/bin/kind

# kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)：クラスタの操作
RUN curl -Lo /usr/local/bin/kubectl https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
    chmod +x /usr/local/bin/kubectl
