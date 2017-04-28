FROM ubuntu:zesty
MAINTAINER nnb <nomnombtc@arcor.de>

RUN apt-get update && apt-get install -y \
  python3.5 libleveldb-dev librocksdb-dev libsnappy-dev libbz2-dev libzip-dev \
  python3-setuptools python3-aiohttp python3-pip python3-openssl git \
  python3-dev libxml2-dev libxslt-dev daemontools && \
  pip3 install plyvel pyrocksdb irc pylru

RUN groupadd -r -g 234 electrum && useradd -r -u 234 -g electrum electrum && \
  mkdir /var/lib/electrumx && chown electrum.electrum /var/lib/electrumx && \
  git clone https://github.com/kyuupichan/electrumx.git 

USER electrum
WORKDIR /electrumx/daemontools
ENTRYPOINT /electrumx/daemontools/run

# copy daemontools directory from https://github.com/kyuupichan/electrumx/tree/master/contrib/daemontools
# to, for example, ~/.electrumx/daemontools and link it into the container, same with the database directory.
#
# docker run -v /home/user/.electrumx/daemontools:/electrumx/daemontools \
# -v /home/user/.electrumx/db:/var/lib/electrumx --net=host -d --name=electrumx ubuntu-electrumx
