FROM artsy/hokusai

ENV SCALA_VERSION=2.12.7 \
    SBT_VERSION=1.2.6 \
    DOCKERIZE_VERSION=v0.6.1

RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends wget curl tar gzip postgresql-client awscli jq\
                                               openjdk-8-jdk ca-certificates-java && \
    cd "/tmp" && \
    wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    curl -q -L -O www.scala-lang.org/files/archive/scala-$SCALA_VERSION.deb && \
    dpkg -i scala-$SCALA_VERSION.deb && \
    curl -q -L -O https://bintray.com/artifact/download/sbt/debian/sbt-$SBT_VERSION.deb && \
    dpkg -i sbt-$SBT_VERSION.deb && \
    curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install -y nodejs && \
    ls /usr/bin/p* && \
    pip2 install hokusai && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Trigger compiler-interface compilation
RUN  cd "/tmp" && \
  mkdir -p project src/main/scala && \
  echo "sbt.version = ${SBT_VERSION}" > project/build.properties && \
  echo "scalaVersion := \"${SCALA_VERSION}\"" > build.sbt && \
  touch src/main/scala/scratch.scala && \
  sbt compile && \
  rm -rf *
