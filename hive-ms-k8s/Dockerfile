FROM ubuntu:22.04

ARG HADOOP_VERSION=3.3.4

RUN apt-get update && \
apt-get install -y ca-certificates --no-install-recommends && \
apt install openjdk-8-jre-headless -y && \
apt-get install -y curl --no-install-recommends && rm -rf /var/lib/apt/lists/*

# Download and extract the Hadoop binary package.
RUN curl https://archive.apache.org/dist/hadoop/core/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz \
 | tar xvz -C /opt/  \
 && ln -s /opt/hadoop-$HADOOP_VERSION /opt/hadoop \
 && rm -r /opt/hadoop/share/doc

# Add S3a jars to the classpath using this hack.
RUN ln -s /opt/hadoop/share/hadoop/tools/lib/hadoop-aws* /opt/hadoop/share/hadoop/common/lib/ && \
    ln -s /opt/hadoop/share/hadoop/tools/lib/aws-java-sdk* /opt/hadoop/share/hadoop/common/lib/



# Download and install the standalone metastore binary.
RUN curl https://archive.apache.org/dist/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz \
 | tar xvz -C /opt/ \
 && ln -s /opt/apache-hive-3.1.3-bin /opt/hive 

# Set necessary environment variables.
ENV HADOOP_HOME="/opt/hadoop"
ENV HIVE_HOME="/opt/hive"
ENV JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/"
ENV PATH="/opt/hive/bin:/opt/hadoop/bin:$JAVA_HOME/bin:${PATH}"

ADD https://repo1.maven.org/maven2/org/postgresql/postgresql/42.7.3/postgresql-42.7.3.jar /opt/hadoop/share/hadoop/common/lib/
ADD https://repo1.maven.org/maven2/org/postgresql/postgresql/42.7.3/postgresql-42.7.3.jar /opt/hive/lib/
