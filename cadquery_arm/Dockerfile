FROM szymanski2adesso/cadquery-client:arm64v8

RUN source ${CONDA_INSTALL_DIR}/bin/activate \
    && conda activate cadquery \
    && pip install jupyter-cadquery==3.5.2 cadquery-massembly==1.0.0 camunda-external-task-client-python3 pydantic \
    && conda activate cadquery 