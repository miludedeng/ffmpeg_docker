FROM centos:7

MAINTAINER Steven <hw@zving.com>

RUN echo '/usr/local/bin' >> /etc/ld.so.conf \
    && echo '/usr/local/lib' >> /etc/ld.so.conf

ENV A52DEC_VERSION=0.7.4 \
    FAAC_VERSION=1.28 \
    FAAD2_VERSION=2.7 \
    LAME_VERSION=3.99.5 \
    XVIDCORE_VERSION=1.1.3 \
    YASM_VERSION=1.2.0 \
    FFMPEG_VERSION=4.2.2 \
    VO_AACENC_VERSION=0.1.3

COPY ffmpeg/* /opt/source/
COPY mpeg4ip.h /opt
COPY CentOS-Base.repo /etc/yum.repo.d/

WORKDIR /opt/source/

RUN yum -y install gcc gcc-c++ make automake autoconf libtool 

RUN tar -zxvf lame-${LAME_VERSION}.tar.gz \
    && tar -zxvf yasm-${YASM_VERSION}.gz \
    && tar -xvf last_x264.tar \
    && tar -zxvf a52dec-${A52DEC_VERSION}.tar.gz \
    && tar -zxvf faad2-${FAAD2_VERSION}.tar.gz \
    && tar -zxvf ffmpeg-${FFMPEG_VERSION}.tar.gz \
    && cd  /opt/source/lame-${LAME_VERSION}\
    && ./configure --enable-shared \
    && make && make install && ldconfig \
    && cd /opt/source/yasm-${YASM_VERSION} \
    && ./configure --enable-shared \
    && make && make install && ldconfig \
    && cd /opt/source/x264-snapshot-20160602-2245 \
    && ./configure --enable-shared \
    && make && make install && ldconfig \
    && cd /opt/source/a52dec-${A52DEC_VERSION} \
    && ./configure --enable-shared=PKGS \
    && make && make install && ldconfig \
    && cd /opt/source/faad2-${FAAD2_VERSION}\
    && chmod +x ./bootstrap \
    && ./bootstrap \
    && ./configure --enable-shared \
    && make && make install && ldconfig \
    && ./configure --enable-shared \
    && make && make install && ldconfig 

RUN cd /opt/source/ffmpeg-${FFMPEG_VERSION}\
    && ./configure --enable-gpl  --enable-postproc --enable-nonfree --enable-postproc --enable-swscale --enable-avfilter --enable-pthreads --enable-libx264 --enable-libmp3lame --enable-version3 \
    && make && make install && ldconfig \
    && rm -rf /opt/source/* \
    && rm -rf /opt/mpeg4ip.h \
    && yum -y remove gcc gcc-c++ make automake autoconf libtool glibc-devel glibc-headers groff-base kernel-headers libgomp libmpc libstdc++-devel m4 mpfr
