FROM cern/cc7-base
LABEL maintainer="marko.petric@cern.ch"
ENV container docker

# Prepare centos to use systemd
RUN yum update -y \
    && yum install -y systemd \
    && yum clean all \
    && (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
        rm -fv /lib/systemd/system/multi-user.target.wants/*; \
        rm -fv /etc/systemd/system/*.wants/*; \
        rm -fv /lib/systemd/system/local-fs.target.wants/*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*udev*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*initctl*; \
        rm -fv /lib/systemd/system/basic.target.wants/*; \
        rm -fv /lib/systemd/system/anaconda.target.wants/*; \
        rm -fv /usr/lib/tmpfiles.d/systemd-nologin.conf;

# Install base packages
COPY packages.txt /tmp/packages
RUN yum install -y yum-plugin-ovl \
    && yum update -y \
    && yum install -y $(cat /tmp/packages) \
    && rm -fv /tmp/packages

# Install HEP_OSlibs
COPY heposlibs-7.2.11-3.el7.cern.x86_64.rpm /tmp/heposlibs.rpm
RUN yum install -y /tmp/heposlibs.rpm \
    && yum clean all \
    && rm -fv /tmp/heposlibs.rpm

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

# Run bash default command
CMD ["/bin/bash"]
