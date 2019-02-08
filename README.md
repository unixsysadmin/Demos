# Demos
Repository is a location of user demos for technologies listed on github.com/containers

`Building` directory is for Demos related to building container images
`Running` directory is for Demos related to running containers.
`Security` directory is for Demos related to containers security.

# Requirements

A small 'scratch' Virtual Machine is perfect for this.  2GB of free space in /var for the container storage should be sufficient.

## Fedora

An up to date Fedora 29 with access to the standard repositories.

## RHEL 7

An up to date RHEL 7 system with the following software installed:

* podman 0.12 or greater (rhel-7-server-extras-rpms repo)
* buildah 1.5 or greater (rhel-7-server-extras-rpms repo)
* docker 1.13 or greater (rhel-7-server-extras-rpms repo)

Register the server with Red Hat or your local Satellite instance so you can consume packages.

# Setup

Create a demo user with full sudo rights and switch to the account:
~~~
sudo useradd demo
echo "demo   ALL=(ALL) ALL" | sudo tee -a /etc/sudoers
sudo su - demo
~~~

Extract the demo repository and enjoy!

# Cleanup

The script should clean itself up, but if needed:

~~~
sudo rm -rf ~demo/.local
~~~

Remove any docker containers and images:

~~~
sudo docker rm $(sudo docker ps -a -q)
sudo docker rmi $(sudo docker images -q)
sudo systemctl restart docker
~~~

# Known issues

* [Bugzilla 1498628 shadow-utils: Update to get newuidmap and newgidmap binaries](https://bugzilla.redhat.com/show_bug.cgi?id=1498628).  Newer versions of shadow-utils are required on RHEL 7, the following may be displayed by buildah:
~~~
ERRO[0000] error reading allowed ID mappings: error reading subuid mappings for user "demo" and subgid mappings for group "demo": No subuid ranges found for user "demo" in /etc/subuid
~~~
