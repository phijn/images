FROM ghcr.io/phijn/images-conda_build:latest AS ocp_copy
RUN apt-get update --allow-insecure-repositories \
    && DEBIAN_FRONTEND=noninteractiv apt-get install -y \
        mesa-common-dev libegl1-mesa-dev libgl1-mesa-dev libglu1-mesa-dev freeglut3-dev \
        libvtk9-dev \
        qtcreator qtbase5-dev \
        rapidjson-dev \
        git \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update --allow-insecure-repositories \
    && apt-get install -y software-properties-common

RUN add-apt-repository -y universe

RUN apt-get update --allow-insecure-repositories \
    && apt-get install -y \
        wget \
        build-essential \
        cmake \
        clang \
        lldb \
        lld \
        ccache \
        ninja-build \
    && apt-get -y upgrade \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/CadQuery/OCP.git /opt/OCP \
    && cd /opt/OCP \
    && git submodule update --init --recursive \
    && git checkout 6b7b7325ab4599a8ba9049f176f099574fe64dfc
