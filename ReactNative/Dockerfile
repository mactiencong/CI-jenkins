# Pull base image.
FROM ubuntu:14.04

ARG DEBIAN_FRONTEND=noninteractive

# Install base software packages
RUN apt-get update && \
    apt-get install software-properties-common \
    python-software-properties \
    wget \
    curl \
    git \
    dialog -y \
    apt-utils -y \
    apt-transport-https -y \
    unzip -y && \
    apt-get clean


# ——————————
# Install Java.
# ——————————

# RUN \
#  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
#  add-apt-repository -y ppa:webupd8team/java && \
#  apt-get update && \
#  apt-get install -y oracle-java8-installer && \
#  rm -rf /var/lib/apt/lists/* && \
#  rm -rf /var/cache/oracle-jdk8-installer
COPY jdk-8u161-linux-x64.tar.gz ./jdk-8u161-linux-x64.tar.gz
RUN tar -xvf jdk-8u161-linux-x64.tar.gz
RUN mkdir -p /usr/lib/jvm
RUN mv ./jdk1.8.0_161 /usr/lib/jvm/
RUN update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_161/bin/java" 1
RUN update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_161/bin/javac" 1
RUN update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0_161/bin/javaws" 1
RUN chmod a+x /usr/bin/java
RUN chmod a+x /usr/bin/javac
RUN chmod a+x /usr/bin/javaws
RUN chown -R root:root /usr/lib/jvm/jdk1.8.0_161


# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/jdk1.8.0_161


# ——————————
# Installs i386 architecture required for running 32 bit Android tools
# ——————————

RUN dpkg --add-architecture i386 && \
    apt-get update -y && \
    apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get autoremove -y && \
    apt-get clean


# ——————————
# Installs Android SDK
# ——————————

ENV ANDROID_SDK_VERSION r24.4.1
ENV ANDROID_BUILD_TOOLS_VERSION build-tools-23.0.3,build-tools-23.0.2,build-tools-23.0.1,build-tools-25.0.0,build-tools-25.0.2,build-tools-25.0.3

ENV ANDROID_SDK_FILENAME android-sdk_${ANDROID_SDK_VERSION}-linux.tgz
ENV ANDROID_SDK_URL http://dl.google.com/android/${ANDROID_SDK_FILENAME}
ENV ANDROID_API_LEVELS android-25
ENV ANDROID_EXTRA_COMPONENTS extra-android-m2repository,extra-google-m2repository
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools
RUN cd /opt && \
    wget -q ${ANDROID_SDK_URL} && \
    tar -xzf ${ANDROID_SDK_FILENAME} && \
    rm ${ANDROID_SDK_FILENAME} && \
    echo y | android update sdk --no-ui -a --filter tools,platform-tools,${ANDROID_API_LEVELS},${ANDROID_BUILD_TOOLS_VERSION} && \
    echo y | android update sdk --no-ui --all --filter "${ANDROID_EXTRA_COMPONENTS}"

# ——————————
# Installs Gradle
# ——————————

# Gradle
# ENV GRADLE_VERSION 2.4
ENV GRADLE_VERSION 3.3

RUN cd /usr/lib \
 && curl -fl https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle-bin.zip \
 && unzip "gradle-bin.zip" \
 && ln -s "/usr/lib/gradle-${GRADLE_VERSION}/bin/gradle" /usr/bin/gradle \
 && rm "gradle-bin.zip"

# Set Appropriate Environmental Variables
ENV GRADLE_HOME /usr/lib/gradle
ENV PATH $PATH:$GRADLE_HOME/bin


# ——————————
# Install Node and global packages
# ——————————
# ENV NODE_VERSION 5.6.0
ENV NODE_VERSION 6.10.3
RUN cd && \
    wget -q http://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-x64.tar.gz && \
    mv node-v${NODE_VERSION}-linux-x64 /opt/node && \
    rm node-v${NODE_VERSION}-linux-x64.tar.gz
ENV PATH ${PATH}:/opt/node/bin

RUN npm install nvm

# ——————————
# Install Basic React-Native packages
# ——————————
RUN npm install react-native-cli -g
RUN npm install rnpm -g

ENV LANG en_US.UTF-8

# ----
# Install typescript
# ----

RUN npm install -g typescript
RUN export PATH=/root/.nvm/versions/node/v${NODE_VERSION}/lib/node_modules/typescript/bin:$PATH

#----
# Make working dir
#----
RUN mkdir -p /var/app
RUN touch ~/.android/repositories.cfg
