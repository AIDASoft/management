FROM ghcr.io/aidasoft/centos7:latest
LABEL maintainer="marko.petric@cern.ch"
ENV container docker

# Install coverity files
COPY cov-analysis-linux64 /cov-analysis-linux64

# Add Coverity to path
ENV PATH="/cov-analysis-linux64/bin:${PATH}"

# Run bash default command
CMD ["/bin/bash"]
