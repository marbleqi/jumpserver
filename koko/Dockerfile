FROM ubuntu:focal
WORKDIR /opt
ARG TARGETARCH \
    Version=v2.28.5
ENV Version=${Version} \
    LANG=zh_CN.UTF-8

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends wget curl gnupg2 ca-certificates lsb-release language-pack-zh-hans netcat software-properties-common \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "LANG=$LANG" > /etc/default/locale \
    && echo "deb https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" > /etc/apt/sources.list.d/mongodb-org-6.0.list \
    && wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | apt-key add - \
    && add-apt-repository -y ppa:redislabs/redis \
    && apt-get update \
    && apt-get install -y --no-install-recommends openssh-client mariadb-client bash-completion redis-tools mongodb-mongosh \
    && apt-get clean all \
    && rm -rf /var/lib/apt/lists/*

RUN set -ex \
    && wget https://github.com/jumpserver/koko/releases/download/${Version}/koko-${Version}-linux-${TARGETARCH}.tar.gz \
    && tar -xf koko-${Version}-linux-${TARGETARCH}.tar.gz \
    && mv koko-${Version}-linux-${TARGETARCH} koko \
    && mv /opt/koko/kubectl /usr/local/bin/ \
    && mv /opt/koko/helm /usr/local/bin/ \
    && chown root:root /usr/local/bin/kubectl /usr/local/bin/helm \
    && wget https://download.jumpserver.org/public/kubectl-linux-${TARGETARCH}.tar.gz \
    && tar -xf kubectl-linux-${TARGETARCH}.tar.gz \
    && chmod 755 kubectl \
    && mv kubectl /usr/local/bin/rawkubectl \
    && wget https://download.jumpserver.org/public/helm-v3.9.0-linux-${TARGETARCH}.tar.gz -O helm.tar.gz \
    && tar -xf helm.tar.gz \
    && mv linux-${TARGETARCH}/helm /usr/local/bin/rawhelm \
    && chmod 755 /usr/local/bin/rawhelm \
    && chown root:root /usr/local/bin/rawhelm \
    && rm -rf linux-${TARGETARCH} \
    && wget http://download.jumpserver.org/public/kubectl_aliases.tar.gz \
    && mkdir /opt/kubectl-aliases/ \
    && tar -xf kubectl_aliases.tar.gz -C /opt/kubectl-aliases/ \
    && chown -R root:root /opt/kubectl-aliases/ \
    && chmod 755 /opt/koko/init-kubectl.sh \
    && rm -rf /opt/*.tar.gz

COPY koko/entrypoint.sh .
RUN chmod 755 ./entrypoint.sh

CMD [ "./entrypoint.sh" ]
