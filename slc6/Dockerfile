FROM cern/slc6-base
LABEL maintainer="marko.petric@cern.ch"
ENV container docker

# Install base packages
COPY packages.txt /tmp/packages
RUN yum install -y yum-plugin-ovl \
    && yum update -y \
    && yum install -y $(cat /tmp/packages) \
    && rm -fv /tmp/packages

# Install HEP_OSlibs
COPY heposlibs-1.1.10-1.el6.x86_64.rpm /tmp/heposlibs.rpm
RUN yum install -y /tmp/heposlibs.rpm \
    && yum clean all \
    && rm -fv /tmp/heposlibs.rpm

# Install distro module (and pip3) for python3
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py \
    && python3 get-pip.py \
    && pip3 install distro \
    && rm -fv get-pip.py

# Set the correct timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# Run bash default command
CMD ["/bin/bash"]
