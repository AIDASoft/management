FROM ubuntu:24.04
LABEL maintainer="andresailer"

# Suppress debconf messages for user interaction during installations
COPY misc/keyboard /etc/default/keyboard
ENV DEBIAN_FRONTEND noninteractive

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

# Set the correct timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# Make sure we do not use ccache wrapper
RUN update-alternatives --remove c++ /usr/bin/g++
RUN update-alternatives --remove cc /usr/bin/gcc

# Run bash as default command
CMD ["/bin/bash"]
