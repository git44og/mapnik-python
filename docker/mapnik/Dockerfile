FROM python:3.7-alpine
MAINTAINER Jens Fischer <jf.seine.mail+mgserver@gmail.com>

RUN echo "@testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk update

RUN apk add --no-cache \
    bash \
    # Mapnik
    curl \
    bzip2 \
    tar \
    # scons.py
    build-base \
    freetype-dev \
    icu-dev \
    harfbuzz-dev \
    boost-dev \
    # Mapnik optional
    tiff-dev \
    sqlite-dev \
    libwebp-dev \
    postgresql-dev \
    cairo-dev \
    gdal-dev@testing \
    proj4-dev@testing

ENV PYTHONUNBUFFERED 1

# Mapnik
ENV MAPNIK_VERSION 3.0.22

RUN curl -sL https://github.com/mapnik/mapnik/releases/download/v${MAPNIK_VERSION}/mapnik-v${MAPNIK_VERSION}.tar.bz2 | tar -xj -C /tmp/ \
 && cd /tmp/mapnik-v${MAPNIK_VERSION} \
 && python scons/scons.py configure INPUT_PLUGINS=all \
 && make JOBS=4 \
 && make install JOBS=4

# Python Bindings
ENV PYTHON_MAPNIK_COMMIT 588fc90624ce8b1952dfa3db3d8c7722f3420cbb

RUN mkdir -p /opt/python-mapnik \
 && curl -L https://github.com/mapnik/python-mapnik/archive/${PYTHON_MAPNIK_COMMIT}.tar.gz | tar xz -C /opt/python-mapnik --strip-components=1 \
 && cd /opt/python-mapnik \
 && export BOOST_PYTHON_LIB=boost_python36 \
 && export BOOST_THREAD_LIB=boost_thread-mt \
 && export BOOST_SYSTEM_LIB=boost_system \
 && python3 setup.py install \
 && rm -r /opt/python-mapnik/build

CMD ["bash"]
