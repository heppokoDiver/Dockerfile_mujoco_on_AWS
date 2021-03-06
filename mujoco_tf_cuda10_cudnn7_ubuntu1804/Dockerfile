# We need the CUDA base dockerfile to enable GPU rendering
# on hosts with GPUs.
# The image below is a pinned version of nvidia/cuda:9.1-cudnn7-devel-ubuntu16.04 (from Jan 2018)
# If updating the base image, be sure to test on GPU since it has broken in the past.
FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04


RUN apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl \
    git \
    libgl1-mesa-dev \
    libgl1-mesa-glx \
    libglew-dev \
    libosmesa6-dev \
    software-properties-common \
    net-tools \
    unzip \
    vim \
    virtualenv \
    wget \
    xpra \
    xserver-xorg-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

#================ Docker install==================
RUN apt-get install -y apt-transport-https ca-certificates curl software-properties-common
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable testing edge"
RUN apt-get update
RUN apt-get install -y docker-ce
RUN apt-get install -y docker-ce-cli
RUN apt-get install -y containerd.io

#================ others ==================
RUN DEBIAN_FRONTEND=noninteractive add-apt-repository --yes ppa:deadsnakes/ppa && apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install --yes python3.5-dev python3.5 python3-pip
RUN virtualenv --python=python3.5 env

RUN rm /usr/bin/python
RUN ln -s /env/bin/python3.5 /usr/bin/python
RUN ln -s /env/bin/pip3.5 /usr/bin/pip
RUN ln -s /env/bin/pytest /usr/bin/pytest

RUN curl -o /usr/local/bin/patchelf https://s3-us-west-2.amazonaws.com/openai-sci-artifacts/manual-builds/patchelf_0.9_amd64.elf \
    && chmod +x /usr/local/bin/patchelf

ENV LANG C.UTF-8

RUN mkdir -p /root/.mujoco \
    && wget https://www.roboti.us/download/mjpro150_linux.zip -O mujoco.zip \
    && unzip mujoco.zip -d /root/.mujoco \
    && rm mujoco.zip
COPY ./mjkey.txt /root/.mujoco/
ENV LD_LIBRARY_PATH /root/.mujoco/mjpro150/bin:${LD_LIBRARY_PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib64:${LD_LIBRARY_PATH}

RUN mkdir /root/code
WORKDIR /root/code
WORKDIR /mujoco_py
RUN pip install --upgrade pip

RUN apt-get install -y zlib1g-dev swig cmake

# install seems cleaner
RUN pip install glfw>=1.4.0
#RUN pip install numpy>=1.11
RUN pip install numpy
RUN pip install Cython>=0.27.2
RUN pip install imageio>=2.1.2
RUN pip install cffi>=1.10
RUN pip install imagehash>=3.4
RUN pip install ipdb
RUN pip install Pillow>=4.0.0
RUN pip install pycparser>=2.17.0
RUN pip install pytest>=3.0.5
RUN pip install pytest-instafail==0.3.0
RUN pip install scipy>=0.18.0
RUN pip install sphinx
RUN pip install sphinx_rtd_theme
RUN pip install numpydoc
RUN pip install cloudpickle==1.2.2
RUN pip install cached-property==1.3.1
RUN pip install lockfile
RUN pip install gym[all]
RUN pip install gitpython==2.1.7
RUN pip install gtimer==1.0.0b5
RUN pip install awscli
RUN pip install boto3
RUN pip install ray==0.2.2
RUN pip install path.py==10.3.1
RUN pip install joblib==0.9.4
RUN pip install opencv-python==3.4.0.12
RUN pip install sk-video==1.1.10
RUN pip install matplotlib
RUN pip install tensorflow-gpu==1.15.0
RUN apt-get -y update 
RUN apt-get -y upgrade
RUN apt-get install -y python3-pip
RUN apt-get install -y build-essential libssl-dev libffi-dev python-dev
#RUN pip3 install python-binance
RUN pip install colorlog
RUN pip install pandas
