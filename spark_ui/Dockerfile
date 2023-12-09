FROM amazonlinux:2
FROM amazoncorretto:11
FROM maven:3.9.3-amazoncorretto-11

RUN yum install -y procps

RUN yum upgrade -y
RUN yum upgrade jettison

WORKDIR /tmp/
ADD pom.xml /tmp

RUN curl -o ./spark-3.4.1-bin-without-hadoop.tgz https://archive.apache.org/dist/spark/spark-3.4.1/spark-3.4.1-bin-without-hadoop.tgz
RUN tar -xzf spark-3.4.1-bin-without-hadoop.tgz && \
    mv spark-3.4.1-bin-without-hadoop /opt/spark && \
    rm spark-3.4.1-bin-without-hadoop.tgz

RUN mvn dependency:copy-dependencies -DoutputDirectory=/opt/spark/jars/

RUN rm -rf /root/.m2

RUN rm /opt/spark/yarn/spark-3.4.1-yarn-shuffle.jar && \
    rm /opt/spark/jars/jsr305-3.0.0.jar && \
    rm /opt/spark/jars/jmespath-java-*.jar && \
    rm /opt/spark/jars/aws-java-sdk-core-*.jar && \
    rm /opt/spark/jars/aws-java-sdk-kms-*.jar && \
    rm /opt/spark/jars/aws-java-sdk-s3-*.jar && \
    rm /opt/spark/jars/ion-java-1.0.2.jar && \
    rm /opt/spark/jars/snappy-java-1.1.8.2.jar && \
    rm /opt/spark/jars/paranamer-2.3.jar && \
    rm /opt/spark/jars/netty-transport-native-unix-common-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-transport-native-epoll-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-transport-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-resolver-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-handler-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-common-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-codec-4.1.63.Final.jar && \
    rm /opt/spark/jars/netty-buffer-4.1.63.Final.jar && \
    rm /opt/spark/jars/metrics-core-3.2.4.jar && \
    rm /opt/spark/jars/jersey-server-1.19.4.jar && \
    rm /opt/spark/jars/jersey-client-1.19.4.jar && \
    rm /opt/spark/jars/gson-2.8.9.jar && \
    rm /opt/spark/jars/commons-math3-3.1.1.jar && \
    rm /opt/spark/jars/commons-io-2.8.0.jar && \
    rm /opt/spark/jars/commons-compress-1.21.jar && \
    rm /opt/spark/jars/avro-1.7.7.jar && \
    rm /opt/spark/jars/mesos-1.4.3-shaded-protobuf.jar && \
    rm /opt/spark/jars/spark-mesos_2.12-3.4.1.jar && \
    rm /opt/spark/jars/snakeyaml-1.33.jar && \
    rm /opt/spark/jars/jettison-1.1.jar && \
    rm /opt/spark/jars/zjsonpatch-0.3.0.jar && \
    rm /opt/spark/jars/parquet*.jar && \
    rm /opt/spark/jars/nimbus-jose-jwt-9.8.1.jar && \
    rm /opt/spark/jars/guava-27.0-jre.jar

COPY jars/snakeyaml-2.0.jar /opt/spark/jars/
COPY jars/mesos-1.11.0.jar /opt/spark/jars/
COPY jars/mesos-1.11.0-shaded-protobuf.jar /opt/spark/jars/
COPY jars/guava-32.1.1-jre.jar /opt/spark/jars/
COPY jars/jettison-1.5.4.jar /opt/spark/jars/
COPY jars/json-smart-2.4.11.jar /opt/spark/jars/
COPY jars/nimbus-jose-jwt-9.31.jar /opt/spark/jars/
COPY jars/spark-network-common_2.12-3.4.1.jar /opt/spark/jars/
COPY jars/hadoop-shaded-guava-1.1.1.jar /opt/spark/jars/

RUN rm -rf /usr/share/maven/lib/*

RUN echo $'\n\
spark.eventLog.enabled                      true\n\
spark.history.ui.port                       8090\n\
' > /opt/spark/conf/spark-defaults.conf

RUN echo $'\n\
log4j.rootLogger = info, console\n\
log4j.appender.console=org.apache.log4j.ConsoleAppender\n\
log4j.appender.console.target = System.out\n\
log4j.appender.console.layout = org.apache.log4j.PatternLayout\n\
log4j.appender.console.layout.ConversionPattern = %d{yyyy-MM-dd HH:mm:ss} %p %c{2}: %m%n\n\
' > /opt/spark/conf/log4j.properties

ENTRYPOINT ["/bin/bash", "-c"]:

