FROM ubuntu:24.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    ninja-build \
    make \
    git \
    pkg-config \
    && \
    rm -rf /var/lib/apt/lists/*
