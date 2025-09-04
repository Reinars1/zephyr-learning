FROM ubuntu:24.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive
ENV ZEPHYR_TOOLCHAIN_VARIANT=zephyr
ENV ZEPHYR_SDK_INSTALL_DIR=/opt/zephyr-sdk
ENV PATH=$ZEPHYR_SDK_INSTALL_DIR/sysroots/x86_64-pokysdk-linux/usr/bin:$PATH

ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG ZEPHYR_SDK_VERSION=0.16.8
ARG ZEPHYR_SDK_URL=https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v${ZEPHYR_SDK_VERSION}/zephyr-sdk-${ZEPHYR_SDK_VERSION}_linux-x86_64_minimal.tar.xz

# Install dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    clangd \
    ninja-build \
    make \
    git \
    python3 \
    python3-pip \
    python3-venv \
    device-tree-compiler \
    wget \
    udev \
    xz-utils \
    gcc-multilib \
    g++-multilib \
    gperf \
    ccache \
    && rm -rf /var/lib/apt/lists/*

# Install Zephyr SDK
RUN wget -q $ZEPHYR_SDK_URL -O /tmp/zephyr-sdk.tar.xz && \
    mkdir -p $ZEPHYR_SDK_INSTALL_DIR && \
    tar -xf /tmp/zephyr-sdk.tar.xz -C $ZEPHYR_SDK_INSTALL_DIR --strip-components=1 && \
    rm /tmp/zephyr-sdk.tar.xz && \
    $ZEPHYR_SDK_INSTALL_DIR/setup.sh -t all -c

# Install west + Python deps
RUN pip3 install --upgrade pip && \
    pip3 install west && \
    west init /workspace && \
    cd /workspace && west update && \
    pip3 install -r zephyr/scripts/requirements.txt

# Create non-root user
RUN groupadd --gid $USER_GID user && \
    useradd --uid $USER_UID --gid $USER_GID -m user && \
    chown -R user:user /workspace

USER user
WORKDIR /workspace

CMD ["/bin/bash"]

