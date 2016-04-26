FROM google/debian:jessie

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    ca-certificates apt-transport-https wget

# RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 684A14CF2582E0C5 && \
#     echo "deb https://repos.influxdata.com/debian jessie stable" > /etc/apt/sources.list.d/influxdata.list && \
#     apt-get update

# ENV TELEGRAF_VERSION 0.12.1-1
# RUN apt-get install -y telegraf=$TELEGRAF_VERSION

ENV TELEGRAF_VERSION 0.12.0
RUN wget -q https://s3.amazonaws.com/get.influxdb.org/telegraf/telegraf_$TELEGRAF_VERSION-1_amd64.deb.asc && \
    wget -q https://s3.amazonaws.com/get.influxdb.org/telegraf/telegraf_$TELEGRAF_VERSION-1_amd64.deb && \
    gpg --batch --verify telegraf_$TELEGRAF_VERSION-1_amd64.deb.asc telegraf_$TELEGRAF_VERSION-1_amd64.deb && \
    dpkg -i telegraf_$TELEGRAF_VERSION-1_amd64.deb && \
    rm -f telegraf_$TELEGRAF_VERSION-1_amd64.deb*

EXPOSE 8125/udp 8092/udp 8094

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["telegraf"]