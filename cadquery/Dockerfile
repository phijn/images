FROM ghcr.io/phijn/images-cq_lib_base:latest AS cadquery
COPY --from=cadquery_build /opt/anaconda/conda-bld /opt/anaconda/conda-bld
SHELL [ "/bin/bash", "-c" ]

WORKDIR /home/cadquery
RUN source /opt/anaconda/bin/activate \
&& conda create -y -n cadquery \
&& conda activate --no-stack cadquery \
&& conda install --use-local -y -c conda-forge -c local cadquery python='3.9' ocp

RUN source /opt/anaconda/bin/activate \
    && conda activate --no-stack  cadquery \
    && conda clean -y -a \
    && du -hs /opt/anaconda

CMD source /opt/anaconda/bin/activate && conda init && conda activate cadquery && echo "Welcome to cadquery" && /usr/bin/bash