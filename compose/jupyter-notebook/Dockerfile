FROM jupyter/minimal-notebook


RUN pip install psycopg2-binary

# ADD ./requirements.txt ./
COPY --chown=${NB_UID}:${NB_GID} requirements.txt /tmp/
RUN pip install --quiet --no-cache-dir --requirement /tmp/requirements.txt && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN mkdir src
WORKDIR src/
