ARG BUILD_FROM
FROM $BUILD_FROM

# File Author / Maintainer
MAINTAINER Michael Hansen

ARG MAKE_THREADS=8

COPY etc/qemu-arm-static /usr/bin/
COPY etc/qemu-aarch64-static /usr/bin/

COPY download/openfst-1.6.9.tar.gz /
COPY download/opengrm-ngram-1.3.4.tar.gz /
COPY download/phonetisaurus-2019.tar.gz /

RUN apt-get update &&  \
    apt-get install -y build-essential python3-dev checkinstall

RUN cd / && tar -xf openfst-1.6.9.tar.gz && cd openfst-1.6.9/ && \
    ./configure --prefix=/build --enable-far --enable-static=no --enable-bin=no --enable-shared --enable-ngram-fsts && \
    make -j $MAKE_THREADS && \
    make install

ENV CPPFLAGS=-I/build/include
ENV LDFLAGS=-L/build/lib

RUN cd / && tar -xf opengrm-ngram-1.3.4.tar.gz && cd opengrm-ngram-1.3.4/ && \
   CXXFLAGS="-I/build/include" LDFLAGS="-L/build/lib" \
   ./configure --prefix=/build && \
   make -j $MAKE_THREADS && \
   make install

RUN cd / && tar -xf phonetisaurus-2019.tar.gz && cd phonetisaurus/ && \
    ./configure --prefix=/build \
       --with-openfst-includes=/build/include \
       --with-openfst-libs=/build/lib && \
    make -j $MAKE_THREADS && \
    make install

RUN find /build -name '*.a' -delete
RUN strip --strip-all /build/lib/*.so*