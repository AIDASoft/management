ARG BASE_TAG
FROM cern/cs9-base
LABEL maintainer="andresailer"
ENV container docker

# Prepare centos to use systemd and add epel-release
# Install base packages, needs epel-release to be installed

COPY packages.txt /tmp/packages

RUN dnf update -y \
    && dnf install -y systemd epel-release \
    && (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
        rm -fv /lib/systemd/system/multi-user.target.wants/*; \
        rm -fv /etc/systemd/system/*.wants/*; \
        rm -fv /lib/systemd/system/local-fs.target.wants/*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*udev*; \
        rm -fv /lib/systemd/system/sockets.target.wants/*initctl*; \
        rm -fv /lib/systemd/system/basic.target.wants/*; \
        rm -fv /lib/systemd/system/anaconda.target.wants/*; \
        rm -fv /usr/lib/tmpfiles.d/systemd-nologin.conf; \
    dnf install -y which $(cat /tmp/packages) \
    && dnf clean all \
    && rm -fv /tmp/packages

# Install krb5.conf (before installing krb5-workstation)
COPY krb5.conf/rhel /etc/krb5.conf

# Add some subfolders in the $HOME folder
RUN mkdir -p /root/.ssh \
    && mkdir -p /root/.ccache

# Run bash as default command
CMD ["/bin/bash"]
