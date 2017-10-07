namespaces(7)

setns(2)
cgroups(7)

LXD - Ubuntu
===

linuxcontainers.org

Linux Container Daemon
A desktop interface is not easy.
Has an entire OS running in it. Image based.

Derived from LXC.

lxc init
lxc launch ubuntu:16.04 c1
lsc list
lsc exec c1 bash
lxc stop c1
lxc delete c1

LXD images: tarball
metadata.yaml + rootfs/

Can build also with debootstrap or Packer

Docker
======

docs.docker.com

"layered images"
fast, via cache

hub.docker.com

doccker search ubuntu
docker pull buuntu:16.04
docker ls ?
docker run --rm -ti --rm ubuntu:16.04 /bin/bash

set configs with full_local_path:full_path_on_docker_image

Rkt (coreos, qubernetes)
===
coreos.com/rkt/docs/latest

Can run docker images,
"better security"
"first tier support" in Kubernetes

Docker Swarm
============




