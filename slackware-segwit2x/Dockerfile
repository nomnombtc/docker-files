FROM tibux/slackware-sbopkg
MAINTAINER  nnb <nomnombtc@arcor.de>

#sbopkg
ENV TERM xterm

RUN echo "http://gd.tuwien.ac.at/opsys/linux/slackware/slackware64-14.2/" > /etc/slackpkg/mirrors && \
  slackpkg -batch=on -default_answer=y update && slackpkg -batch=on -default_answer=y upgrade-all
RUN slackpkg -batch=on -default_answer=y update && slackpkg -batch=on -default_answer=y install \
  shadow boost db48 cxxlibs libtool python pkg-config openssl-solibs libffi libunistring gc guile \
  libidn libevent flex

RUN sbopkg -e stop -Bi six 
RUN sbopkg -e stop -Bi python-dateutil
RUN sbopkg -e stop -Bi python-gflags
RUN sbopkg -e stop -Bi pytz
RUN sbopkg -e stop -Bi protobuf

ENV GIT_BRANCH segwit2x
ENV GIT_GPG_KEY 651EA65291BFDB26
ENV GIT_VERIFY_COMMIT 38b970f722d1bcc473a08d29503e303d921d2e18

RUN gpg --recv-key $GIT_GPG_KEY && \
  git clone https://github.com/btc1/bitcoin.git -b $GIT_BRANCH && \
  cd bitcoin/ && git verify-commit $GIT_VERIFY_COMMIT || exit 1 && \
  sh ./autogen.sh && ./configure --with-gui=no && \
  make -j7 && make install && rm -rf /bitcoin/*

ENV HOME /bitcoin
RUN useradd -s /bin/bash -m -d /bitcoin bitcoin
RUN chown bitcoin:bitcoin -R /bitcoin

ADD ./bin /usr/local/bin
RUN chmod a+x /usr/local/bin/* 

USER bitcoin

VOLUME ["/bitcoin"]

EXPOSE 18554 18555

CMD ["btc_oneshot"]

#docker run -v /home/user/.sw2xtest:/bitcoin --name=sw2xtest -d --net=host slackware-segwit2x
