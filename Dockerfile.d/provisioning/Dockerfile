FROM ubuntu:24.04

LABEL maintainer="docker@mazgi.com"
LABEL org.opencontainers.image.source="https://github.com/mazgi/container-image.provisioning"

# Set in non-interactive mode.
ARG DEBIAN_FRONTEND=noninteractive
# https://docs.docker.com/engine/reference/builder/#automatic-platform-args-in-the-global-scope
ARG TARGETARCH
ARG TARGETPLATFORM
# https://www.terraform.io/downloads.html
ARG TERRAFORM_VERSIONS=1.11.4,1.11.0,1.10.5,1.9.8,1.8.5,1.2.5,1.2.2,1.2.1,1.1.8,1.1.7,1.1.6,1.0.6,0.15.5

RUN echo 'apt::install-recommends "false";' > /etc/apt/apt.conf.d/no-install-recommends\
  && apt-get update\
#   && :
# RUN :\
  && : Set up locales\
  && apt-get install --assume-yes locales procps dialog\
  && echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen\
  && locale-gen\
#   && :
# RUN :\
  && : Install basic packages\
  && apt-get install --assume-yes sudo apt-utils curl dnsutils git jq openssh-client tmux unzip zsh\
  && apt-get install --assume-yes apt-transport-https ca-certificates gnupg gnupg2 rsync software-properties-common\
  && apt-get install --assume-yes vim\
  && apt-get install --assume-yes nodejs npm\
  && apt-get install --assume-yes software-properties-common\
#   && :
# RUN :\
  && : Install packages related K8s\
  # https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
  && curl -o /usr/local/bin/kubectl -L "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/${TARGETARCH}/kubectl"\
  && chmod a+x /usr/local/bin/kubectl\
  # https://helm.sh/docs/intro/install/#from-script
  && curl -fsSL -o /tmp/get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3\
  && chmod 700 /tmp/get_helm.sh\
  && /tmp/get_helm.sh\
#   && :
# RUN :\
  && : Add the APT Repository [1Password CLI]\
  # See https://developer.1password.com/docs/cli/get-started/
  && curl --fail --silent --show-error --location https://downloads.1password.com/linux/keys/1password.asc\
  | gpg --no-default-keyring --keyring /usr/share/keyrings/1password-archive-keyring.gpg --import -\
  && echo\
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/1password-archive-keyring.gpg] https://downloads.1password.com/linux/debian/$(dpkg --print-architecture) stable main"\
  | tee /etc/apt/sources.list.d/1password.list > /dev/null\
  && mkdir -p /etc/debsig/policies/AC2D62742012EA22/\
  && curl --fail --silent --show-error --location https://downloads.1password.com/linux/debian/debsig/1password.pol\
  | tee /etc/debsig/policies/AC2D62742012EA22/1password.pol\
  && mkdir -p /usr/share/debsig/keyrings/AC2D62742012EA22/\
  && curl --fail --silent --show-error --location https://downloads.1password.com/linux/keys/1password.asc\
  | gpg --dearmor --output /usr/share/debsig/keyrings/AC2D62742012EA22/debsig.gpg\
#   && :
# RUN :\
  && : Add the APT Repository [Ansible]\
  # See https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
  && add-apt-repository --yes --update ppa:ansible/ansible\
