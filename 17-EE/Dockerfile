# ----------------------------------
# Pterodactyl Core Dockerfile
# Environment: Java
# Minimum Panel Version: 1.7.0
# ----------------------------------
FROM ubuntu:22.04

ARG GRAAL_VERSION=22.3.2
ARG JAVA_VERSION=17

MAINTAINER RikoDEV, <kontakt@riko.dev>

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y \
 && apt-get install -y curl ca-certificates openssl git tar sqlite3 fontconfig tzdata iproute2 \
 && export ARCH=$(case ${TARGETPLATFORM:-linux/amd64} in \
    "linux/amd64")   echo "linux-amd64"  ;; \
    "linux/arm64")   echo "linux-aarch64" ;; \
    *)               echo ""        ;; esac) \
  && echo "ARCH=$ARCH" \
  && curl --retry 3 -Lfso /tmp/graalvm.tar.gz https://oca.opensource.oracle.com/gds/GRAALVM_EE_JAVA17_22_3_2/graalvm-ee-java17-${ARCH}-22.3.2.tar.gz \
  && mkdir -p /opt/java/graalvm \
  && cd /opt/java/graalvm \
  && tar -xf /tmp/graalvm.tar.gz --strip-components=1 \
  && export PATH="/opt/java/graalvm/bin:$PATH" \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /tmp/graalvm.tar.gz

ENV JAVA_HOME=/opt/java/graalvm \
    PATH="/opt/java/graalvm/bin:$PATH"

# Step 2 - add pterodactyl stuff
RUN useradd -d /home/container -m container

USER        container
ENV         USER=container HOME=/home/container

WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh

CMD         ["/bin/bash", "/entrypoint.sh"]