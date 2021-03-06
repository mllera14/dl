# Using the MMRL Docker image for deep learning

![Docker Pulls](https://img.shields.io/docker/pulls/mmrl/dl.svg?style=popout)

This directory contains `Dockerfile` to make it easy to get up and running with
deep learning via [Docker](http://www.docker.com/).

## Installation

Install the latest nvidia drivers
* [Ubuntu](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu-installation)

### Installing Docker

General installation instructions are
[on the Docker site](https://docs.docker.com/install/), but we give some
quick links here:

* [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [macOS](https://docs.docker.com/docker-for-mac/install/)
* [Windows](https://docs.docker.com/docker-for-windows/install/)

### Installing nvidia-docker

* [Add the nvidia repository](https://nvidia.github.io/nvidia-docker/)
These instructions are for Ubuntu. For other distributions, see [here](https://nvidia.github.io/nvidia-docker/).

```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```

* Install the nvidia-docker2 package

```
sudo apt-get install nvidia-docker2
sudo pkill -SIGHUP dockerd
```

* Verify the installation

`docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi`

You should see something like this showing the GPUs available:

```
Fri Apr 12 16:51:39 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.56       Driver Version: 418.56       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  TITAN Xp            Off  | 00000000:05:00.0  On |                  N/A |
| 23%   30C    P8    10W / 250W |     72MiB / 12192MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  TITAN Xp            Off  | 00000000:09:00.0 Off |                  N/A |
| 23%   27C    P8     9W / 250W |      2MiB / 12196MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1361      G   /usr/lib/xorg/Xorg                            69MiB |
+-----------------------------------------------------------------------------+
```

## Quick start - Running the container

To launch the image with GPU support and mount the present working directory in the container type:

    $ docker run --runtime=nvidia -it --rm --net=host -v $(pwd):/workspace/src mmrl/dl

Then open a browser and enter the following URL if your running the container locally:

    $ http://127.0.0.1:8888

(If you're running the container on a remote server, replace 127.0.0.1 with the name or IP address of the server.)
You will then be asked for a token which you can copy and paste from the terminal output that looks something like this:

```
http://(kraken or 127.0.0.1):8888/?token=5233b03f99e38bf5c5fc045abd65fbe154ef8ae1a48afd2a
```

## Advanced - Building and running your own container

We are using `Makefile` to simplify docker commands within make commands.

Build the container and start a Jupyter Notebook

    $ make notebook

Alternatively, run the container with the new Jupyter lab interface

    $ make lab

Build the container and start an iPython shell

    $ make ipython

Build the container and start a bash

    $ make bash

For GPU support install NVIDIA drivers (ideally latest) and
[nvidia-docker](https://github.com/NVIDIA/nvidia-docker). Run using

    $ make notebook GPU=0 # or [ipython, bash]

Switch between Theano and TensorFlow

    $ make notebook BACKEND=theano
    $ make notebook BACKEND=tensorflow

Mount a volume for external data sets

    $ make DATA=~/mydata

Prints all make tasks

    $ make help

You can change Theano parameters by editing `/docker/theanorc`.

## Advanced - Customising your own container

The above `docker run` command will pull the [ready-made Docker image for deep learning](https://hub.docker.com/r/mmrl/dl) from the [MMRL repository on Docker Hub](https://hub.docker.com/u/mmrl), however you can use it as a base image and customise it to your needs.

Create your own Dockerfile and start with the following line to inherit all the features of the mmrl/dl container:

```
FROM mmrl/dl
```
