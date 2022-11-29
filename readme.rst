This repository contains the build containers we use in our infrastructure to build BSPs and software components. They get pushed to docker.io, so you can use them directly on your development machine or in a ci loop.

Run the prebuild container
==========================
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-18.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-20.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-22.04:phy1 bash

Build the container locally
===========================
You can also use the provided container files to build them locally:

| podman build -t yocto-ubuntu-16.04:phy1 yocto-ubuntu-16.04/
| podman build -t yocto-ubuntu-18.04:phy1 yocto-ubuntu-18.04/
| podman build -t yocto-ubuntu-20.04:phy1 yocto-ubuntu-20.04/
| podman build -t yocto-ubuntu-22.04:phy1 yocto-ubuntu-22.04/

Run the local container
=======================
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-16.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-18.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-20.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-22.04:phy1 bash

Push local container to a registry
==================================
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-16.04:phy1 docker.io/phybuilder/yocto-ubuntu-16.04:phy1;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-18.04:phy1 docker.io/phybuilder/yocto-ubuntu-18.04:phy1;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-20.04:phy1 docker.io/phybuilder/yocto-ubuntu-20.04:phy1;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-22.04:phy1 docker.io/phybuilder/yocto-ubuntu-22.04:phy1;unset pass
