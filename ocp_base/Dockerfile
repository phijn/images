FROM ghcr.io/phijn/images-ocp_build:latest AS ocp_build_makefiles
SHELL [ "/bin/bash", "-c" ]
WORKDIR /opt/OCP

RUN source /opt/anaconda/bin/activate cpp-py-bindgen \
    && cmake -DPython3_FIND_VIRTUALENV=ONLY -DPython3_EXECUTABLE=/opt/anaconda/envs/cpp-py-bindgen/bin/python \
    -DPython3_ROOT_DIR=/opt/anaconda/envs/cpp-py-bindgen -B build -S ../OCP -G Ninja -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_C_COMPILER_LAUNCHER="ccache" -DCMAKE_CXX_COMPILER_LAUNCHER="ccache"

FROM ocp_build_makefiles as ocp_base
SHELL [ "/bin/bash", "-c" ]
WORKDIR /opt/OCP
RUN cmake --build build -j 4 -- -k 0; exit 0
RUN cmake --build build -j 2 -- -k 0; exit 0
RUN cmake --build build -- -k 0; exit 0
RUN cmake --build build -- -k 0 \
    && cmake --install build

# test -
RUN source /opt/anaconda/bin/activate \
    && cd build \
    && LD_DEBUG=libs python -c"import OCP;"
