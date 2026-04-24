ARG VARIANT="ubuntu-24.04"

FROM mcr.microsoft.com/vscode/devcontainers/cpp:$VARIANT

RUN apt-get update \
    && apt-get -y install cmake build-essential wget ca-certificates \
    gdb-multiarch automake autoconf libtool libftdi-dev libusb-1.0-0-dev pkg-config clang-format libhidapi-dev \
    python3.12 python3.12-venv python3-pip python3.12-dev \
    git tio gcc-arm-none-eabi libnewlib-arm-none-eabi g++ libstdc++-arm-none-eabi-newlib inetutils-ping \
    libjim-dev

RUN usermod -aG plugdev vscode
USER vscode


WORKDIR /tinyml

RUN git clone https://github.com/raspberrypi/openocd.git --depth=1 --recurse-submodules \
    && cd openocd && ./bootstrap && ./configure --disable-werror && make -j8 
    
USER root
RUN cd openocd && make install 
USER vscode


RUN git clone https://github.com/raspberrypi/pico-sdk.git --depth=1 && git -C pico-sdk submodule update --init

ENV PICO_SDK_PATH=/tinyml/pico-sdk

RUN  git clone https://github.com/raspberrypi/picotool.git --depth=1 \
    && cd picotool && mkdir build && cd build && cmake ../ && make -j8
    
    
USER root
RUN cd picotool/build && cp picotool /usr/local/bin/
USER vscode

WORKDIR /tinyml
RUN python3.12 -m venv tinyml_executorch
RUN tinyml_executorch/bin/pip install --upgrade pip
RUN tinyml_executorch/bin/pip install ipykernel && tinyml_executorch/bin/python -m ipykernel install --user --name=tinyml_executorch --display-name "Python (tinyml_executorch)"
RUN echo "source /tinyml/tinyml_executorch/bin/activate" >> /home/vscode/.bashrc

RUN git clone -b release/1.1 https://github.com/pytorch/executorch.git --depth=1

WORKDIR /tinyml/executorch

# We use bash to run the script, and we "activate" the venv in the same line
RUN /bin/bash -c "source /tinyml/tinyml_executorch/bin/activate && ./install_executorch.sh"

RUN /bin/bash -c "source /tinyml/tinyml_executorch/bin/activate && pip install matplotlib"
ENV EXECUTORCH_ROOT=/tinyml/executorch/

WORKDIR /tinyml 

RUN git clone https://github.com/thew0205/tflite-micro.git --depth 1 -b pico_port

WORKDIR /tinyml/tflite-micro 
RUN ./setup_pico.sh
ENV TFLITE_MICRO_PATH=/tinyml/tflite-micro

RUN /bin/bash -c "source /tinyml/tinyml_executorch/bin/activate && pip install tflite-micro"