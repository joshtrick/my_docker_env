FROM ubuntu:18.04

MAINTAINER hao jiang (hao.jiang@corerain.com)
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NOWARNINGS yes
ENV TERM xterm-256color
ARG MAKE_THREADS=4

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    autoconf \
    automake \
    build-essential \
    curl \
    ctags \
    ca-certificates \
    dirmngr \
    freeglut3-dev \
    gnupg2 \
    gpg-agent \
    graphviz \
    gawk \
    git \
    id-utils \
    libssl-dev \
    libpq-dev \
    libtool \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    libgtk-3-dev \
    libcairo2-dev \
    libmtdev-dev \
    libxkbcommon-x11-0 \
    libglu1-mesa-dev \
    libatlas-base-dev \
    libboost-all-dev \
    libhdf5-serial-dev \
    libleveldb-dev \
    liblmdb-dev \
    libsnappy-dev \
    mesa-common-dev \
    net-tools \
    openssh-server \
    pkg-config\
    python3 \
    python3-dev \
    python3-pip \
    sudo \
    software-properties-common \
    unzip \
    virtualenv \
    vim \
    wget \
    xauth \
    xclip \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN mkdir -p ~/tmp/

# Install cmake
RUN cd ~/tmp/ \
    && wget -O cmake-3.14.7-Linux-x86_64.tar.gz \
    https://cmake.org/files/v3.14/cmake-3.14.7-Linux-x86_64.tar.gz \
    && tar xzf cmake-3.14.7-Linux-x86_64.tar.gz \
    && mv cmake-3.14.7-Linux-x86_64 /opt/cmake-3.14.7 \
    && ln -sf /opt/cmake-3.14.7/bin/* /usr/bin/ \
    && rm -rf ~/tmp/*


# Install OpenCV for C and C++
RUN cd ~/tmp/ \
    && wget -O opencv-3.4.11.zip \
    https://codeload.github.com/opencv/opencv/zip/3.4.11 \
    && ls \
    && unzip opencv-3.4.11.zip \
    && cd opencv-3.4.11 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_PROTOBUF=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DWITH_CUBLAS=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_CUFFT=OFF \
    -DWITH_OPENCL=OFF \
    -DWITH_OPENCLAMDBLAS=OFF \
    -DWITH_OPENCLAMDFFT=OFF \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf ~/tmp/*

# Install exvim
RUN cd ~/tmp/ \
    && git clone https://github.com/exvim/main \
    && cd main \
    && sh unix/install.sh \
    && sh unix/replace-my-vim.sh \
    && sed -i "45s/.*/        silent exec 'language C.UTF-8'/" ~/.vimrc \
    && sed -i "50s/call/\" call/" ~/.vimrc.plugins \
    && sed -i "51s/call/\" call/" ~/.vimrc.plugins \
    && rm -rf ~/tmp/*

# Change download sources
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bk \
    && sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

RUN rm -rf ~/tmp

WORKDIR ~
# Finished
# To build an image:
# docker build -t <image_name>:<image_tag> -f <Dockerfile_name> .
