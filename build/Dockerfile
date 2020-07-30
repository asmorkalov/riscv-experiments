FROM ubuntu:18.04

# CMake PPA to get modern CMake
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        apt-transport-https \
        gnupg \
        software-properties-common \
        wget && \
    wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor | tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null && \
    apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main' && \
    apt-get update && \
    apt-get clean

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        gcc \
        g++ \
        git \
        make \
        cmake \
        python \
        python3 \
        gcc-multilib \
        autoconf \
        automake \
        autotools-dev \
        curl \
        libmpc-dev \
        libmpfr-dev \
        libgmp-dev \
        gawk \
        build-essential \
        bison \
        flex \
        texinfo \
        gperf \
        libtool \
        patchutils \
        bc \
        zlib1g-dev \
        libexpat-dev && \
    apt-get clean

RUN mkdir /riscv && cd /riscv && \
    git clone https://github.com/riscv/riscv-gnu-toolchain.git && \
    cd riscv-gnu-toolchain && \
    git checkout rvv-0.9.x && \
    git submodule update --init --recursive && \
    ./configure --prefix=/riscv/riscv-gnu-toolchain-install --with-arch=rv64gcv --with-abi=lp64d --enable-linux && \
    make -j16 && \
    cd .. && rm -rf riscv-gnu-toolchain

RUN cd /riscv && git clone https://github.com/isrc-cas/rvv-llvm.git && \
    mkdir rvv-llvm-build && cd rvv-llvm-build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;RISCV" -DLLVM_ENABLE_PROJECTS=clang -DCMAKE_INSTALL_PREFIX=/riscv/rvv-llvm-install ../rvv-llvm/llvm && \
    make -j16 && make install && \
    cd .. && rm -rf rvv-llvm rvv-llvm-build

RUN useradd ci -m -s /bin/bash -G users
USER ci

CMD bash
