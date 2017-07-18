FROM tibux/slackware-sbopkg
MAINTAINER  nnb <nomnombtc@arcor.de>

#needed by sbopkg
ENV TERM xterm

RUN echo "http://gd.tuwien.ac.at/opsys/linux/slackware/slackware64-14.2/" > /etc/slackpkg/mirrors && \
  slackpkg -batch=on -default_answer=y update && slackpkg -batch=on -default_answer=y upgrade-all && \
  slackpkg -batch=on -default_answer=y update && slackpkg -batch=on -default_answer=y install \
  shadow boost db48 cxxlibs libtool python pkg-config openssl-solibs libffi libunistring gc guile \
  libidn libevent flex

RUN sbopkg -e stop -Bi six && \
  sbopkg -e stop -Bi python-dateutil && \
  sbopkg -e stop -Bi python-gflags && \
  sbopkg -e stop -Bi pytz && \
  sbopkg -e stop -Bi protobuf && \
  useradd -s /bin/bash -m -d /bitcoin bitcoin

ENV BU_VERSION 1.0.3.0
ENV BU_SHA256 27f4d0b6c2148aefd981479f15d950f58709a42639a9ebf8cefee80b6a82ec0a

RUN wget https://github.com/BitcoinUnlimited/BitcoinUnlimited/archive/v$BU_VERSION/BitcoinUnlimited-$BU_VERSION.tar.gz && \
  echo "$BU_SHA256 BitcoinUnlimited-$BU_VERSION.tar.gz" | sha256sum -c || exit 1 && \
  tar xvf BitcoinUnlimited-*.tar.gz && rm -f BitcoinUnlimited-*.tar.gz && \
  cd BitcoinUnlimited-$BU_VERSION && \
  sh ./autogen.sh && ./configure --with-gui=no && \
  make && make install && rm -rf /BitcoinUnlimited-*

ENV HOME /bitcoin

COPY ./bin /usr/local/bin

USER bitcoin

VOLUME ["/bitcoin"]

EXPOSE 8332 8333

CMD ["btc_oneshot"]

#docker run -v /home/user/.bitcoin:/bitcoin/.bitcoin --name=bitcoind -d -p 8333:8333 slackware-bitcoinunlimited
