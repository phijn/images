FROM ghcr.io/phijn/images-ocp_copy:latest AS ocp_build
SHELL [ "/bin/bash", "-c" ]

# COPY ./OCP /opt/OCP
WORKDIR /opt/OCP
ENV CONDA_ENV="env.yml"
ENV CPP_PY_BINDGEN=/opt/anaconda/envs/cpp-py-bindgen

RUN source /opt/anaconda/bin/activate \
    && sed -e s/python=.../python='3.9'/  env.yml > _env.yml \
    && conda init \
    && conda env create -f _env.yml \
    && conda install -n cpp-py-bindgen -y python='3.9'