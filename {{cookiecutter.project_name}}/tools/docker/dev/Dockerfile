#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM ubuntu:20.04

ARG SHELL=/bin/bash
ARG SHELL_PACKAGE=bash
# Configure apt and install packages
RUN    DEBIAN_FRONTEND=noninteractive \
         apt-get update \
    && DEBIAN_FRONTEND=noninteractive \
         apt-get -y install apt-utils \
                            build-essential \
                            curl \
                            dialog \
                            git \
                            iproute2 \
                            lldb \
                            lsb-release \
                            pkg-config \
                            python3 \
                            sudo \
                            $SHELL_PACKAGE 2>&1 \
    && DEBIAN_FRONTEND=noninteractive \
         apt-get autoremove -y \
    && DEBIAN_FRONTEND=noninteractive \
         apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user to use
ARG USER=user
ARG UID=1000
ARG GID=$UID
ARG DOCKER_GID=100
RUN    groupadd --gid $GID $USER \
    && groupadd --gid $DOCKER_GID docker \
    && useradd -m -s $SHELL --uid $UID --gid $GID -G $DOCKER_GID $USER \
    # Add sudo support for the non-root user
    && echo $USER ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USER\
    && chmod 0440 /etc/sudoers.d/$USER

USER $USER

# Install Rust
ARG RUST_VERSION={{ cookiecutter.rust_version }}
ENV RUSTUP_HOME=/home/$USER/.rustup
ENV CARGO_HOME=/home/$USER/.cargo
ENV PATH="${PATH}:${CARGO_HOME}/bin"
RUN    mkdir ${RUSTUP_HOME} \
    # Install rust
    && curl https://sh.rustup.rs -sSf | bash -s -- \
      -y \
      --no-modify-path \
      --default-toolchain ${RUST_VERSION} \
      --component rls \
      --component rust-analysis \
      --component rust-src

WORKDIR /home/$USER

CMD ["tail", "-f", "/dev/null"]
