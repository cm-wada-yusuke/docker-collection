FROM alpine:3.10

LABEL MAINTAINERS="Guillaume Scheibel <guillaume.scheibel@gmail.com>, Damien DUPORTAL <damien.duportal@gmail.com>"

ARG asciidoctor_version=2.0.10
ARG asciidoctor_confluence_version=0.0.2
ARG asciidoctor_pdf_version=1.5.0.beta.7
ARG asciidoctor_diagram_version=1.5.19
ARG asciidoctor_epub3_version=1.5.0.alpha.9
ARG asciidoctor_mathematical_version=0.3.1
ARG asciidoctor_revealjs_version=2.0.0

ENV ASCIIDOCTOR_VERSION=${asciidoctor_version} \
  ASCIIDOCTOR_CONFLUENCE_VERSION=${asciidoctor_confluence_version} \
  ASCIIDOCTOR_PDF_VERSION=${asciidoctor_pdf_version} \
  ASCIIDOCTOR_DIAGRAM_VERSION=${asciidoctor_diagram_version} \
  ASCIIDOCTOR_EPUB3_VERSION=${asciidoctor_epub3_version} \
  ASCIIDOCTOR_MATHEMATICAL_VERSION=${asciidoctor_mathematical_version} \
  ASCIIDOCTOR_REVEALJS_VERSION=${asciidoctor_revealjs_version}

ENV COMPASS_VERSION 0.12.7
ENV ZURB_FOUNDATION_VERSION 4.3.2
ENV MERMAID_VERSION 7.0.9

WORKDIR /root

# Installing package required for the runtime of
# any of the asciidoctor-* functionnalities
RUN apk add --no-cache \
  bash \
  curl \
  ca-certificates \
  findutils \
  font-bakoma-ttf \
  graphviz \
  inotify-tools \
  make \
  openjdk8-jre \
  python3 \
  py3-pillow \
  py3-setuptools \
  ruby \
  ruby-mathematical \
  ruby-rake \
  ttf-liberation \
  ttf-dejavu \
  tzdata \
  unzip \
  which

# Installing Ruby Gems needed in the image
# including asciidoctor itself
RUN apk add --no-cache --virtual .rubymakedepends \
  build-base \
  libxml2-dev \
  ruby-dev \
  && gem install --no-document \
  "asciidoctor:${ASCIIDOCTOR_VERSION}" \
  "asciidoctor-confluence:${ASCIIDOCTOR_CONFLUENCE_VERSION}" \
  "asciidoctor-diagram:${ASCIIDOCTOR_DIAGRAM_VERSION}" \
  "asciidoctor-epub3:${ASCIIDOCTOR_EPUB3_VERSION}" \
  "asciidoctor-mathematical:${ASCIIDOCTOR_MATHEMATICAL_VERSION}" \
  asciimath \
  "asciidoctor-pdf:${ASCIIDOCTOR_PDF_VERSION}" \
  "asciidoctor-revealjs:${ASCIIDOCTOR_REVEALJS_VERSION}" \
  coderay \
  epubcheck:3.0.1 \
  haml \
  kindlegen:3.0.3 \
  rouge \
  slim \
  thread_safe \
  tilt \
  && apk del -r --no-cache .rubymakedepends

# Installing Python dependencies for additional
# functionnalities as diagrams or syntax highligthing
RUN apk add --no-cache --virtual .pythonmakedepends \
  build-base \
  python3-dev \
  py3-pip \
  && pip3 install --no-cache-dir \
  actdiag \
  'blockdiag[pdf]' \
  nwdiag \
  seqdiag \
  && apk del -r --no-cache .pythonmakedepends

RUN gem install asciidoctor-pdf-cjk-kai_gen_gothic --no-ri --no-rdoc && \
  gem install --version ${COMPASS_VERSION} compass --no-ri --no-rdoc && \
  gem install --version ${ZURB_FOUNDATION_VERSION} zurb-foundation --no-ri --no-rdoc && \
  asciidoctor-pdf-cjk-kai_gen_gothic-install && \
  ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  apk add --no-cache nodejs nodejs-npm ca-certificates openssl && \
  rm -rf /tmp/* /var/tmp/* && \
  wget -O VLGothic.zip "http://osdn.jp/frs/redir.php?m=jaist&f=%2Fvlgothic%2F62375%2FVLGothic-20141206.zip" && \
  unzip VLGothic.zip && \
  mkdir -p /root/.fonts && \
  cp VLGothic/VL-Gothic-Regular.ttf /root/.fonts && \
  rm -rf /root/VLGothic* && \
  # wget -qO- "https://github.com/dustinblackman/phantomized/releases/download/2.1.1/dockerized-phantomjs.tar.gz" | tar xz -C / && \
  # npm install -g phantomjs && \
  npm install -g mermaid@${MERMAID_VERSION} && \
  wget https://github.com/asciidoctor/asciidoctor-stylesheet-factory/archive/master.zip && \
  unzip master.zip && \
  cd asciidoctor-stylesheet-factory-master && \
  compass compile && \
  cp -pr stylesheets / && \
  cd .. && \
  rm -rf master.zip asciidoctor-stylesheet-factory-master

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
VOLUME /documents

CMD ["/bin/bash"]
