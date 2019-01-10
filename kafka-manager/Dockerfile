FROM openjdk:8-jdk-slim

ENV ZK_HOSTS=localhost:2181 \
    KM_VERSION=1.3.3.22 \
    KM_REVISION=04b585eddd81768b03b6989efcfb5032307a1888 \
    KM_CONFIGFILE="conf/application.conf"

ADD start-kafka-manager.sh /kafka-manager-${KM_VERSION}/start-kafka-manager.sh

RUN buildDeps="wget git unzip" \
    && apt-get update && apt-get install -y --no-install-recommends $buildDeps \
    && cd /tmp \
    && git clone https://github.com/yahoo/kafka-manager.git \
    && cd kafka-manager \
    && echo 'scalacOptions ++= Seq("-Xmax-classfile-name", "200")' >> build.sbt \
    && ./sbt clean dist \
    && unzip  -d / ./target/universal/kafka-manager-${KM_VERSION}.zip \
    && cd ../ \
    && rm -fr /tmp/* /root/.sbt /root/.ivy2 \
    && chmod +x /kafka-manager-${KM_VERSION}/start-kafka-manager.sh \
    && apt-get purge -y --auto-remove \
                     -o APT::AutoRemove::RecommendsImportant=false \
                     $buildDeps \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /kafka-manager-${KM_VERSION}

EXPOSE 9000
ENTRYPOINT ["./start-kafka-manager.sh"]
