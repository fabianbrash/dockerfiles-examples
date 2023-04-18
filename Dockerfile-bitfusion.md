```Example bitfusion build```




````
FROM ubuntu:20.04

LABEL maintainer="Fabian Brash"

RUN apt-get update && apt-get upgrade -y && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends --no-install-suggests -y \
curl \
ca-certificates \
wget \
python3

WORKDIR /opt/bitfusion
RUN wget https://packages.vmware.com/bitfusion/ubuntu/20.04/bitfusion-client-ubuntu2004_4.5.3-4_amd64.deb

RUN apt-get update && apt-get install -y ./bitfusion-client-ubuntu2004_4.5.3-4_amd64.deb


````
