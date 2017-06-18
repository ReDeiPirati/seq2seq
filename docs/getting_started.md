## Download & Setup

To use tf-seq2seq you need a working installation of TensorFlow 1.0 with
Python 2.7 or Python 3.5. Follow the [Installing TensorFlow](https://www.tensorflow.org/install/) guide for detailed setup instructions. With TensorFlow installed, you can clone this repository:

```bash
git clone https://github.com/google/seq2seq.git
cd seq2seq

# Install package and dependencies
pip install -e .
```

## TF-Seq2Seq via Docker

Now is possible to run [tf-seq2seq via Docker](https://github.com/google/seq2seq/seq2seq/tools/docker). Unfortunately, we haven't deployed an automatic pipeline to build container, so we provide the Dockerfile to build images for your repository.

### Build Image
First of all you have to build the image for running tf-seq2seq, according to your python preference(python2.7 or python3.5) and hardware capabilities(with or without GPU) from the right Dockerfile

* [Docker for Python 2.7](https://github.com/google/seq2seq/seq2seq/tools/docker/py27)
* [Docker for Python 3.5](https://github.com/google/seq2seq/seq2seq/tools/docker/py35)

Both the directories have Dockerfile to build image with basic depenecies(Dockerfile), development dependecies(Dockerfile.devel) and GPU support(Dockerfile.gpu and Dockerfile.devel-gpu). The file provided, are coded from the [official TensorFlow Docker directory](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/docker/README.md) with **latest** TensorFlow version/tags.
Now you are ready to build the image(you have to be in py27 or py35 dir to run this command):

```bash
$ docker build -t image_name -f Dockerfile.suffix .
```

We give you some examples:
```bash
# In py27 dir
$ docker build -t py27 -f Dockerfile .
$ docker build -t py27-devel-gpu -f Dockerfile.devel-gpu .

# In py35 dir
$ docker build -t py35-devel -f Dockerfile.devel .
$ docker build -t py35-gpu -f Dockerfile.gpu .
```

### Build Custom Image
To build a container with a certain TensorFlow version, look at the tags/version in the [Docker Store](https://store.docker.com/community/images/tensorflow/tensorflow/tags), then change the first line's tags of the Dockerfile with your choice and run the `docker build` command.

Some examples:
```
# In py27 Dockerfile, with the TensorFlow 1.0.1 version
FROM tensorflow/tensorflow:latest --Became--> FROM tensorflow/tensorflow:1.0.1

#In py35 Dockerfile.devel-gpu, with the TensorFlow 1.1.0-rc2 version
FROM tensorflow/tensorflow:latest-devel-gpu-py3 --Became-->	1.2.0-rc2-devel-gpu-py3
```

Once a container is built, you will find the Tf-Seq2Seq package in the `/src/seq2seq` path(the default workdir).

### Running container

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

## Validate your installation

To make sure everything works as expect you can run a simple pipeline unit test:

```bash
python -m unittest seq2seq.test.pipeline_test
```

Or the full test pipeline(you have to download nose if you haven't installed the tf-seq2seq via Docker):
```bash
nosetests
```

If you see a "OK" message, you are all set. Note that you may need to install pyrouge, pyyaml, and matplotlib, in order for these tests to pass. If you run into other setup issues,
please [file a Github issue](https://github.com/google/seq2seq/issues).

## Common Installation Issues

### Incorrect matploblib backend

In order to generate plots using matplotlib you need to have set the correct [backend](http://matplotlib.org/faq/usage_faq.html#what-is-a-backend). Also see this [StackOverflow thread](http://stackoverflow.com/questions/4930524/how-can-i-set-the-backend-in-matplotlib-in-python). To use the `Agg` backend, simply:

```
echo "backend : Agg" >> $HOME/.config/matplotlib/matplotlibrc
```

## Next Steps

- Learn about [concepts and terminology](concepts.md)
- Read through the [Neural Machine Translation Tutorial](nmt.md)
- Use [pre-processed datasets](data.md) or train a model on your own data
- [Contribute!](contributing.md)
