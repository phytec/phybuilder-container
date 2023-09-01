==========
phyBUILDER
==========

This repository contains the build containers we use in our infrastructure to build BSPs and software components.
They get pushed to docker.io, so you can use them directly on your development machine or in a ci loop:
https://hub.docker.com/u/phybuilder

Run the prebuild container
==========================
If you have not pulled any of our container right now, you can run:

* the latest available containers with:

| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-18.04 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-20.04 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-22.04 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-debian-12 bash
|
* an available container with a special tag:

| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-18.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-20.04:phy2 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-22.04:phy2 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it docker.io/phybuilder/yocto-debian-12:phy1 bash
|
If you want to run a container with docker, only remove "--userns=keep-id":

| docker run --rm=true -v /home:/home --workdir=$PWD -it docker.io/phybuilder/yocto-ubuntu-16.04 bash

Build the container locally
===========================
You can also use the provided container files to build them locally:

| podman build -t yocto-ubuntu-16.04:phy1 yocto-ubuntu-16.04/
| podman build -t yocto-ubuntu-18.04:phy1 yocto-ubuntu-18.04/
| podman build -t yocto-ubuntu-20.04:phy2 yocto-ubuntu-20.04/
| podman build -t yocto-ubuntu-22.04:phy2 yocto-ubuntu-22.04/
| podman build -t yocto-debian-12:phy1 yocto-debian-12/
| podman build -t action-runner-ubuntu-22.04:phy2 action-runner-ubuntu-22.04/

Run the local / already pulled container
========================================
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-16.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-18.04:phy1 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-20.04:phy2 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-22.04:phy2 bash
| podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-debian-12:phy1 bash

Push local container to a registry
==================================
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-16.04:phy1 docker.io/phybuilder/yocto-ubuntu-16.04:phy1;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-18.04:phy1 docker.io/phybuilder/yocto-ubuntu-18.04:phy1;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-20.04:phy2 docker.io/phybuilder/yocto-ubuntu-20.04:phy2;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-ubuntu-22.04:phy2 docker.io/phybuilder/yocto-ubuntu-22.04:phy2;unset pass
| 
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/yocto-debian-12:phy1 docker.io/phybuilder/yocto-debian-12:phy1;unset pass
|
| pass=$(cat ~/sync/env/password_dockerhub_phybuilder);podman push --creds phybuilder:${pass} localhost/action-runner-ubuntu-22.04:phy2 docker.io/phybuilder/action-runner-ubuntu-22.04:phy2;unset pass


Known issues
============

1. When using rootless podman on some host distros or kernel versions, you might get the following error:

.. code-block::
        ERROR: Task (...) failed with exit code '1'
        Pseudo log:
        path mismatch [1 link]: ino 33756398 db '/tmp/sh-thd.OrwpmG' req '/tmp/sh-thd.gJsVnF'.
        Setup complete, sending SIGUSR1 to pid 449.

The issue has been discussed here:
https://groups.google.com/g/kas-devel/c/Dm3OcBS-yao

The workaround is to add the following cmdline option to podman: "--tmpfs /tmp"

