FROM ubuntu:disco
MAINTAINER nnb <nomnombtc@arcor.de>

RUN apt-get update && apt-get install -y \
  python3.7 libleveldb-dev librocksdb-dev libsnappy-dev libbz2-dev liblz4-dev \
  libzip-dev python3-setuptools python3-pip python3-openssl git python3.7-dev \
  libxml2-dev libxslt-dev daemontools

RUN python3.7 -m pip install plyvel 'python-rocksdb>=0.6.9' pylru \
  'aiorpcX>=0.17.0,<0.18' attrs 'aiohttp>=3.3' 'uvloop>=0.12.2'

RUN groupadd -r -g 234 electrum && useradd -r -u 234 -g electrum electrum && \
  mkdir /var/lib/electrumx && chown electrum.electrum /var/lib/electrumx && \
  git clone https://github.com/kyuupichan/electrumx.git && \
  update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1

USER electrum
WORKDIR /electrumx/daemontools
ENTRYPOINT /electrumx/daemontools/run

# copy daemontools directory from https://github.com/kyuupichan/electrumx/tree/master/contrib/daemontools
# to, for example, ~/.electrumx/daemontools and link it into the container, same with the database directory.
#
# docker run -v /home/user/.electrumx/daemontools:/electrumx/daemontools \
# -v /home/user/.electrumx/db:/var/lib/electrumx --net=host -d --name=electrumx ubuntu-electrumx