#   && :
# RUN :\
  && : Add the APT Repository [Docker]\
  # See https://docs.docker.com/engine/install/ubuntu/
  && curl --fail --silent --show-error --location https://download.docker.com/linux/ubuntu/gpg\
  | gpg --no-default-keyring --keyring /usr/share/keyrings/docker-archive-keyring.gpg --import -\
  && echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"\
  | tee /etc/apt/sources.list.d/docker.list > /dev/null\
#   && :
# RUN :\
  && : Add the APT Repository [Google Cloud]\
  # See https://cloud.google.com/sdk/docs/install#deb
  && curl --fail --silent --show-error --location https://packages.cloud.google.com/apt/doc/apt-key.gpg\
  | gpg --no-default-keyring --keyring /usr/share/keyrings/cloud.google.gpg --import -\
  && echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main"\
  | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list\
#   && :
# RUN :\
  && : Add the APT Repository [Hashicorp Terraform]\
  # See https://developer.hashicorp.com/terraform/install
  && curl --fail --silent --show-error --location https://apt.releases.hashicorp.com/gpg\
  | gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --import -\
  && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"\
  | tee -a /etc/apt/sources.list.d/hashicorp.list\
# && :
# RUN :\
  && : Add the APT Repositories [Microsoft Azure]\
  # See https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt
  && curl --fail --silent --show-error --location https://packages.microsoft.com/keys/microsoft.asc\
  | gpg --no-default-keyring --keyring /usr/share/keyrings/microsoft-keyring.gpg --import -\
  && echo\
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/microsoft-keyring.gpg] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main"\
  | tee /etc/apt/sources.list.d/azure-cli.list > /dev/null\
  && echo\
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/microsoft-keyring.gpg] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod/ $(lsb_release -cs) main"\
  | tee /etc/apt/sources.list.d/microsoft-debian-prod.list > /dev/null\
# && :
# RUN :\
  && : Install Packages from Added Repositories\
  && apt-get update\
  # 1Password CLI
  && apt-get install --assume-yes 1password-cli\
  # Ansible
  && apt-get install --assume-yes ansible\
  # Azure CLI
  && apt-get install --assume-yes azure-cli\
  # && apt-get install --assume-yes powershell\
  # && pwsh -c 'Install-Module -AllowClobber -Force -Name Az'\
  # Docker
  && apt-get install --assume-yes containerd.io docker-ce docker-ce-cli docker-compose-plugin\
  # Google Cloud
  && apt-get install --assume-yes google-cloud-sdk google-cloud-sdk-gke-gcloud-auth-plugin\
#   && :
# RUN :\
  && : Install AWS CLI v2 for ${TARGETARCH}\
  && test "${TARGETARCH}" = "amd64" && curl --output /tmp/awscliv2.zip --fail --silent --show-error --location "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" || : NOP\
  && test "${TARGETARCH}" = "arm64" && curl --output /tmp/awscliv2.zip --fail --silent --show-error --location "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" || : NOP\
  && unzip /tmp/awscliv2.zip -d /tmp/\
  && /tmp/aws/install\
  && rm -rf /tmp/awscliv2.zip /tmp/aws/\
#   && :
# RUN :\
  && : Install Firebase CLI using Node npm\
  && npm install -g firebase-tools\
#   && :
# RUN :\
  && : Install starship\
  # See https://starship.rs/guide/#%F0%9F%9A%80-installation
  && curl --output "/tmp/install.sh" --fail --silent --show-error --location "https://starship.rs/install.sh"\
  && sh /tmp/install.sh --yes\
  && rm -rf /tmp/install.sh\
#   && :
# RUN :\
  && : Install Terraform using tfenv\
  # Install tfenv
  && git clone --depth=1 https://github.com/tfutils/tfenv.git /usr/local/tfenv\
  && ls -1 /usr/local/tfenv/bin/ | xargs -IB ln -s /usr/local/tfenv/bin/B /usr/local/bin/B\
  # Install Terraform via tfenv
  && echo $TERRAFORM_VERSIONS | tr ',' '\n' | xargs -IV tfenv install V\
  # use the beginning version on the list
  && tfenv use ${TERRAFORM_VERSIONS%%,*}\
#   && :
# RUN :\
  && : Scan GitHub ssh keys\
  && mkdir -p ~/.ssh/\
  && ssh-keyscan github.com 2> /dev/null >> ~/.ssh/known_hosts\
#   && :
# RUN :\
  && : Place empty .zshrc\
  && touch /etc/skel/.zshrc\
#   && :
# RUN :\
  && : Clean up\
  && apt-get clean autoclean\
  && apt-get autoremove --yes\
  && rm -rf /tmp/*\
  && rm -rf /var/lib/apt/\
  && rm -rf /var/log/*\
  && :

COPY rootfs /

# Reset DEBIAN_FRONTEND to default(`dialog`).
# If you no need `dialog`, you can set `DEBIAN_FRONTEND=readline`.
# see also: man 7 debconf
ENV DEBIAN_FRONTEND=
