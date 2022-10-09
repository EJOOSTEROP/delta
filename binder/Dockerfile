ARG BASE_CONTAINER=yikunkero/spark:latest

FROM $BASE_CONTAINER as spark

LABEL maintainer="Andrew Bauman"

FROM spark as delta

USER root

ARG DELTA_CORE_VERSION="2.1.0"

RUN pip install --no-cache-dir delta-spark==${DELTA_CORE_VERSION} \
notebook jupyterlab

FROM delta as startup

ARG NBuser=NBuser
ARG NB_UID=1000
ENV USER ${NBuser}
ENV NB_UID ${NB_UID}
ARG GROUP=NBuser
ARG WORKDIR=/opt/spark/work-dir
ENV DELTA_PACKAGE_VERSION=delta-core_2.12:2.1.0

RUN groupadd -r ${GROUP} && useradd -r -m -g ${GROUP} ${NBuser} --uid ${NB_UID}

COPY --chown=${NBuser} ./binder/quickstart_binder.ipynb "${HOME}"
RUN chown -R ${NBuser}:${GROUP} /home/${NBuser}/ \
&& chown -R ${NBuser}:${GROUP} ${WORKDIR}

USER ${NBuser}
