# Using TF-Seq2Seq via Docker

This directory contains `Dockerfile`s to make it easy to get up and running with
TF-Seq2Seq via [Docker](https://www.docker.com/).


## Installing Docker

General installation instructions are
[on the Docker site](https://docs.docker.com/), but we give some
quick links here:

* [Docker for Mac OSX](https://www.docker.com/docker-mac)
* [Docker for Windows PC](https://www.docker.com/docker-windows)
* [Docker for Debian](https://www.docker.com/docker-debian)
* [Docker for Ubuntu](https://www.docker.com/docker-ubuntu)
* [Docker for CentOS Distribution](https://www.docker.com/docker-centos-distribution)


## Dockerfile

At the moment we haven't deployed an automatic pipeline to build container, so we provide the Dockerfile to build images
This directory is structured to maintain Dockerfile for python2.7(py27) and python3.5(py35).

* `Dockerfile` - TF-Seq2Seq - CPU only!

* `Dockerfile.devel` - Developer build for TF-Seq2Seq - CPU only!

* `Dockerfile.gpu` - TF-Seq2Seq with support of NVidia CUDA

* `Dockerfile.devel-gpu` - Developer build for TF-Seq2Seq with support of NVidia CUDA

The file provided, are coded from the [official TensorFlow Docker directory](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/docker/README.md) and with **latest** TensorFlow version/tags.


## Build image

To build the container's image run in the directory of your favorite python version(some_path/seq2seq/seq2seq/tools/docker/{python-version}/):

	$ docker build -t image_name -f Dockerfile.suffix .


Some examples
```bash
# In py27 dir
$ docker build -t py27 -f Dockerfile .
$ docker build -t py27-devel-gpu -f Dockerfile.devel-gpu .

# In py35 dir
$ docker build -t py35-devel -f Dockerfile.devel .
$ docker build -t py35-gpu -f Dockerfile.gpu .
```

To build a container with a certain TensorFlow version, look at the tags/version in the [Docker Store](https://store.docker.com/community/images/tensorflow/tensorflow/tags), then change the first line's tags of the Dockerfile with your choice:

Some examples:
```
# In py27 Dockerfile, with the TensorFlow 1.0.1 version
FROM tensorflow/tensorflow:latest --Became--> FROM tensorflow/tensorflow:1.0.1

#In py35 Dockerfile.devel-gpu, with the TensorFlow 1.1.0-rc2 version
FROM tensorflow/tensorflow:latest-devel-gpu-py3 --Became-->	1.2.0-rc2-devel-gpu-py3
```

Once a container is built, you will find the Tf-Seq2Seq package in the `/src/seq2seq` path(the default workdir).

## Running container

Run non-GPU container using

    $ docker run -it -p hostPort:containerPort image_name(provided during the building step)

Some examples
```bash
# Run a container with the Tf-Seq2Seq package in a py27 developer env
$ docker run -it py27-devel

# Run a container with the Tf-Seq2Seq package in a py35 env and look at the result with TensorBoard
$ docker run -it -p 6006:6006 py35

# Run a container with the Tf-Seq2Seq package in a py27 env and work at a Tf-seq2seq package cloned in the Host through the container
$ docker run -it -v $(pwd):/seq2seq -w /seq2seq py27
```

For GPU support install NVidia drivers (ideally latest) and
[nvidia-docker](https://github.com/NVIDIA/nvidia-docker). Run using

    $ nvidia-docker run -it -p hostPort:containerPort {repository_name}(provided during the building step)

The examples are the same as above with the only difference that the command is `nvidia-docker`.

Note: If you would have a problem running nvidia-docker you may try the old method
we have used. But it is not recommended. If you find a bug in nvidia-docker, please report
it there and try using nvidia-docker as described above.

```bash
    $ export CUDA_SO=$(\ls /usr/lib/x86_64-linux-gnu/libcuda.* | xargs -I{} echo '-v {}:{}')
    $ export DEVICES=$(\ls /dev/nvidia* | xargs -I{} echo '--device {}:{}')
    $ docker run -it -p 8888:8888 $CUDA_SO $DEVICES gcr.io/tensorflow/tensorflow:latest-gpu
```
