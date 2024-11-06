# За основну взят
# https://github.com/apache/camel/blob/camel-4.8.1/dsl/camel-jbang/camel-jbang-container/Dockerfile

FROM docker.io/eclipse-temurin:21-jdk

ENV JBANG_VERSION=0.118.0
# /!\ Camel version must be changed in the entrypoint line too
ARG CAMEL_VERSION=4.8.1

RUN wget -c https://github.com/jbangdev/jbang/releases/download/v$JBANG_VERSION/jbang.tar -O - | tar xv && \
    chmod +x jbang/bin/jbang
ENV PATH="${PATH}:/jbang/bin"
ENV JBANG_PATH=/jbang/bin

RUN jbang trust add https://github.com/apache/camel

# used to initiliaze dependencies in the docker image
RUN jbang -Dcamel.jbang.version=$CAMEL_VERSION camel@apache/camel version
# RUN jbang  camel@apache/camel version

RUN echo "# /bin/sh" >> /run-camel.sh \
    && echo "jbang -Dcamel.jbang.version=$CAMEL_VERSION camel@apache/camel \$@" >> /run-camel.sh \
    && chmod +x /run-camel.sh

# ENTRYPOINT [ "jbang", "-Dcamel.jbang.version=4.4.0", "camel@apache/camel"]
ENTRYPOINT [ "/bin/sh", "/run-camel.sh", "run", "/integrations/*"]
# ENTRYPOINT [ "jbang", "camel@apache/camel"]