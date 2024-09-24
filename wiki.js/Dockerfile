ARG BUILD_FROM=ghcr.io/hassio-addons/base:15.0.8
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache \
    nodejs \
    wget \
    tar \
    nginx \
    mariadb-client \
    \
    && mkdir -p /run/nginx

WORKDIR /wiki
ARG WIKIJS_VERSION="v2.5.302"
RUN wget https://github.com/Requarks/wiki/releases/download/${WIKIJS_VERSION}/wiki-js.tar.gz

RUN mkdir -p /run/nginx
RUN tar xzf wiki-js.tar.gz -C /wiki
RUN rm wiki-js.tar.gz
RUN rm /wiki/config.sample.yml

# Copy ingress to nginx
RUN rm /etc/nginx/http.d/default.conf
COPY ingress.conf /etc/nginx/http.d/
# Copy data for add-on
COPY run.sh /
#COPY config.yml /wiki/
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]
#CMD [ "nginx","-g","daemon off;error_log /dev/stdout debug;" ]
