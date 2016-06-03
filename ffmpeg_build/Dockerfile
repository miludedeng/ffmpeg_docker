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
    FFMPEG_VERSION=1.2.11 \
    VO_AACENC_VERSION=0.1.3

COPY ffmpeg/a52dec-${A52DEC_VERSION}.tar.gz /opt/source/
COPY ffmpeg/faac-${FAAC_VERSION}.tar.gz /opt/source/
COPY ffmpeg/faad2-${FAAD2_VERSION}.tar.gz /opt/source/
COPY ffmpeg/lame-${LAME_VERSION}.tar.gz /opt/source/
COPY ffmpeg/last_x264.tar /opt/source/
COPY ffmpeg/xvidcore-${XVIDCORE_VERSION}.tar.gz /opt/source/
COPY ffmpeg/yasm-${YASM_VERSION}.gz /opt/source/
COPY ffmpeg/vo-aacenc_${VO_AACENC_VERSION}.orig.tar.gz /opt/source/
COPY ffmpeg/ffmpeg-${FFMPEG_VERSION}.tar.gz /opt/source/
COPY CentOS-Base.repo /etc/yum.repo.d/
WORKDIR /opt/source/

RUN tar -zxvf lame-${LAME_VERSION}.tar.gz \
    && tar -zxvf yasm-${YASM_VERSION}.gz \
    && tar -zxvf xvidcore-${XVIDCORE_VERSION}.tar.gz \
    && tar -xvf last_x264.tar \
    && tar -zxvf a52dec-${A52DEC_VERSION}.tar.gz \
    && tar -zxvf faac-${FAAC_VERSION}.tar.gz \
    && tar -zxvf faad2-${FAAD2_VERSION}.tar.gz \
    && tar -zxvf vo-aacenc_${VO_AACENC_VERSION}.orig.tar.gz \
    && tar -zxvf ffmpeg-${FFMPEG_VERSION}.tar.gz 

RUN yum -y install gcc gcc-c++ make automake autoconf libtool

WORKDIR /opt/source/lame-${LAME_VERSION}

RUN ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/yasm-${YASM_VERSION}

RUN ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/xvidcore-${XVIDCORE_VERSION}/build/generic

RUN ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/x264-snapshot-20160602-2245

RUN ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/a52dec-${A52DEC_VERSION}

RUN ./configure --enable-shared=PKGS \
    && make && make install && ldconfig

WORKDIR /opt/source/faac-${FAAC_VERSION}

RUN ./bootstrap && ./configure --enable-shared  

COPY mpeg4ip.h common/mp4v2/mpeg4ip.h

RUN make && make install && ldconfig

WORKDIR /opt/source/faad2-${FAAD2_VERSION}

RUN chmod +x ./bootstrap \
    && ./bootstrap \
    && ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/vo-aacenc-${VO_AACENC_VERSION}

RUN ./configure --enable-shared \
    && make && make install && ldconfig

WORKDIR /opt/source/ffmpeg-${FFMPEG_VERSION}

RUN ./configure --enable-gpl  --enable-postproc --enable-nonfree --enable-postproc --enable-swscale --enable-avfilter --enable-pthreads --enable-libxvid --enable-libx264 --enable-libmp3lame  --enable-libfaac --enable-libvo-aacenc --enable-version3 \
    && make && make install && ldconfig \
    && rm -rf /opt/source/*
