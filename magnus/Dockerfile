FROM ubuntu:focal
WORKDIR /opt
ARG TARGETARCH \
    Version=v2.28.5
ENV Version=${Version} \
    LANG=zh_CN.UTF-8

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends wget curl gnupg2 ca-certificates lsb-release language-pack-zh-hans netcat \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "LANG=$LANG" > /etc/default/locale \
    && apt-get install -y --no-install-recommends mariadb-client bash-completion \
    && apt-get clean all \
    && rm -rf /var/lib/apt/lists/*

ARG WISP_VERSION=v0.0.10

RUN set -ex \
    && wget https://github.com/jumpserver/wisp/releases/download/${WISP_VERSION}/wisp-${WISP_VERSION}-linux-${TARGETARCH}.tar.gz \
    && tar -xf wisp-${WISP_VERSION}-linux-${TARGETARCH}.tar.gz \
    && mv wisp-${WISP_VERSION}-linux-${TARGETARCH}/wisp /usr/local/bin/ \
    && chown root:root /usr/local/bin/wisp \
    && chmod 755 /usr/local/bin/wisp \
    && rm -rf wisp-${WISP_VERSION}-linux-${TARGETARCH} \
    && wget https://github.com/jumpserver/magnus-release/releases/download/${Version}/magnus-${Version}-linux-${TARGETARCH}.tar.gz \
    && tar -xf magnus-${Version}-linux-${TARGETARCH}.tar.gz \
    && mv magnus-${Version}-linux-${TARGETARCH} /opt/magnus \
    && chmod 755 /opt/magnus/magnus \
    && chown -R root:root /opt/magnus \
    && rm -rf /opt/*.tar.gz

COPY magnus/entrypoint.sh .
RUN chmod 755 ./entrypoint.sh

CMD [ "./entrypoint.sh" ]
