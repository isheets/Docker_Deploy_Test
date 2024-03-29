FROM ubuntu

# Install required packages

RUN apt-get update && \
    apt-get install -y build-essential && \
    apt-get install -y software-properties-common && \
    apt-get install -y python

# Install Java.

# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean;

# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

# Setup JAVA_HOME -- useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME


# Install Storm.

ENV STORM_USER=usama \
    STORM_CONF_DIR=/conf \
    STORM_DATA_DIR=/data \
    STORM_LOG_DIR=/logs

# Add a user and make dirs
RUN set -ex; \
    adduser "$STORM_USER"; \
    mkdir -p "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"; \
    chown -R "$STORM_USER:$STORM_USER" "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"``

ARG DISTRO_NAME=apache-storm-1.2.2

# Download Apache Storm.
RUN set -ex; \
    wget -q "http://us.mirrors.quenda.co/apache/storm/apache-storm-1.2.2/apache-storm-1.2.2.tar.gz"; \
    export GNUPGHOME="$(mktemp -d)"; \
    tar -xzf "$DISTRO_NAME.tar.gz"; \
    chown -R "$STORM_USER:$STORM_USER" "$DISTRO_NAME"; \
    rm "$DISTRO_NAME.tar.gz";

ENV PATH $PATH:/$DISTRO_NAME/bin

ADD storm.yaml /conf

RUN chown -R "$STORM_USER" "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"


# Install Maven.

ENV MAVEN_VERSION="3.6.1" \
    M2_HOME=/usr/lib/mvn

RUN cd /tmp && \
    wget "http://apache.mirrors.lucidnetworks.net/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz" && \
    tar -zxvf "apache-maven-$MAVEN_VERSION-bin.tar.gz" && \
    mv "apache-maven-$MAVEN_VERSION" "$M2_HOME" && \
    ln -s "$M2_HOME/bin/mvn" /usr/bin/mvn && \
    rm -R /tmp/*