FROM ubuntu:14.04

RUN apt-get update && apt-get install -y  curl grep sed unzip xz-utils

WORKDIR /var/lib/sonarscanner
RUN mkdir -p /var/lib/sonarscanner/src


ENV LANG=C.UTF-8

# install openjdk
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update
RUN apt-get install -y openjdk-8-jdk
RUN update-alternatives --config java
RUN update-alternatives --config javac

RUN curl --insecure -o ./sonarscanner.zip -L https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.0.3.778-linux.zip
RUN unzip sonarscanner.zip
RUN rm sonarscanner.zip
ENV SONAR_RUNNER_HOME /var/lib/sonarscanner/sonar-scanner-3.0.3.778-linux
ENV PATH $PATH:${SONAR_RUNNER_HOME}/bin

COPY sonar-runner.properties ./sonar-scanner-3.0.3.778-linux/conf/sonar-scanner.poperties


#   ensure Sonar uses the provided Java for musl instead of a borked glibc one
RUN sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' ${SONAR_RUNNER_HOME}/bin/sonar-scanner

# Install nodejs
ENV NODE_PATH /var/lib/nodejs
RUN mkdir -p ${NODE_PATH}
RUN curl -O  https://nodejs.org/dist/v8.9.4/node-v8.9.4-linux-x64.tar.xz
RUN tar -xJvf node-v8.9.4-linux-x64.tar.xz -C ${NODE_PATH}
ENV PATH $PATH:${NODE_PATH}/node-v8.9.4-linux-x64/bin

# Use bash if you want to run the environment from inside the shell, otherwise use the command that actually runs the underlying stuff
CMD /bin/bash
