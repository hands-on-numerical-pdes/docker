# Copyright (C) 2022-2025 by the Hands On Numerical PDEs authors
#
# This file is part of Hands On Numerical PDEs.
#
# SPDX-License-Identifier: MIT

ARG ARCH

FROM ghcr.io/numericalpdes/h5py:latest-${ARCH} AS h5py

FROM ghcr.io/numericalpdes/base:latest-${ARCH}

ARG ARCH

COPY --from=h5py /etc /etc
COPY --from=h5py /usr /usr
COPY . ${DOCKER_SCRIPTS}

RUN cd ${DOCKER_SCRIPTS} && . scripts/docker_pre.sh && cd - && \
    cd ${INSTALL_SCRIPTS} && \
        if [ "${ARCH}" = "arm64" ]; then sed -i 's|python3 -m pip install --user .\[jupyter\]|echo "Skipped due to vtk not being available on linux arm64"|' vtk/build.sh; fi && \
        bash vtk/build.sh && \
        cd - && \
    cd ${DOCKER_SCRIPTS} && . scripts/docker_post.sh && cd -
