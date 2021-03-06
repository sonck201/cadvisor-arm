# cAdvisor ARM build

cAdvisor docker image build for ARM devices (for example: Raspberry PI).

This package is based on official [gcr.io/cadvisor/cadvisor](https://github.com/google/cadvisor)

## Content

- [How it works](#how-it-works)
- [How to use](#how-to-use)
  - [Docker](#docker)
  - [Custom build](#custom-build)

## How it works

This package compile official [gcr.io/cadvisor/cadvisor](https://github.com/google/cadvisor) package on Raspberry PI with `arm32v7/golang` docker image and build [google/cadvisor](https://github.com/google/cadvisor) as `arm32v6/alpine` image.

## How to use

### With Docker

#### Supported tags and respective `Dockerfile` links

**NOTE:** Tag corresponds to the version of cAdvisor

- `v0.40.0` - [(Dockerfile)](https://github.com/sonck201/cadvisor/blob/a6d644b87e7e2173c9aad47b7f5a102551972713/README.md)
- `v0.38.6`, `latest` - [(Dockerfile)](https://github.com/sonck201/cadvisor/blob/a6d644b87e7e2173c9aad47b7f5a102551972713/README.md)

The best (and recommended) way how to use this package is as [Docker image](https://hub.docker.com/r/sonck201/cadvisor/).

```shell
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  sonck201/cadvisor:latest
```

I trying update build of this package as soon as possible for each [google/cadvisor](https://github.com/google/cadvisor) update, but when you need more actual version I recommend you use custom build.

### Docker Compose Example

```yml
version: '3'

services:
  cadvisor:
    image: sonck201/cadvisor
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - 8080:8080
```

## Custom build

Or you can use custom build on your ARM (Raspberry PI) device.

```shell
git clone git@github.com:sonck201/cadvisor.git && cd cadvisor
docker build -t <image name> .
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  $IMAGE_NAME:$IMAGE_TAG
```

**IMPORTANT NOTE: Build must be only on ARM device. On x86/x64 CPU not work!**

## References

- https://console.cloud.google.com/gcr/images/cadvisor/GLOBAL/cadvisor
