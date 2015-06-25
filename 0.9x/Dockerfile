FROM java:8-jdk
MAINTAINER Roberto Quintanilla <roberto.quintanilla@gmail.com>

# Set the JAVA_HOME, HBase version and the Hadoop major version:
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
  HBASE_VERSION=0.98.13 \
  HBASE_DOWNLOAD_SHA256=16bd573b2989a996c1cef7003ab55fba50a744ce3875a360717d476ead0d68ab

# Fetch HBase:
# - Install supervisord to run hbase master + thrift (see https://docs.docker.com/articles/using_supervisord/)
RUN apt-get update \
  && apt-get install -y supervisor --no-install-recommends \
  && mkdir -p /hbase \
  && curl -fSL -o hbase.tar.gz "http://apache.webxcreen.org/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-hadoop2-bin.tar.gz" \
  && echo "$HBASE_DOWNLOAD_SHA256 *hbase.tar.gz" | sha256sum -c - \
	&& tar -xzf hbase.tar.gz -C /hbase --strip-components=1 \
	&& rm hbase.tar.gz \
  && rm -rf /var/lib/apt/lists/*

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

ENV PATH /hbase/bin:$JAVA_HOME/bin:$PATH

# Exposed Ports: ---------------------------------------------------------------
#                | Master | Master | Regionserver | Regionserver |
#      Zookeeper | API    | Web UI | API          | Web UI       | Thrift
EXPOSE 2181        60000    60010    60020          60030          9090

WORKDIR /hbase

# Define default command.
CMD ["/usr/bin/supervisord"]
