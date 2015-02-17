FROM vovimayhem/java:oracle-java7
MAINTAINER Roberto Quintanilla <roberto.quintanilla@gmail.com>

# Add the hbase user and group first to make sure their IDs get assigned
# consistently (we like it to be 1000), regardless of whatever dependencies get
# added:
RUN cp /etc/default/useradd /etc/default/useradd.bak \
  && echo "HOME=" >> /etc/default/useradd \
  && useradd --create-home --shell /usr/sbin/nologin hbase \
  && rm -rf /etc/default/useradd \
  && mv /etc/default/useradd.bak /etc/default/useradd \
  && rm /hbase/.bash_logout /hbase/.bashrc /hbase/.profile

# Set the HBase version and the Hadoop major version:
ENV HBASE_VERSION=0.98.10.1 HADOOP_MAJOR_VERSION=2

# Fetch HBase:
# - Install curl so we can fetch the HBase code:
RUN apt-get update \
  && apt-get install -y curl --no-install-recommends \
  && curl -SL "http://apache.webxcreen.org/hbase/stable/hbase-$HBASE_VERSION-hadoop$HADOOP_MAJOR_VERSION-bin.tar.gz" \
  | tar -xzC /hbase --strip-components=1 \
  && apt-get remove -y curl && apt-get -y autoremove \
  && rm -rf /var/lib/apt/lists/*

ADD hbase-site.xml /hbase/conf/hbase-site.xml

ENV PATH /hbase/bin:$PATH

#      zookeeper - HBase Master API port - HBase Master Web UI - Regionserver API port - HBase Regionserver web UI
EXPOSE 2181        60000                   60010                 60020                   60030

# Act as the HBase user:
USER hbase
WORKDIR /hbase

# Define default command.
CMD ["hbase", "master", "start"]
