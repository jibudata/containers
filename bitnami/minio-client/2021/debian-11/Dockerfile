FROM registry.cn-shanghai.aliyuncs.com/jibutech/minideb:bullseye

ARG TARGETARCH

# https://dl.minio.io/client/mc/release/linux-amd64/archive/mc.RELEASE.2021-12-10T00-14-28Z
# https://dl.minio.io/client/mc/release/linux-arm64/archive/mc.RELEASE.2021-12-10T00-14-28Z
LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.ref.name="2021.12.10-debian-11-r2" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/minio-client" \
      org.opencontainers.image.title="minio-client" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="2021.12.10"

ENV HOME="/" \
    OS_ARCH="${TARGETARCH:-amd64}" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl procps
RUN apt-get autoremove --purge -y curl && \
    apt-get update && apt-get upgrade -y && \
    apt-get clean && rm -rf /var/lib/apt/lists /var/cache/apt/archives

RUN mkdir -p /opt/bitnami/minio-client/bin
COPY linux-${OS_ARCH}/mc /opt/bitnami/minio-client/bin/mc

#https://github.com/tianon/gosu/releases
RUN mkdir -p /opt/bitnami/common/bin
COPY linux-${OS_ARCH}/gosu /opt/bitnami/common/bin/gosu

RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/minio-client/postunpack.sh
ENV APP_VERSION="2021.12.10" \
    BITNAMI_APP_NAME="minio-client" \
    PATH="/opt/bitnami/minio-client/bin:/opt/bitnami/common/bin:$PATH"

WORKDIR /opt/bitnami/minio-client
USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/minio-client/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/minio-client/run.sh" ]
