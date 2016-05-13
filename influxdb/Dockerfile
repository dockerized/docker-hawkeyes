FROM buildpack-deps:trusty-curl
MAINTAINER Tony.Shao

ENV INFLUXDB_VERSION=influxdb_0.13.0
ENV INFLUXDB_FILE=${INFLUXDB_VERSION}_amd64.deb

ENV GOSU_VERSION 1.7
RUN set -x \
    && apt-get update && apt-get install -y --no-install-recommends ca-certificates wget && rm -rf /var/lib/apt/lists/* \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

EXPOSE 8088 8091 8083 8086

USER root

WORKDIR /tmp

RUN wget --quiet https://dl.influxdata.com/influxdb/releases/${INFLUXDB_FILE} \
      && apt-get -qq -y update \
      && dpkg -i ${INFLUXDB_FILE} \
      && apt-get autoremove \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

WORKDIR /var/lib/influxdb

ENTRYPOINT [ "tini", "--", "/entrypoint.sh"]

CMD [ "influxd" ]
