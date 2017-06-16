FROM tensorflow/tensorflow:latest-py3

MAINTAINER Alessio Gozzoli <agozzoli.ai@gmail.com>

RUN apt-get update && apt-get install -y \
			python-tk \
			python3-tk \
			git \
			&& \
		apt-get clean && \
		rm -rf /var/lib/apt/lists/*

# Install Seq2Seq Dependencies
WORKDIR /
RUN pip3.5 --no-cache-dir install -e git+https://github.com/google/seq2seq.git#egg=seq2seq && \
    pip3.5 --no-cache-dir install \
    			nose \
    			pylint \
    			tox \
    			yapf \
    			mkdocs

# Set Matploblib Backend
RUN mkdir -p /root/.config/matplotlib/ && \
			touch /root/.config/matplotlib/matplotlibrc && \
			echo "backend : Agg" >> /root/.config/matplotlib/matplotlibrc

# Default workdir
WORKDIR "/src/seq2seq"

CMD ["/bin/bash"]
