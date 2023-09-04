FROM --platform=linux/amd64 python:3.9.17 as general-requirements
# From ubuntu:20.04
# RUN apt-get -y update && apt-get -y install git wget autoconf automake libtool curl make g++ unzip cmake python3 python3-dev python3-sip-dev
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update && apt-get -y install wget git unzip

RUN wget https://github.com/google/protobuf/releases/download/v3.5.0/protobuf-all-3.5.0.zip

RUN git clone https://github.com/Ultimaker/libArcus.git
RUN git clone https://github.com/Ultimaker/CuraEngine.git
RUN git clone https://github.com/Spiritdude/Print3r.git

RUN apt-get -y update && apt-get -y install libtool g++ autoconf automake

RUN apt-get -y update && apt-get -y install cmake

# RUN apt-get -y update && apt-get -y install python3-sip-dev

FROM general-requirements as cura-requirements

# install curaengine
RUN apt-get -y update && apt-get -y install ninja-build
RUN pip install conan==1.56
RUN conan config install https://github.com/ultimaker/conan-config.git
RUN conan profile new default --detect --force

FROM cura-requirements as cura

WORKDIR "/CuraEngine"
RUN git pull
RUN git checkout 5.3.1
RUN conan install . --build=missing --update
RUN cmake --preset release && cmake --build --preset release
RUN . build/generators/conanrun.sh

FROM cura as print3r-requirements

# install print3r
WORKDIR "/Print3r"
RUN git pull
RUN git checkout master
COPY print3r/Makefile.patch ./Makefile.patch
RUN patch < ./Makefile.patch
COPY print3r/print3r.patch ./print3r.patch
RUN patch < ./print3r.patch

FROM print3r-requirements as print3r

# RUN sed -i 's/sudo//g' Makefile
RUN make requirements && make install
# COPY print3r/print3r /usr/bin/print3r
# RUN chmod u+x /usr/bin/print3r
RUN cd settings/slicer/cura5/ && wget https://github.com/Ultimaker/Cura/blob/5.3.1/resources/definitions/fdmprinter.def.json
# RUN cd settings/slicer/cura5/ && wget https://github.com/Ultimaker/Cura/blob/5.3.1/resources/definitions/artillery_sidewinder_x1.def.json

COPY ./profiles/x2.ini /Print3r/settings/printer/x2.ini

RUN ln -s /CuraEngine/build/Release/CuraEngine /usr/local/bin/CuraEngine5

ENV CAMUNDA_URL="https://camunda.philipp-auch.de/engine-rest"
ENV CAMUNDA_PASSWORD="xecfo8-guxrad-cAbmar"
ENV CAMUNDA_USER="worker"

RUN pip install camunda-external-task-client-python3 pydantic

ENV PYTHONPATH=$PYTHONPATH:/home

WORKDIR "/home"

ENTRYPOINT ["/bin/sh", "-c", "python -u /home/cura/workers.py"]

# dc run --rm print3r print3r --printer=x2 --slicer=cura5 --verbose=2 --output=/models/coaster.png render /models/04a2275e-0e9e-11ee-835e-0242ac1c0003_coaster.stl
