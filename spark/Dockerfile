FROM alpine:latest

RUN apk --update add make automake gcc g++ subversion && \
    apk --update add curl tar bash python3-dev openjdk8

RUN curl -L http://apache.spinellicreations.com/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz -o spark-2.4.3-bin-hadoop2.7.tgz 

# livy binary
RUN curl -L http://mirrors.ocf.berkeley.edu/apache/incubator/livy/0.6.0-incubating/apache-livy-0.6.0-incubating-bin.zip \
     -o apache-livy-0.6.0-incubating-bin.zip

RUN tar -xzf spark-2.4.3-bin-hadoop2.7.tgz && \
    mv spark-2.4.3-bin-hadoop2.7 /spark && \
    rm spark-2.4.3-bin-hadoop2.7.tgz && \
    pip3 install --upgrade pip && \
    pip3 install numpy pandas && \
    mkdir /etc/conf.d/hadoop

# livy config
RUN unzip apache-livy-0.6.0-incubating-bin.zip && \
    mv apache-livy-0.6.0-incubating-bin /livy && \
    rm apache-livy-0.6.0-incubating-bin.zip && \
    mkdir /livy/logs && \
    mkdir -p /hadoop/conf

ENV SPARK_HOME="/spark"
ENV PYSPARK_PYTHON="/usr/bin/python3"
ENV PYSPARK_DRIVER_PYTHON="jupyter"
ENV PYSPARK_DRIVER_PYTHON_OPTS="notebook"
ENV JAVA_HOME="/usr/lib/jvm/java-1.8-openjdk"
ENV HADOOP_CONF_DIR="/etc/conf.d/hadoop"

COPY start-master.sh /start-master.sh
COPY start-worker.sh /start-worker.sh
COPY livy.conf /livy/conf
