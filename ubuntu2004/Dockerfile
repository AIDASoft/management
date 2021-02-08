FROM ubuntu:20.04
LABEL maintainer="marko.petric@cern.ch"

# Suppress debconf messages for user interaction during installations
COPY misc/keyboard /etc/default/keyboard
ENV DEBIAN_FRONTEND noninteractive

# Install krb5.conf (before installing krb5-user)
COPY krb5.conf/common /etc/krb5.conf

# Install native packages
COPY packages.txt /tmp/packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends apt-utils \
    && apt-get upgrade -y \
    && apt-get install -y $(cat /tmp/packages) \
    && rm -fv /tmp/packages \
    && locale-gen en_US.UTF-8 \
    && rm -rfv /var/lib/apt/lists/*

# Install network file transfer programs
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get install -y wget \
    && rm -fv /tmp/packages \
    && rm -rfv /var/lib/apt/lists/*

# Xrootd client from CERN debian repository
RUN echo "deb http://storage-ci.web.cern.ch/storage-ci/debian/xrootd/ focal release" > /etc/apt/sources.list.d/xrootd.list
RUN curl -sL http://storage-ci.web.cern.ch/storage-ci/storageci.key -o /tmp/storageci.key \
    && APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1 apt-key add /tmp/storageci.key \
    && rm -fr /tmp/storageci.key
RUN apt-get update && apt-get install -y xrootd-client

# Set the correct timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# Add some subfolders in the $HOME folder
RUN mkdir /root/.ssh \
    && mkdir /root/.ccache

# Setup SSH configuration
COPY misc/config /root/.ssh/config
RUN chmod 600 /root/.ssh/config

# Setup ccache
RUN mkdir -p /ccache
COPY misc/ccache.conf /root/.ccache/ccache.conf
RUN cp $( which ccache ) /usr/local/bin \
    && ln -s /usr/local/bin/ccache /usr/local/bin/gcc \
    && ln -s /usr/local/bin/ccache /usr/local/bin/g++ \
    && ln -s /usr/local/bin/ccache /usr/local/bin/cc \
    && ln -s /usr/local/bin/ccache /usr/local/bin/c++

# Run bash as default command
CMD ["/bin/bash"]
