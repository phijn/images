FROM ghcr.io/phijn/images-conda_build:latest AS cq_lib_base
RUN apt-get update --allow-insecure-repositories \
    && DEBIAN_FRONTEND=noninteractiv apt-get install -y \
        libegl1-mesa libglu1-mesa freeglut3 \
        libvtk9.1 \
        rapidjson-dev \
        git \
    && rm -rf /var/lib/apt/lists/*
