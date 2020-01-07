FROM codercom/code-server:v2
MAINTAINER Titiwut M. <public@iamnut.com>
SHELL ["/bin/bash", "-c"]
ENV TZ=Asia/Bangkok
USER root

RUN sed -i 's/archive.ubuntu.com/mirrors.bangmod.cloud/g' /etc/apt/sources.list \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive \
       apt-get install --no-install-recommends -y tzdata software-properties-common apt-transport-https apt-utils iputils-ping

RUN cd /tmp \
    && wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb \
    && add-apt-repository universe \
    && apt-get update \
    && apt-get install --no-install-recommends -y dotnet-sdk-3.1

RUN rm -rf /var/lib/apt/lists/*

USER coder
RUN mkdir -p /home/coder/.vscode/User
ENTRYPOINT ["dumb-init", "code-server", "--host", "0.0.0.0", "--user-data-dir", "/home/coder/.vscode"]
