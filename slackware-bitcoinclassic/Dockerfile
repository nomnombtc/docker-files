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

ENV CL_VERSION 1.2.5
ENV CL_SHA256 900218eb2cd40c96a357a601e17fc6eacc91970f369761462de5a695c20701d5

RUN wget https://github.com/bitcoinclassic/bitcoinclassic/archive/v$CL_VERSION/bitcoinclassic-$CL_VERSION.tar.gz && \
  echo "$CL_SHA256 bitcoinclassic-$CL_VERSION.tar.gz" | sha256sum -c || exit 1 && \
  tar xvf bitcoinclassic-$CL_VERSION.tar.gz && rm -f bitcoinclassic-$CL_VERSION.tar.gz && \
  cd bitcoinclassic-$CL_VERSION && \
  sh ./autogen.sh && ./configure --with-gui=no && \
  make && make install && rm -rf /bitcoinclassic-$CL_VERSION

ENV HOME /bitcoin

COPY ./bin /usr/local/bin

USER bitcoin

VOLUME ["/bitcoin"]

EXPOSE 8332 8333

CMD ["btc_oneshot"]

#docker run -v /home/user/.bitcoin:/bitcoin/.bitcoin --name=bitcoind -d -p 8333:8333 slackware-bitcoinclassic
