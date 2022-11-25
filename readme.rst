Build the container
===================
podman build -t yocto-ubuntu-20.04:phy1 yocto-ubuntu-20.04/
podman build -t yocto-ubuntu-22.04:phy1 yocto-ubuntu-22.04/

Run the container
=================
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-20.04:phy1 bash
podman run --rm=true -v /home:/home --userns=keep-id --workdir=$PWD -it yocto-ubuntu-22.04:phy1 bash


