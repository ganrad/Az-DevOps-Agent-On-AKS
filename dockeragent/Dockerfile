# Azure DevOps Pipeline Agent : 08-16-2019
#
# Description: This dockerfile authored by Microsoft has been modified to include the following build tools
#   - docker-ce-cli
#   - kubectl
#   - helm
#
FROM ubuntu:16.04
MAINTAINER Ganesh Radhakrishnan ganrad01@gmail.com

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
        libcurl3 \
        libicu55 \
        libunwind8 \
        netcat \
        zip \
        unzip \
        wget \
        apt-transport-https \
        gnupg-agent \
        software-properties-common

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

RUN apt-get update && apt-get install docker-ce-cli

# Install all build tools in /build-tools
WORKDIR /build-tools

# Install kubectl v1.15.x (latest)
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl \
&& chmod +x ./kubectl \
&& mv ./kubectl /usr/local/bin/kubectl

# Install helm v2.14.3 (latest)
RUN wget https://storage.googleapis.com/kubernetes-helm/helm-v2.14.3-linux-amd64.tar.gz \
&& tar -zxvf helm-v2.14.3-linux-amd64.tar.gz \
&& mv ./linux-amd64/helm /usr/local/bin/helm

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]
