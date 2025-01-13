# phyBUILDER

This repository contains the build containers we use in our infrastructure to build BSPs and software components.
They get pushed to docker.io, so you can use them directly on your development machine or in a ci loop:
https://hub.docker.com/u/phybuilder

## Overview

We provide different container for different software projects. This is a quick
overview of the use-cases of the different images:

| Containers | Use Cases / Content |
|------------|-------------------|
| yocto-debian-*<br>yocto-ubuntu-* | • Build our Yocto BSP manually or in CI<br>• Run phyLinux<br>• Bitbake build and shell<br>• Cover all host dependencies |
| zephyr-ubuntu-* | Build a Zephyr RTOS board. This image includes all required host dependencies |
| zephyr-*-ubuntu-* | Like zephyr-ubuntu-* but also includes the official Zephyr SDK |
| action-runner-* | Run github actions from different projects in self hosted runners |
| python-ubuntu-* | Image containing several python versions. It is used to build different python based projects |

## Run the prebuild container

If you have not pulled any of our container right now, you can run:

### Latest available containers:

```bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-18.04 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-20.04 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-22.04 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-debian-12 bash
```

### Available containers with special tags:

```bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-18.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-20.04:phy2 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-22.04:phy2 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-debian-12:phy1 bash
```

If you want to run a container with docker, only remove "--userns=keep-id":

```bash
docker run --rm=true -v /home:/home --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04 bash
```

## Build the container locally

You can also use the provided container files to build them locally:

```bash
podman build -t yocto-ubuntu-16.04:phy1 yocto-ubuntu-16.04/
podman build -t yocto-ubuntu-18.04:phy1 yocto-ubuntu-18.04/
podman build -t yocto-ubuntu-20.04:phy2 yocto-ubuntu-20.04/
podman build -t yocto-ubuntu-22.04:phy2 yocto-ubuntu-22.04/
podman build -t yocto-debian-12:phy1 yocto-debian-12/
podman build -t zephyr-ubuntu-22.04:phy1 zephyr-ubuntu-22.04/
podman build -t zephyr-0.16.y-ubuntu-22.04:phy1 zephyr-0.16.y-ubuntu-22.04/
podman build -t action-runner-ubuntu-22.04:phy2 action-runner-ubuntu-22.04/
```

## Run the local / already pulled container

```bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-16.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-18.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-20.04:phy2 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-22.04:phy2 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-debian-12:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it zephyr-ubuntu-22.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it zephyr-0.16.y-ubuntu-22.04:phy1 bash
```

## Push local container to a registry

When a container image changed content and we want to update the build image on the registry, just use the release script. It will check what is the newest tag and also update the "latest" tag.

```bash
scripts/release yocto-ubuntu-22.04/Containerfile
```

## Known issues

1. When using rootless podman on some host distros or kernel versions, you might get the following error:

```
ERROR: Task (...) failed with exit code '1'
Pseudo log:
path mismatch [1 link]: ino 33756398 db '/tmp/sh-thd.OrwpmG' req '/tmp/sh-thd.gJsVnF'.
Setup complete, sending SIGUSR1 to pid 449.
```

The issue has been discussed here:
https://groups.google.com/g/kas-devel/c/Dm3OcBS-yao

The workaround is to add the following cmdline option to podman: `--tmpfs /tmp`