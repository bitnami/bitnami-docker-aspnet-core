FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/app" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages build-essential ca-certificates curl git gzip libc6 libcap2-bin libcom-err2 libgcc1 libgssapi-krb5-2 libicu-dev libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblttng-ust-dev libsqlite3-dev libssl-dev libstdc++6 pkg-config procps tar unzip wget zlib1g
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/aspnet-core-5.0.0-0-linux-amd64-debian-10.tar.gz && \
    echo "9f9bc784c46d08c1739c3d5ffd09d59dd5c8d51829621daf1967d2080e19bbaa  /tmp/bitnami/pkg/cache/aspnet-core-5.0.0-0-linux-amd64-debian-10.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/aspnet-core-5.0.0-0-linux-amd64-debian-10.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/aspnet-core-5.0.0-0-linux-amd64-debian-10.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    0/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password

COPY rootfs /
RUN /opt/bitnami/scripts/aspnet-core/postunpack.sh
ENV BITNAMI_APP_NAME="aspnet-core" \
    BITNAMI_IMAGE_VERSION="5.0.0-debian-10-r2" \
    DOTNET_CLI_HOME="/app" \
    DOTNET_ROOT="/opt/bitnami/aspnet-core/bin" \
    PATH="/opt/bitnami/aspnet-core/bin:$PATH"

WORKDIR /app
ENTRYPOINT [ "/opt/bitnami/scripts/aspnet-core/entrypoint.sh" ]
CMD [ "/bin/bash" ]