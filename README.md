# gitea-dronci-arm-podman
Gitea + DronCI on ARM with podman-compose

podman-compose is exists for [Ubuntu Kinetic](https://packages.ubuntu.com/search?suite=all&arch=arm64&searchon=names&keywords=podman-compose) (22.10)

I am on Ubuntu Jammy (22.04), so:

```
sudo apt install python3 python3-pip
sudo pip3 install --upgrade pip
sudo pip3 install python-dotenv pyyaml podman-compose
podman-compose  version
sudo loginctl enable-linger 1002
```

```

su - podman
podman volume create --driver local --opt type=none --opt device=/opt/nbase/container-volumes/gitea/data --opt o=bind gitea-data
podman volume create --driver local --opt type=none --opt device=/opt/nbase/container-volumes/gitea/config --opt o=bind gitea-config
podman volume create --driver local --opt type=none --opt device=/opt/nbase/container-volumes/drone/data --opt o=bind drone-data
```

bug in podmon v3.4.4

```
mkdir /tmp/SourceMountDocker
mkdir /tmp/SourceMountPodman
podman volume create -o type=bind -o device=/tmp/SourceMountPodman TestVolumePodman
docker volume create -o type=bind -o device=/tmp/SourceMountDocker TestVolumeDocker

podman run --rm -it -v TestVolumePodman:/t alpine:latest
Error: error mounting volume TestVolumeDocker for container 9544d48431315134f9590b46aa8d52f1037b2c36517bf7684104d1a2327225fa: cannot mount volumes without root privileges: operation requires root privileges

podman version
Version:      3.4.4
API Version:  3.4.4
Go Version:   go1.17.3
Built:        Thu Jan  1 01:00:00 1970
OS/Arch:      linux/arm64

docker run --rm -it -v TestVolumeDocker:/t alpine:latest

```

----
- https://docs.gitea.io/en-us/install-with-docker-rootless/
- for runner:
  - https://github.com/containers/podman/blob/main/docs/tutorials/remote_client.md
- https://github.com/docker/compose
- https://compose-spec.io/
- https://software.opensuse.org/download/package?package=podman&project=devel%3Akubic%3Alibcontainers%3Aunstable
- https://docs.oracle.com/en/operating-systems/oracle-linux/podman/podman-ConfiguringStorageforPodman.html#podman-install-storage
- https://github.com/containers/common/blob/main/docs/containers.conf.5.md
