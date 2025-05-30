# container-image.provisioning

[![start-stop-containers](https://github.com/mazgi/container-image.provisioning/actions/workflows/start-stop-containers.yaml/badge.svg)](https://github.com/mazgi/container-image.provisioning/actions/workflows/start-stop-containers.yaml)

Language|Build Status|Container Images
---|---|---
provisioning|[![provisioning](https://github.com/mazgi/container-image.provisioning/actions/workflows/provisioning.yaml/badge.svg)](https://github.com/mazgi/container-image.provisioning/actions/workflows/provisioning.yaml)|[ghcr.io/mazgi/provisioning](https://github.com/mazgi/container-image.provisioning/pkgs/container/provisioning)

## How to Use

### Write out your IDs and information in the .env file

If you have an old `.env` file, you are able to reset it by removing it.

```console
rm -f .env
```

:information_source: If you are using Linux, write out UID, GID, and GID for the `docker` group, into the `.env` file to let that as exported on Docker Compose as environment variables.

```console
test $(uname -s) = 'Linux' && {
  echo -e "DOCKER_GID=$(getent group docker | cut -d : -f 3)"
  echo -e "GID=$(id -g)"
  echo -e "UID=$(id -u)"
} >> .env || :
```

## Supplementary Information

### Environment Variable Names

Environment variable names and uses are as follows.

| Name       | Required on Linux | Value                                                                                                                                   |
| ---------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| UID        | **Yes**           | This ID number is used as UID for your Docker user, so this ID becomes the owner of all files and directories created by the container. |
| GID        | **Yes**           | The same as the above UID.                                                                                                              |
| DOCKER_GID | **Yes**           | This ID number is used to provide permission to read and write your docker socket on your local machine from your container.            |
