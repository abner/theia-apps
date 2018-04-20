FROM abner/openjdk8u161-jdk-alpine as java-base

FROM node:8-alpine

COPY --from=java-base /usr/lib/jvm/java-1.8-openjdk /usr/lib/jvm/java-1.8-openjdk

ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk/jre \
    PATH=$PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin

RUN apk add --no-cache make gcc g++ python
ARG version=latest
WORKDIR /home/theia

ADD $version.package.json ./package.json

ENV GIT_SSL_NO_VERIFY=true

RUN yarn && \
    yarn theia build && \
    rm -rf ./node_modules/electron && \
    yarn cache clean

FROM node:8-alpine
RUN apk add --no-cache git openssh bash
WORKDIR /home/theia
COPY --from=0 /home/theia /home/theia
EXPOSE 3000
ENV SHELL /bin/bash
ENV USE_LOCAL_GIT true
ENTRYPOINT [ "yarn", "theia", "start", "/home/project", "--hostname=0.0.0.0" ]
