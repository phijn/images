FROM ghcr.io/phijn/images-ocp_base:latest as build_conda_package
WORKDIR /opt/OCP
RUN source /opt/anaconda/bin/activate \
    && conda install -y conda-build \
    && conda create -y -n conda-build -y python='3.9'

RUN source /opt/anaconda/bin/activate \
    && conda activate conda-build \
    && PYTHON_VERSION='3.9' conda build -c conda-forge -c conda-forge/label/occt_rc conda