FROM ubuntu:24.04

LABEL org.opencontainers.image.source="https://github.com/mazgi/container-image.provisioning"

# https://docs.docker.com/engine/reference/builder/#automatic-platform-args-in-the-global-scope
ARG TARGETARCH
ARG TARGETPLATFORM
# Set in non-interactive mode.
ARG DEBIAN_FRONTEND=noninteractive

# https://github.com/ansible-community/ansible-build-data
ARG ANSIBLE_VERSION=10.1.0
# https://www.terraform.io/downloads.html
ARG TERRAFORM_VERSIONS=1.8.5,1.2.5,1.2.2,1.2.1,1.1.8,1.1.7,1.1.6,1.0.6,0.15.5

RUN echo 'apt::install-recommends "false";' > /etc/apt/apt.conf.d/no-install-recommends\
  && apt-get update\
# Todo
  && : Clean up\
  && apt-get clean autoclean\
  && apt-get autoremove --yes\
  && rm -rf /tmp/*\
  && rm -rf /var/lib/apt/\
  && rm -rf /var/log/*\
  && :
