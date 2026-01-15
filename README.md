# Docker Image Packaging for AlmaLinux

<a href="https://alvistack.com" title="AlviStack" target="_blank"><img src="/alvistack.svg" height="75" alt="AlviStack"></a>

[![GitLab pipeline
status](https://img.shields.io/gitlab/pipeline/alvistack/docker-almalinux/master)](https://gitlab.com/alvistack/docker-almalinux/-/pipelines)
[![GitHub
tag](https://img.shields.io/github/tag/alvistack/docker-almalinux.svg)](https://github.com/alvistack/docker-almalinux/tags)
[![GitHub
license](https://img.shields.io/github/license/alvistack/docker-almalinux.svg)](https://github.com/alvistack/docker-almalinux/blob/master/LICENSE)
[![Docker
Pulls](https://img.shields.io/docker/pulls/alvistack/almalinux-10.svg)](https://hub.docker.com/r/alvistack/almalinux-10)

AlmaLinux OS is an open-source, community-driven Linux operating system that fills the gap left by the discontinuation of the CentOS Linux stable release. AlmaLinux OS is an Enterprise Linux distro, binary compatible with RHEL®, and guided and built by the community.

As a standalone, completely free OS, AlmaLinux OS enjoys \$1M in annual sponsorship from CloudLinux Inc. and support from more than 25 other sponsors. Ongoing development efforts are governed by the members of the community.

Learn more about AlmaLinux: <https://almalinux.org/>

## Supported Tags and Respective Packer Template Links

- [`alvistack/almalinux-10`](https://hub.docker.com/r/alvistack/almalinux-10)
  - [`packer/almalinux-10-docker/packer.json`](https://github.com/alvistack/docker-almalinux/blob/master/packer/almalinux-10-docker/packer.json)
- [`alvistack/almalinux-9`](https://hub.docker.com/r/alvistack/almalinux-9)
  - [`packer/almalinux-9-docker/packer.json`](https://github.com/alvistack/docker-almalinux/blob/master/packer/almalinux-9-docker/packer.json)
- [`alvistack/almalinux-8`](https://hub.docker.com/r/alvistack/almalinux-8)
  - [`packer/almalinux-8-docker/packer.json`](https://github.com/alvistack/docker-almalinux/blob/master/packer/almalinux-8-docker/packer.json)

## Overview

This Docker container makes it easy to get an instance of SSHD up and
running with AlmaLinux.

Based on [Official AlmaLinux Docker
Image](https://hub.docker.com/_/almalinux/) with some minor hack:

- Packaging by Packer Docker builder and Ansible provisioner in single
  layer
- Handle `ENTRYPOINT` with
  [catatonit](https://github.com/openSUSE/catatonit)
- Handle `CMD` with SSHD

### Quick Start

Start SSHD:

    # Pull latest image
    docker pull alvistack/almalinux-10

    # Run as detach
    docker run \
        -itd \
        --name almalinux \
        --publish 2222:22 \
        alvistack/almalinux-10

**Success**. SSHD is now available on port `2222`.

Because this container **DIDN'T** handle the generation of root
password, so you should set it up manually with `pwgen` by:

    # Generate password with pwgen
    PASSWORD=$(docker exec -i almalinux pwgen -cnyB1); echo $PASSWORD

    # Inject the generated password
    echo "root:$PASSWORD" | docker exec -i almalinux chpasswd

Alternatively, you could inject your own SSH public key into container's
authorized_keys by:

    # Inject your own SSH public key
    (docker exec -i almalinux sh -c "cat >> /root/.ssh/authorized_keys") < ~/.ssh/id_rsa.pub

Now you could SSH to it as normal:

    ssh root@localhost -p 2222

## Versioning

### `YYYYMMDD.Y.Z`

Release tags could be find from [GitHub
Release](https://github.com/alvistack/docker-almalinux/tags) of this
repository. Thus using these tags will ensure you are running the most
up to date stable version of this image.

### `YYYYMMDD.0.0`

Version tags ended with `.0.0` are rolling release rebuild by [GitLab
pipeline](https://gitlab.com/alvistack/docker-almalinux/-/pipelines) in
weekly basis. Thus using these tags will ensure you are running the
latest packages provided by the base image project.

## License

- Code released under [Apache License 2.0](LICENSE)
- Docs released under [CC BY
  4.0](http://creativecommons.org/licenses/by/4.0/)

## Author Information

- Wong Hoi Sing Edison
  - <https://twitter.com/hswong3i>
  - <https://github.com/hswong3i>
