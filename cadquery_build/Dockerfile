FROM ghcr.io/phijn/images-cq_lib_base:latest AS cadquery_build_base
WORKDIR /opt/cadquery
# COPY ./cadquery .
RUN git clone https://github.com/CadQuery/cadquery.git /opt/cadquery \
    && git checkout 4c6f968ac1e411a53d20779309778e1b4a585fa3
#COPY ./conda-bld /opt/anaconda/conda-bld

COPY --from=build_conda_package /opt/anaconda/conda-bld /opt/anaconda/conda-bld
RUN sed -e 's/\"cadquery-ocp/#\"cadquery-ocp/'  setup.py > _setup.py \
    && sed -e 's/use_scm_version=.*,/use_scm_version=False,/' _setup.py > setup.py \
    && sed -e 's/defaults/defaults\n  - local/' environment.yml > _environment.yml && mv _environment.yml environment.yml

FROM cadquery_build_base as cadquery_build
ENV export PACKAGE_VERSION=2.1
RUN source /opt/anaconda/bin/activate \
    && conda install -y conda-build \
    && conda create -y -n conda-build -y python='3.9'

RUN source /opt/anaconda/bin/activate conda-build \
    && PYTHON_VERSION='3.9' conda build -c conda-forge -c local conda