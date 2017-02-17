[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE.md)

dockerfiles
===========

Compilation of Dockerfiles with automated builds enabled on the [Docker Hub](https://hub.docker.com/u/nlubock/). **Not suitable for production environments.** These images are under continuous development, so breaking changes may be introduced.

All images are based on Ubuntu Core 16.04 LTS, built with minimising size/layers and [best practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/) in mind. Dependencies are indicated left to right e.g. cuda-theano is theano built on top of CUDA. Explicit dependencies are excluded.

Graphical applications
----------------------

Starting graphical (X11) applications is possible with the following commands:

```sh
docker run -it `# Running interactively, but can be replaced with -d for daemons` \
  -e DISPLAY `# Pass $DISPLAY` \
  -v=/tmp/.X11-unix:/tmp/.X11-unix `# Pass X11 socket` \
  --ipc=host `# Allows MIT-SHM` \
  <image>
```

General information on running desktop applications with Docker can be found [in this blog post](https://blog.jessfraz.com/post/docker-containers-on-the-desktop/). You probably will also need to configure the X server host (`xhost`) to [give access](http://wiki.ros.org/docker/Tutorials/GUI). For hardware acceleration on Linux, it is possible to use `nvidia-docker` (with an image built for NVIDIA Docker), although OpenGL is [not fully supported](https://github.com/NVIDIA/nvidia-docker/issues/11).

On Mac OS X, use XQuartz and [allow connections from network clients](https://blogs.oracle.com/OracleWebCenterSuite/entry/running_gui_applications_on_docker). Then the following can be used:

```sh
docker run -it \
  -e DISPLAY=`ifconfig en0 | grep inet | awk '$1=="inet" {print $2}'`:0 `# Use XQuartz network $DISPLAY` \
  --ipc=host \
  <image>
```

Daemonising containers
----------------------

Most containers run as a foreground process. To daemonise (in Docker terminology, detach) such a container it is possible to use:

`docker run -d <image> sh -c "while true; do sleep 1; done"`

It is now possible to access the daemonised container, for example using bash:

`docker exec -it <id> bash`

Sibling containers
------------------

To start containers on the host from within a docker container, the container requires `docker-engine` installed, with the same API version as the Docker daemon on the host. The Docker socket also needs to be mounted inside the container:

`-v /var/run/docker.sock:/var/run/docker.sock`

CUDA
----

All images pull the most recent versions of [CUDA](http://www.nvidia.com/object/cuda_home_new.html) and [cuDNN](https://developer.nvidia.com/cudnn) from [NVIDIA's DockerHub](https://hub.docker.com/r/nvidia/cuda/)

These images need to be run on an Ubuntu host OS with [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker) installed. The driver requirements can be found on the [NVIDIA Docker wiki](https://github.com/NVIDIA/nvidia-docker/wiki/CUDA#requirements).

Acknowledgements
----------------

These Dockerfiles have been modified from [Kaixhin](https://github.com/Kaixhin) source for the original repo is [here](https://github.com/Kaixhin/dockerfiles).
