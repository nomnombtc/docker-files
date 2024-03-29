FROM ubuntu:focal
MAINTAINER nnb <nomnombtc@arcor.de>

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
  python3 libleveldb-dev librocksdb-dev libsnappy-dev libbz2-dev liblz4-dev \
  libzip-dev python3-setuptools python3-pip python3-openssl git python3-dev \
  libxml2-dev libxslt-dev daemontools cython cython3

RUN python3 -m pip install plyvel 'python-rocksdb>=0.6.9' pylru \
  'aiorpcX[ws]>=0.22.0,<0.23' attrs 'aiohttp>=3.3,<4' 'uvloop>=0.14'

RUN groupadd -r -g 234 electrum && useradd -r -u 234 -g electrum electrum && \
  mkdir /var/lib/electrumx && chown electrum.electrum /var/lib/electrumx && \
  git clone https://github.com/spesmilo/electrumx.git

USER electrum
WORKDIR /electrumx/daemontools
ENTRYPOINT /electrumx/daemontools/run

# copy daemontools directory from https://github.com/spesmilo/electrumx/tree/master/contrib/daemontools
# to, for example, ~/.electrumx/daemontools and link it into the container, same with the database directory.
#
# docker run -v /home/user/.electrumx/daemontools:/electrumx/daemontools \
# -v /home/user/.electrumx/db:/var/lib/electrumx --net=host -d --name=electrumx ubuntu-electrumx
