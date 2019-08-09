FROM htakeuchi/docker-asciidoctor-jp
LABEL MAINTAINER Yusuke WADA

WORKDIR /root

RUN apk -v --update add \
  git \
  python3 \
  groff \
  less \
  mailcap \
  rsync \
  && \
  pip3 install --upgrade pip setuptools && \
  pip3 install --upgrade awscli s3cmd python-magic && \
  apk -v --purge del py-pip && \
  rm /var/cache/apk/*

WORKDIR /documents
