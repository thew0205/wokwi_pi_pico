# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.177.0/containers/cpp/.devcontainer/base.Dockerfile

# [Choice] Debian / Ubuntu version: debian-10, debian-9, ubuntu-20.04, ubuntu-18.04
ARG VARIANT="ubuntu-24.04"

FROM mcr.microsoft.com/vscode/devcontainers/cpp:$VARIANT

# [Optional] Uncomment this section to install additional packages.
RUN apt-get update \
    && apt-get -y install cmake build-essential wget ca-certificates \
    gdb-multiarch automake autoconf libtool libftdi-dev libusb-1.0-0-dev pkg-config clang-format libhidapi-dev \
    python3.12 python3.12-venv python3-pip python3.12-dev \
    git tio gcc-arm-none-eabi libnewlib-arm-none-eabi g++ libstdc++-arm-none-eabi-newlib inetutils-ping
WORKDIR /tinyml


# RUN apt-get install -y libjim-dev


# # # # RUN git clone https://github.com/openocd-org/openocd.git --depth=1 --recurse-submodules \
# RUN git clone https://github.com/raspberrypi/openocd.git --depth=1 --recurse-submodules \
#     && cd openocd && ./bootstrap && ./configure --disable-werror && make -j8 install 

# RUN git clone -b master https://github.com/raspberrypi/pico-sdk.git --depth=1 && git -C pico-sdk submodule update --init
# ENV PICO_SDK_PATH=/tinyml/pico-sdk

# RUN cd /tinyml && git clone https://github.com/raspberrypi/picotool.git --depth=1 \
#     && cd picotool && mkdir build && cd build && cmake ../ && make -j8 && cp picotool /usr/local/bin/

# RUN usermod -aG plugdev vscode

# #2. Clone the repository
WORKDIR /tinyml
RUN git clone -b release/1.1 https://github.com/pytorch/executorch.git

# # 3. Set up the environment and run installation
WORKDIR /tinyml/executorch
RUN python3.12 -m venv .venv
# We use the full path to python/pip within the venv instead of "activating" 
# because activation doesn't persist across RUN layers.
RUN .venv/bin/pip install --upgrade pip

# We use bash to run the script, and we "activate" the venv in the same line
RUN /bin/bash -c "source .venv/bin/activate && ./install_executorch.sh"

# WORKDIR /tinyml/projects

# # COPY blink_wokwi /tinyml/projects/blink_wokwi
# # COPY pico_executorch /tinyml/projects/pico_executorch

# ENV EXECUTORCH_ROOT=/tinyml/executorch/
 

