FROM cern/cs8-base
LABEL maintainer="marko.petric@cern.ch"
ENV container docker

# Prepare centos to use systemd
RUN dnf update -y \
    && dnf install -y systemd \
    && dnf clean all \
    && (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
        rm -fv /lib/systemd/system/multi-user.target.wants/*; \
        rm -fv /etc/systemd/system/*.wants/*; \
        rm -fv /lib/systemd/system/local-fs.target.wants/*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*udev*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*initctl*; \
        rm -fv /lib/systemd/system/basic.target.wants/*; \
        rm -fv /lib/systemd/system/anaconda.target.wants/*; \
        rm -fv /usr/lib/tmpfiles.d/systemd-nologin.conf;

# Install krb5.conf (before installing krb5-workstation)
COPY krb5.conf/rhel /etc/krb5.conf

# Install base packages
COPY packages.txt /tmp/packages
RUN dnf install -y which
RUN dnf install -y epel-release
RUN dnf install -y dnf-plugin-ovl

RUN dnf update -y
RUN dnf install -y $(cat /tmp/packages)

RUN rm -fv /tmp/packages

# Default python is python 2
RUN alternatives --set python /usr/bin/python2

# Some generators need f77 defined
RUN ln -sf /usr/bin/gfortran /usr/bin/f77

# Install HEP_OSlibs
# COPY builder/centos8/heposlibs-7.2.11-3.el7.cern.x86_64.rpm /tmp/heposlibs.rpm
#RUN dnf install --nobest --skip-broken -y /tmp/heposlibs.rpm \
#    && dnf clean all \
#    && rm -fv /tmp/heposlibs.rpm

# Set the correct timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# Add some subfolders in the $HOME folder
RUN mkdir /root/.ssh \
    && mkdir /root/.ccache

# Setup SSH configuration
COPY misc/config /root/.ssh/config
RUN chmod 600 /root/.ssh/config

# Setup ccache
#RUN mkdir -p /ccache
#COPY misc/ccache.conf /root/.ccache/ccache.conf
#RUN cp $( which ccache ) /usr/local/bin \
#    && ln -s /usr/local/bin/ccache /usr/local/bin/gcc \
#    && ln -s /usr/local/bin/ccache /usr/local/bin/g++ \
#    && ln -s /usr/local/bin/ccache /usr/local/bin/cc \
#    && ln -s /usr/local/bin/ccache /usr/local/bin/c++

# Run bash default command
CMD ["/bin/bash"]
