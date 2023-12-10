FROM ubuntu:24.04 AS wget
RUN apt-get -qq update \
    && apt-get -qq install -y wget \
    && rm -rf /var/lib/apt/lists/*

ARG PYTHON_VERSION='3.9'
ARG ANACONDA3_VERSION='2022.10'

### getting conda for different plattforms
FROM wget AS anaconda-arm64
WORKDIR /tmp/
RUN wget -q "https://repo.anaconda.com/archive/Anaconda3-$ANACONDA3_VERSION-Linux-aarch64.sh" -O "./anaconda3.sh"

FROM wget AS anaconda-amd64
WORKDIR /tmp/
RUN wget -q "https://repo.anaconda.com/archive/Anaconda3-$ANACONDA3_VERSION-Linux-x86_64.sh" -O "./anaconda3.sh"

FROM wget AS anaconda-ppc64le
WORKDIR /tmp/
RUN wget -q "https://repo.anaconda.com/archive/Anaconda3-$ANACONDA3_VERSION-Linux-$ARCH.sh" -O "./anaconda3.sh"

FROM wget AS anaconda-s390x
WORKDIR /tmp/
RUN wget -q "https://repo.anaconda.com/archive/Anaconda3-$ANACONDA3_VERSION-Linux-$ARCH.sh" -O "./anaconda3.sh"


FROM anaconda-${TARGETARCH} as conda_build
SHELL [ "/bin/bash", "-c" ]
ENV CONDA_INSTALL_DIR /opt/anaconda
WORKDIR /tmp/
RUN /bin/bash ./anaconda3.sh -b -p${CONDA_INSTALL_DIR} \
    && rm ./anaconda3.sh

RUN source ${CONDA_INSTALL_DIR}/bin/activate \
    && conda init \
    && conda update -y conda
