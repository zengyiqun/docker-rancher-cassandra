FROM debian:jessie-backports

RUN echo 'deb http://httpredir.debian.org/debian jessie-backports main' >> /etc/apt/sources.list.d/openjdk8.list
RUN echo 'deb http://www.apache.org/dist/cassandra/debian 311x main' >> /etc/apt/sources.list.d/cassandra.list

RUN apt-key adv --keyserver pool.sks-keyservers.net --recv-key A278B781FE4B2BDA

ENV CASSANDRA_VERSION 3.11.3

RUN apt-get update \
	&& apt-get -t jessie-backports install -y openjdk-8-jre-headless \
	&& apt-get install -y ntp \
	&& apt-get install -y curl cassandra="$CASSANDRA_VERSION" \
	&& rm -rf /var/lib/apt/lists/*

ENV CASSANDRA_CONFIG /etc/cassandra

ADD docker-entrypoint.sh /docker-entrypoint.sh

RUN chmod +777 /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

VOLUME /var/lib/cassandra

# 7000: intra-node communication
# 7001: TLS intra-node communication
# 7199: JMX
# 9042: CQL
# 9160: thrift service
EXPOSE 7000 7001 7199 9042 9160
CMD ["cassandra", "-f"]
