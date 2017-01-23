FROM python:3.5

# Setup java
RUN set -x && \
    apt-get update && \
    apt-get install --no-install-recommends -y software-properties-common && \
    echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"  > \
        /etc/apt/sources.list.d/webupd8team-java.list && \
    echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" >> \
        /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get update && echo yes | apt-get install -y --force-yes oracle-java8-installer && \
    apt-get update && apt-get install oracle-java8-set-default && \
    apt-get remove software-properties-common -y --auto-remove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

ENV HADOOP_MAJOR_MINOR_VERSION 2.7
ENV HADOOP_VERSION ${HADOOP_MAJOR_MINOR_VERSION}.3
ENV SPARK_VERSION 1.6.1

# Setup hadoop variables
ENV HADOOP_HOME /opt/hadoop
ENV PATH ${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin
ENV HADOOP_MAPRED_HOME ${HADOOP_HOME}
ENV HADOOP_COMMON_HOME ${HADOOP_HOME}
ENV HADOOP_HDFS_HOME ${HADOOP_HOME}
ENV YARN_HOME ${HADOOP_HOME}
ENV HADOOP_COMMON_LIB_NATIVE_DIR ${HADOOP_HOME}/lib/native
ENV HADOOP_OPTS "-Djava.library.path=${HADOOP_HOME}/lib"
ENV HDFS_CONF_DIR ${HADOOP_HOME}/etc/hadoop
ENV YARN_CONF_DIR ${HADOOP_HOME}/etc/hadoop
ENV HADOOP_CONF_DIR ${HADOOP_HOME}/etc/hadoop

# Setup Hive
ENV HIVE_CONF_DIR ${HADOOP_CONF_DIR}

# Setup spark
ENV SPARK_HOME=/opt/spark-${SPARK_VERSION}
ENV PYTHONPATH=${SPARK_HOME}/python:${SPARK_HOME}/python/lib/py4j-0.10.3-src.zip
ENV PYSPARK_PYTHON=python
ENV PATH=$PATH:${SPARK_HOME}/bin

# Add these two spark packages when submitting PySpark applications
#ENV PYSPARK_SUBMIT_ARGS="--packages com.databricks:spark-csv_2.11:1.4.0,com.databricks:spark-avro_2.10:2.0.1,graphframes:graphframes:0.1.0-spark1.6 pyspark-shell"
ENV PYSPARK_SUBMIT_ARGS="--packages com.databricks:spark-csv_2.11:1.5.0,com.databricks:spark-avro_2.11:3.1.0,graphframes:graphframes:0.3.0-spark2.0-s_2.11 pyspark-shell"

# Exposes the relevant ports and setup the port settings
ENV SPARK_MASTER_OPTS="-Dspark.driver.port=7001 -Dspark.fileserver.port=7002 -Dspark.broadcast.port=7003 -Dspark.replClassServer.port=7004 -Dspark.blockManager.port=7005 -Dspark.executor.port=7006 -Dspark.ui.port=4040 -Dspark.broadcast.factory=org.apache.spark.broadcast.HttpBroadcastFactory"
ENV SPARK_WORKER_OPTS="-Dspark.driver.port=7001 -Dspark.fileserver.port=7002 -Dspark.broadcast.port=7003 -Dspark.replClassServer.port=7004 -Dspark.blockManager.port=7005 -Dspark.executor.port=7006 -Dspark.ui.port=4040 -Dspark.broadcast.factory=org.apache.spark.broadcast.HttpBroadcastFactory"

ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_WORKER_PORT 8888
ENV SPARK_WORKER_WEBUI_PORT 8081

# Set up sqoop
ENV SQOOP_HOME /opt/sqoop
ENV PATH ${PATH}:${SQOOP_HOME}/bin:${HADOOP_HOME}/bin

# Download Binaries
RUN set -x && \
    echo "Downloading Hadoop" && \
    wget -qO - http://apache.stu.edu.tw/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz | \
        tar -xz -C /opt/ && \
    mv /opt/hadoop-${HADOOP_VERSION} /opt/hadoop && \
    echo "Downloading Spark" && \
    wget -qO - http://apache.stu.edu.tw/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_MAJOR_MINOR_VERSION}.tgz |\
    tar -xz -C /opt/ && \
    mv /opt/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_MAJOR_MINOR_VERSION} /opt/spark-${SPARK_VERSION} && \
    echo "Downloading Spark Packages" && \
    wget -q http://repo1.maven.org/maven2/com/databricks/spark-avro_2.11/3.1.0/spark-avro_2.11-3.1.0.jar -P ${SPARK_HOME}/lib && \
    wget -q http://repo1.maven.org/maven2/com/databricks/spark-csv_2.11/1.5.0/spark-csv_2.11-1.5.0.jar -P ${SPARK_HOME}/lib && \
    echo "Downloading Sqoop" && \
    wget -qO - http://www.apache.org/dist/sqoop/1.4.6/sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz | tar -xz -C /opt && \
    cd /opt && ln -s ./sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop && \
    echo "Downloading the JDBC drivers for Postgresql" && \
    wget -qP /opt/sqoop/lib/ https://jdbc.postgresql.org/download/postgresql-9.4-1201.jdbc4.jar && \
    echo "Downloading the JDBC drivers for mysql" && \
    wget -qP /tmp/ http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.37.tar.gz && \
    tar -C /tmp/ -xzf /tmp/mysql-connector-java-5.1.37.tar.gz && \
    cp /tmp/mysql-connector-java-5.1.37/mysql-connector-java-5.1.37-bin.jar /opt/sqoop/lib/ && \
    echo "Downloading mssql driver for sqoop" && \
    wget -qO - 'http://download.microsoft.com/download/0/2/A/02AAE597-3865-456C-AE7F-613F99F850A8/sqljdbc_4.0.2206.100_enu.tar.gz' | \
    tar xz -C /tmp && \
    mv /tmp/sqljdbc_4.0/enu/sqljdbc4.jar ${SQOOP_HOME}/lib && \
    rm -r /tmp/sqljdbc_4.0 && \
    echo "Cleaning up" && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 8080 7077 8888 8081 4040 7001 7002 7003 7004 7005 7006

CMD '/bin/bash'
