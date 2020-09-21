FROM ubuntu:18.04
LABEL maintainer="marko.petric@cern.ch"

# Suppress debconf messages for user interaction during installations
COPY keyboard /etc/default/keyboard
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
    && locale-gen en_US.UTF-8

# Install HEP_OSlibs
COPY heposlibs-7.2.11-2.bionic_amd64.deb /tmp/heposlibs.deb
RUN apt-get install -y /tmp/heposlibs.deb \
    && rm -fv /tmp/heposlibs.deb \
    && rm -rfv /var/lib/apt/lists/*

# Set the correct timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# Run bash as default command
CMD ["/bin/bash"]
