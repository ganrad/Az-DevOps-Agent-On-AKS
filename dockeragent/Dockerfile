# Azure DevOps Pipeline Agent : 08-16-2019
#
# Description: This dockerfile builds an Azure DevOps self-hosted agent. It has been modified to include the following build tools
#   - docker-ce-cli
#   - Aqua Trivy, an open source Container image scanner (ID09272019)
#   - Azure CLI (ID09262019)
#   - kubectl
#   - helm
#
# NOTES:
# ganrad01: ID10092020 : Use base image Ubuntu:18.04, libcurl4 and Helm v3
# ganrad01: ID02242021 : libicu60 is a dependency for .NET Core 3.1
#
FROM ubuntu:18.04
MAINTAINER Ganesh Radhakrishnan ganrad01@gmail.com

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

# ID02242021.sn
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"
# ID02242021.en

RUN apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
	libcurl4 \
        # libcurl3 \
        # libicu55 \
        libicu60 \
        libunwind8 \
        netcat \
        zip \
        unzip \
        wget \
        apt-transport-https \
	gnupg \
        gnupg-agent \
        # software-properties-common \
        apt-utils

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Setup Aqua Trivy public key and repo. - ID09272019
RUN wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | apt-key add -
RUN echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -cs) main | tee -a /etc/apt/sources.list.d/trivy.list

# Update Ubuntu and install docker CLI, Aqua Trivy
RUN apt-get update && apt-get install docker-ce-cli trivy

# Install all build tools in /build-tools
WORKDIR /build-tools

# Install Azure CLI - ID09262019
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Install kubectl (latest)
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl \
&& chmod +x ./kubectl \
&& mv ./kubectl /usr/local/bin/kubectl

# Install helm v2 (latest)
# RUN wget https://get.helm.sh/helm-v2.16.0-linux-amd64.tar.gz \
# && tar -zxvf helm-v2.16.0-linux-amd64.tar.gz \
# && mv ./linux-amd64/helm /usr/local/bin/helm

# Install helm v3 (latest)
RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
&& chmod 700 get_helm.sh \
&& ./get_helm.sh

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]
