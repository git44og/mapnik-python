FROM python:3.7-alpine
MAINTAINER Jens Fischer <jf.seine.mail+mgserver@gmail.com>

ENV PYTHONUNBUFFERED 1

RUN apk add --no-cache \
    curl \
    bash \
    boost-dev \
    build-base \
    bzip2 \
    freetype-dev \
    icu-dev \
    harfbuzz-dev \
    tar \
    # scons
    jpeg-dev \
    tiff-dev \
    cairo \
    postgresql-dev \
    libwebp-dev \
    boost-regex \
 && apk add \
    --no-cache \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
    gdal-dev \
    proj4-dev
#    alpine-sdk \ / alternative to build-base
#    missing from apt-get
#    postgresql-libs \
#    software-properties-common
#    libboost-dev \
#    libboost-filesystem-dev \
#    libboost-program-options-dev \
#    libboost-python-dev \
#    libboost-regex-dev \
#    libboost-system-dev \
#    libboost-thread-dev \
#    libicu-dev \
#    libtiff5-dev \
#    libfreetype6-dev \
#    libpng12-dev \
#    libxml2-dev \
#    libproj-dev \
#    libsqlite3-dev \
#    libgdal-dev \
#    libcairo-dev \
#    python-cairo-dev \
#    postgresql-contrib \
#    libharfbuzz-dev \
#    nodejs \
#    python3-dev \
#    python-dev \
#    git \
#    python-pip \
#    python-setuptools \
#    python-wheel \
#    python3-setuptools \
#    python3-pip \
#    python3-wheel \

#    more from scc
#    rsync \
#    gettext \
#    curl \
# && apk add --no-cache --virtual \
#    .build-deps \
#    ca-certificates \
#    gcc \
#    python3-dev \
#    postgresql-dev \
#    linux-headers \
#    musl-dev \
#    libffi-dev \
#    jpeg-dev \
#    zlib-dev \
#    freetype-dev \
#    libxslt-dev

RUN mkdir /app

WORKDIR /app

# Mapnik
ENV MAPNIK_VERSION 3.0.22

RUN curl -sL https://github.com/mapnik/mapnik/releases/download/v${MAPNIK_VERSION}/mapnik-v${MAPNIK_VERSION}.tar.bz2 | tar -xj -C /tmp/

# yum install freetype-devel
# yum install libtool-ltdl-devel
# yum install libpng-devel
# yum install libtiff-devel
# yum install libjpeg-devel
# yum install gcc-c++
# yum install libicu-devel
# yum install python-devel
# yum install bzip2-devel

RUN cd /tmp/mapnik-v${MAPNIK_VERSION} && python scons/scons.py configure INPUT_PLUGINS=all
RUN cd /tmp/mapnik-v${MAPNIK_VERSION} && make JOBS=4 && make install JOBS=4





#    geos \
#    giflib \
#    libcrypto1.1 \
#    libcurl \
#    libgcc \
#    libjpeg-turbo \
#    libpng \
#    libpq \
#    libstdc++ \
#    musl \
#    sqlite-libs \
#    tiff \
#    zlib \
#    gdal

# Node Bindings
#ENV NODE_MAPNIK_VERSION 3.5.13
#RUN mkdir -p /opt/node-mapnik && curl -L https://github.com/mapnik/node-mapnik/archive/v${NODE_MAPNIK_VERSION}.tar.gz | tar xz -C /opt/node-mapnik --strip-components=1
#RUN cd /opt/node-mapnik && npm install --unsafe-perm=true --build-from-source && npm link

# Python Bindings
ENV PYTHON_MAPNIK_COMMIT 588fc90624ce8b1952dfa3db3d8c7722f3420cbb
#ENV PYTHON_MAPNIK_COMMIT 3a60211dee366060acf4e5e0de8b621b5924f2e6
RUN apk add --no-cache \
    py-setuptools \
    boost-python \
 && export BOOST_PYTHON_LIB=boost_python \
 && export BOOST_THREAD_LIB=boost_thread \
 && export BOOST_SYSTEM_LIB=boost_system
RUN mkdir -p /opt/python-mapnik && curl -L https://github.com/mapnik/python-mapnik/archive/${PYTHON_MAPNIK_COMMIT}.tar.gz | tar xz -C /opt/python-mapnik --strip-components=1


RUN cd /opt/python-mapnik && python2 setup.py install && python3 setup.py install && rm -r /opt/python-mapnik/build

# use jdocker77/mapnik:3.0.10 to here

# OSM 2 PGSQL to import pbf data into postgis
#RUN apt-get update && \
#    apt-get install -y osm2pgsql git wget && \
#    npm install -g carto

#RUN ls
#RUN mkdir -p /opt/services/flaskapp/src
# We copy the requirements.txt file first to avoid cache invalidations
#COPY requirements.txt /opt/services/flaskapp/src/
#WORKDIR /opt/services/flaskapp/src
#RUN pip install -r requirements.txt

#EXPOSE 5090
#CMD ["python", "app.py"]

CMD ["bash"]
