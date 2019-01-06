FROM openshift/jenkins-slave-base-centos7

USER root

ENV SONARQUBE_SCANNER_VERSION=3.2.0.1227 \
    PATH=$PATH:/sonarqube-scanner/bin \
    HOME=/home/jenkins \
    JAVA_VERSION=8u191 \
    BUILD_VERSION=b12

# Install Java without interaction
RUN yum install -y centos-release-scl-rh && \
    INSTALL_PKGS="wget" && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/$JAVA_VERSION-$BUILD_VERSION/2787e4a523244c269598db4e85c51e0c/jdk-$JAVA_VERSION-linux-x64.rpm" -O /tmp/jdk-8-linux-x64.rpm && \
    yum -y install /tmp/jdk-8-linux-x64.rpm && \
    yum clean all

WORKDIR /

# Install SonarQube Scanner
RUN set -x && \
    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONARQUBE_SCANNER_VERSION-linux.zip && \
    unzip sonar-scanner-cli-$SONARQUBE_SCANNER_VERSION-linux.zip && \
    rm sonar-scanner-cli-$SONARQUBE_SCANNER_VERSION-linux.zip && \
    mv /sonar-scanner-$SONARQUBE_SCANNER_VERSION-linux /sonarqube-scanner

RUN chown -R 1029:0 /usr/local/bin && \
    chown -R 1029:0 $HOME && \
    chmod -R g+rw $HOME

USER 1029