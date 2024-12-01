FROM openjdk:17-jdk-bullseye

LABEL version="1.1.2"

ARG PASSPHRASE

ENV PASSPHRASE=$PASSPHRASE
ENV VERSION_SDK_TOOLS "9477386_latest"
ENV ANDROID_SDK_ROOT "/sdk"
ENV ANDROID_HOME $ANDROID_SDK_ROOT
ENV GRADLE_PROFILER_VERSION "0.20.0"
ENV GRADLE_PROFILER "/gradle-profiler/gradle-profiler-${GRADLE_PROFILER_VERSION}"
ENV PATH "${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools:${ANDROID_SDK_ROOT}/platform-tools/bin:${ANDROID_SDK_ROOT}/tools/:${ANDROID_SDK_ROOT}/tools/bin:${GRADLE_PROFILER}/bin:${PATH}"

ENV DEBIAN_FRONTEND noninteractive
ENV LANG 'en_US.UTF-8'
ENV LANGUAGE 'en_US:en'
ENV LC_ALL 'en_US.UTF-8'
ENV WHO "jenkins_agent"

# RUN dpkg --add-architecture i386 
# RUN dpkg --add-architecture amd64

RUN apt-get update && \
    apt-get install --yes \
    xvfb lib32z1 lib32stdc++6 build-essential \
    libcurl4-openssl-dev libglu1-mesa libxi-dev libxmu-dev \
    libglu1-mesa-dev

RUN apt-get -qq update && \
  apt-get install -qqy --no-install-recommends \
  ca-certificates \
  bzip2 \
  vim \
  wget \
  curl \
  expect \
  git \
  git-core \
  html2text \
  unzip \
  locales \
  rsync \
  openssh-server \
  iputils-ping \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY gradle-profiler-${GRADLE_PROFILER_VERSION}.zip gradle-profiler/

RUN  echo "export ANDROID_SDK_ROOT=$ANDROID_SDK_ROOT" | cat - ~/.bashrc >> temp && mv temp ~/.bashrc

#install gradle-profiler
RUN unzip gradle-profiler/gradle-profiler-${GRADLE_PROFILER_VERSION}.zip -d gradle-profiler && \
  rm -v gradle-profiler/gradle-profiler-${GRADLE_PROFILER_VERSION}.zip

#python 3 and required dependencies
RUN apt-get update && \
  apt-get install -y python3 python3-pip
ADD requirements.txt /opt/requirements.txt
WORKDIR /opt
RUN pip install -r requirements.txt

RUN groupadd -g 1000 -r jenkins && \
  groupadd -g 999 -r docker && \
  groupadd -g 994 -r dockerroot && \
  useradd --create-home -d "/home/jenkins" -u 1000 -r -g jenkins -G docker,dockerroot jenkins

ADD --chown=jenkins https://services.gradle.org/distributions/gradle-8.6-all.zip /home/jenkins/.gradle/wrapper/dists/gradle-8.6-all/531yu94o2h2dc7gma58ju6mq5/gradle-8.6-all.zip

RUN mkdir -p $ANDROID_SDK_ROOT && \
    chown -R jenkins $ANDROID_SDK_ROOT

# switch to user jenkins
USER jenkins

RUN curl -s https://dl.google.com/android/repository/commandlinetools-linux-${VERSION_SDK_TOOLS}.zip > $ANDROID_SDK_ROOT/sdk.zip && \
  mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
  unzip $ANDROID_SDK_ROOT/sdk.zip -d $ANDROID_SDK_ROOT/cmdline-tools && \
  mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
  rm -v $ANDROID_SDK_ROOT/sdk.zip

ADD --chown=jenkins packages.txt $ANDROID_SDK_ROOT

RUN mkdir -p $HOME/.android && \
  touch $HOME/.android/repositories.cfg

RUN yes | sdkmanager --licenses && \
    sdkmanager --package_file=$ANDROID_SDK_ROOT/packages.txt