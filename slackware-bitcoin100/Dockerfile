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

ENV GIT_BRANCH 0.12
ENV GIT_GPG_KEY 74810B012346C9A6
ENV GIT_VERIFY_COMMIT c1b7421781b7a53485c6db4a6005a80e32267c9f

RUN gpg --recv-key $GIT_GPG_KEY && \
  git clone https://github.com/bitcoin/bitcoin.git -b $GIT_BRANCH && \
  cd bitcoin/ && git verify-commit $GIT_VERIFY_COMMIT || exit 1 && \
  git remote add xtbtc https://github.com/bitcoinxt/bitcoin.git && \
  git fetch xtbtc && git pull xtbtc pull/1/head && \
  sh ./autogen.sh && ./configure --with-gui=no && \
  make && make install && rm -rf /bitcoin/*

ENV HOME /bitcoin

COPY ./bin /usr/local/bin

USER bitcoin

VOLUME ["/bitcoin"]

EXPOSE 8332 8333

CMD ["btc_oneshot"]

#docker run -v /home/user/.bitcoin:/bitcoin/.bitcoin --name=bitcoind -d -p 8333:8333 slackware-bitcoin100
