FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get -y install git


RUN apt-get install -y apt-utils locales

#INSTALL esp-open-sdk

#install depencies
RUN apt-get -y install make unrar-free autoconf automake libtool gcc g++ gperf \
    flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial \
    sed git unzip bash help2man wget bzip2 libtool-bin


#crosstool-NG must not be executed by root therefor we create and change to a new user
RUN useradd -ms /bin/bash builder
USER builder
WORKDIR /home/builder

RUN git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
WORKDIR  /home/builder/esp-open-sdk
RUN make toolchain esptool libhal STANDALONE=n  -j 12
#RUN export PATH="/home/builder/esp-open-sdk/xtensa-lx106-elf/bin:$PATH"
ENV PATH /home/builder/esp-open-sdk/xtensa-lx106-elf/bin:$PATH
# check out esp-open-rtos
WORKDIR /home/builder
RUN git clone --recursive https://github.com/Superhouse/esp-open-rtos.git
# RUN export SDK_PATH="/home/builder/esp-open-rtos"
ENV SDK_PATH="/home/builder/esp-open-rtos"
# check out esp-homekit-dome and compile ledtest
WORKDIR /home/builder
RUN git clone --recursive https://github.com/maximkulkin/esp-homekit-demo
WORKDIR /home/builder/esp-homekit-demo
# copy wifi sample just to compile
RUN cp wifi.h.sample wifi.h
RUN make -C examples/led all

USER root
RUN apt-get install -y vim
