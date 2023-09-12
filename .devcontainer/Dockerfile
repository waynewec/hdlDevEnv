FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=ETC/UTC

# Re-synchronize the OS package index
RUN apt-get update
RUN apt-get upgrade
## Install Python with --enable-shared
RUN apt-get install -y python python3-pip

RUN pip3 install --upgrade pip

# Install extra packages
RUN pip3 install -U pytest flake8 pylint pytype mypy gcovr cherrypy dowser


# Install all needed packages for all simulators
RUN apt-get update
RUN apt-get install -y git perl make flex gnat gtkwave swig autoconf g++ bison libfl2 libfl-dev ccache libgoogle-perftools-dev numactl perl-doc
RUN apt-get install -y gperf
RUN rm -rf /var/lib/apt/lists/*

## Install Icarus Verilog
RUN git clone https://github.com/steveicarus/iverilog.git \
    && cd iverilog \
    && autoconf \
    && ./configure \
    && make --silent \
    && make --silent install \
    && cd .. \
    && rm -rf iverilog

## Install Verilator
#ENV VERILATOR_BRANCH=stable
ENV VERILATOR_BRANCH=v4.106

RUN git clone https://github.com/verilator/verilator.git --branch ${VERILATOR_BRANCH} verilator \
    && unset VERILATOR_ROOT \
    && cd verilator \
    && autoconf \
    && ./configure \
    && make --silent \
    && make --silent install \
    && cd .. \
    && rm -rf verilator

## Install GHDL
ENV GHDL_BRANCH=v1.0.0
RUN git clone https://github.com/ghdl/ghdl.git --depth=1 --branch ${GHDL_BRANCH} ghdl \
    && cd ghdl \
    && ./configure \
    && make --silent \
    && make --silent install \
    && cd .. \
    && rm -rf ghdl

# Install cvc
RUN git clone https://github.com/cambridgehackers/open-src-cvc.git --depth=1 cvc \
    && cd cvc/src \
    && make -f makefile.cvc64 --silent \
    && cp cvc64 /usr/local/bin \
    && cd ../.. \
    && rm -rf cvc

RUN pip3 install cocotb[bus]

#Teros HDL req
RUN pip3 install vunit edalize