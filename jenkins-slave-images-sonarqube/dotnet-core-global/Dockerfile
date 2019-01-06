FROM openshift/jenkins-slave-base-centos7

USER root

ENV SONARQUBE_SCANNER_VERSION=4.4.2.1543 \
    HOME=/home/jenkins 

# Install .NET sdk rpm
RUN rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Install headless Java and .NET sdk
RUN yum install -y centos-release-scl-rh && \
    INSTALL_PKGS="wget dotnet-sdk-2.1" && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all

WORKDIR /

# Install SonarQube Scanner as the running user
RUN dotnet tool install --global dotnet-sonarscanner
ENV PATH="${PATH}:$HOME/.dotnet/tools"

RUN chown -R 1029:0 /usr/local/bin && \
    chown -R 1029:0 $HOME && \
    chmod -R g+rwx $HOME

USER 1029