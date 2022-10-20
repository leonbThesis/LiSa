FROM python:3.6-slim

ARG maxmind_key

RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    g++ \
    libpcap-dev \
    make \
    patch \
    git \
    # remove QEMU from apt-get install for manual install
    openvpn \
    binutils \
    iprange \
    wget \
    tar \
    e2tools \
    && git clone https://github.com/radare/radare2 \
    && radare2/sys/install.sh \
    && useradd -m lisa \
    && echo "Downloading LiSa Linux images ..." \
    # Modify linux-images path
    && git clone -b SLV --single-branch https://github.com/leonbThesis/linux-images \
    && mv linux-images /home/lisa/images

COPY --chown=lisa:lisa ./data /home/lisa/data
COPY --chown=lisa:lisa ./docker /home/lisa/docker
COPY --chown=lisa:lisa ./lisa /home/lisa/lisa
COPY --chown=lisa:lisa ./requirements.txt /home/lisa/requirements.txt

ENV PYTHONPATH /home/lisa

# get additional packages for manual QEMU install (https://wiki.qemu.org/Hosts/Linux)
RUN apt-get install -y \
    libglib2.0-dev \
    libfdt-dev \
    libpixman-1-dev \
    zlib1g-dev \
    bzip2 \
    ninja-build

# get modified QEMU version
RUN git clone -b SLV --single-branch https://github.com/leonbThesis/qemu

# manually build QEMU
WORKDIR qemu

RUN mkdir build

WORKDIR build

RUN ../configure --target-list=x86_64-softmmu,mips-softmmu,i386-softmmu,arm-softmmu,aarch64-softmmu \
    && make -j4 \
    && mv qemu-system-x86_64 /usr/bin \
    && mv qemu-system-mips /usr/bin \ 
    && mv qemu-system-i386 /usr/bin \
    && mv qemu-system-arm /usr/bin \
    && mv qemu-system-aarch64 /usr/bin

WORKDIR /home/lisa

RUN pip install -r requirements.txt \
    && iprange -j data/blacklists/* > data/ipblacklist \
    && ./docker/worker/maxmind.sh $maxmind_key \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    git \
    gcc \
    g++ \
    make \
    patch \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /radare2/.git

CMD ["./docker/worker/init.sh"]
