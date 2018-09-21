FROM tibux/slackware-sbopkg
MAINTAINER  nnb <nomnombtc@arcor.de>

#sbopkg
ENV TERM xterm

RUN echo "http://ftp.gwdg.de/pub/linux/slackware/slackware64-14.2/" > /etc/slackpkg/mirrors && \
  slackpkg -batch=on -default_answer=y update && \
  slackpkg -batch=on -default_answer=y install libunistring && \
  slackpkg -batch=on -default_answer=y upgrade-all
RUN slackpkg -batch=on -default_answer=y update && \
  slackpkg -batch=on -default_answer=y install shadow boost db48 \
  cxxlibs libtool python pkg-config openssl-solibs libffi gc guile \
  libidn libevent flex

RUN sbopkg -e stop -Bi six && \
  sbopkg -e stop -Bi python-dateutil && \
  sbopkg -e stop -Bi python-gflags && \
  sbopkg -e stop -Bi pytz && \
  sbopkg -e stop -Bi zeromq

ENV CORE_VER 0.16.3
#laanwj key
ENV GPG_KEY 90C8019E36C2E964
ENV PKG_SHA256 836eed97dfc79cff09f356e8fbd6a6ef2de840fb9ff20ebffb51ccffdb100218

RUN gpg --recv-key $GPG_KEY && \
  wget https://bitcoincore.org/bin/bitcoin-core-${CORE_VER}/bitcoin-${CORE_VER}.tar.gz && \
  wget https://bitcoincore.org/bin/bitcoin-core-${CORE_VER}/SHA256SUMS.asc && \
  gpg --verify SHA256SUMS.asc && grep $PKG_SHA256 SHA256SUMS.asc && \
  echo "$PKG_SHA256 bitcoin-${CORE_VER}.tar.gz" | sha256sum -c || exit 1 && \
  tar xvf bitcoin-${CORE_VER}.tar.gz && \
  cd bitcoin-${CORE_VER}/ || exit 1 && \
  ./configure --with-gui=no && \
  make -j7 && make install && \
  rm -rf /bitcoin* && rm -f /SHA256SUMS.asc && rm -rf /tmp/SBo
  
ENV HOME /bitcoin
RUN useradd -s /bin/bash -m -d /bitcoin bitcoin
RUN chown bitcoin:bitcoin -R /bitcoin

ADD ./bin /usr/local/bin
RUN chmod a+x /usr/local/bin/* 

USER bitcoin

VOLUME ["/bitcoin"]

EXPOSE 8332 8333

CMD ["btc_oneshot"]
