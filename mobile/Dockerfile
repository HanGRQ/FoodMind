# 使用 Ubuntu 20.04 作为基础镜像
FROM ubuntu:20.04

# 设置非交互模式
ARG DEBIAN_FRONTEND=noninteractive

# 更新软件源并安装依赖
RUN apt-get update \
    && apt-get install -y \
    software-properties-common \
    wget \
    xz-utils \
    zip \
    openjdk-8-jdk \
    curl

# 清理缓存
RUN rm -rf /var/lib/apt/lists/*

# 设置 JAVA_HOME 环境变量
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
ENV PATH=${JAVA_HOME}/bin:${PATH}

# 安装 Node.js 和 npm
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs

# 安装 Android SDK 工具
RUN wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip -O /tmp/android-sdk.zip \
    && mkdir -p /opt/android-sdk \
    && cd /opt/android-sdk \
    && unzip /tmp/android-sdk.zip \
    && rm /tmp/android-sdk.zip

# 设置 Android SDK 的环境变量
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$PATH

# 安装 Android 构建工具和平台
RUN yes | sdkmanager --licenses \
    && yes | sdkmanager "build-tools;35.0.0" \
    && yes | sdkmanager "platforms;android-27"

# 安装 Cordova，并关闭 telemetry
RUN npm install -g cordova --unsafe-perm \
    && cordova telemetry off

# 下载并安装 Gradle
RUN wget https://services.gradle.org/distributions/gradle-8.10.2-bin.zip -P /tmp \
    && unzip /tmp/gradle-8.10.2-bin.zip -d /opt/gradle \
    && rm /tmp/gradle-8.10.2-bin.zip

# 设置 Gradle 的环境变量
ENV GRADLE_HOME=/opt/gradle
ENV PATH=$GRADLE_HOME/bin:$PATH


# 设置工作目录
WORKDIR /mobile
