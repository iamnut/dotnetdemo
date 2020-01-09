FROM codercom/code-server:v2
MAINTAINER Titiwut M. <public@iamnut.com>
SHELL ["/bin/bash", "-c"]
USER root
ENV TZ=Asia/Bangkok \
    SERVICE_URL=https://marketplace.visualstudio.com/_apis/public/gallery \
    ITEM_URL=https://marketplace.visualstudio.com/items

RUN sed -i 's/archive.ubuntu.com/mirrors.bangmod.cloud/g' /etc/apt/sources.list \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive \
       apt-get install --no-install-recommends -y \
         tzdata \
         apt-transport-https \
         apt-utils \
         ca-certificates \
         gnupg-agent \
         software-properties-common \
         iputils-ping

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
    && apt-get update \
    && apt-get install docker-ce-cli

RUN cd /tmp \
    && wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb \
    && add-apt-repository universe \
    && apt-get update \
    && apt-get install --no-install-recommends -y dotnet-sdk-3.1

RUN rm -rf /var/lib/apt/lists/*

USER coder
RUN mkdir -p /home/coder/.vscode/User \
    && mkdir -p /home/coder/.vscode/User/extensions \
    && code-server --extensions-dir /home/coder/.vscode/User/extensions \
                   --install-extension ms-vscode.csharp \
                   --install-extension mhutchie.git-graph

ENTRYPOINT ["dumb-init", "code-server", "--host", "0.0.0.0", "--user-data-dir", "/home/coder/.vscode","--extensions-dir", "/home/coder/.vscode/User/extensions"]
