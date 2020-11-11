FROM ubuntu:18.04

LABEL MAINTAINERS="Yusuke Wada <wada.yusuke@classmethod.jp>"

ARG asciidoctor_version=2.0.10
ARG asciidoctor_confluence_version=0.0.2
ARG asciidoctor_pdf_version=1.5.3
ARG asciidoctor_diagram_version=2.0.1
ARG asciidoctor_epub3_version=1.5.0.alpha.18
ARG asciidoctor_mathematical_version=0.3.1
ARG asciidoctor_revealjs_version=4.0.1

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
RUN apt-get update && apt-get install -y \
  ruby \
  ruby-dev \
  cmake \
  wget \
  curl \
  bison flex libffi-dev libxml2-dev libgdk-pixbuf2.0-dev libcairo2-dev libpango1.0-dev fonts-lyx


# Installing Ruby Gems needed in the image
# including asciidoctor itself
RUN gem install --no-document \
  rake \
  "asciidoctor:${ASCIIDOCTOR_VERSION}" \
  "asciidoctor-confluence:${ASCIIDOCTOR_CONFLUENCE_VERSION}" \
  "asciidoctor-diagram:${ASCIIDOCTOR_DIAGRAM_VERSION}" \
  "asciidoctor-epub3:${ASCIIDOCTOR_EPUB3_VERSION}" \
  "asciidoctor-mathematical:${ASCIIDOCTOR_MATHEMATICAL_VERSION}" \
  asciimath \
  "asciidoctor-pdf:${ASCIIDOCTOR_PDF_VERSION}" \
  "asciidoctor-revealjs:${ASCIIDOCTOR_REVEALJS_VERSION}" \
  coderay \
  epubcheck \
  haml \
  # kindlegen \ EOL 
  rouge \
  slim \
  thread_safe \
  tilt

# Installing Ruby Gems needed in the image
# including asciidoctor japanese package
RUN gem install asciidoctor-pdf-cjk-kai_gen_gothic --no-ri --no-rdoc && \
  gem install --version ${COMPASS_VERSION} compass --no-ri --no-rdoc && \
  gem install --version ${ZURB_FOUNDATION_VERSION} zurb-foundation --no-ri --no-rdoc && \
  asciidoctor-pdf-cjk-kai_gen_gothic-install && \
  ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  apt-get install -y nodejs npm ca-certificates openssl && \
  rm -rf /tmp/* /var/tmp/* && \
  wget -O VLGothic.zip "http://osdn.jp/frs/redir.php?m=jaist&f=%2Fvlgothic%2F62375%2FVLGothic-20141206.zip" && \
  unzip VLGothic.zip && \
  mkdir -p /root/.fonts && \
  cp VLGothic/VL-Gothic-Regular.ttf /root/.fonts && \
  rm -rf /root/VLGothic* && \
  # wget -qO- "https://github.com/dustinblackman/phantomized/releases/download/2.1.1/dockerized-phantomjs.tar.gz" | tar xz -C / && \
  npm install -g mermaid@${MERMAID_VERSION} && \
  wget https://github.com/asciidoctor/asciidoctor-stylesheet-factory/archive/master.zip && \
  unzip master.zip && \
  cd asciidoctor-stylesheet-factory-master && \
  compass compile && \
  cp -pr stylesheets / && \
  cd .. && \
  rm -rf master.zip asciidoctor-stylesheet-factory-master

# Installing AWS CLI v2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
  unzip awscliv2.zip && \
  ./aws/install

# Installing ssh for AWS Amplify building
RUN apt-get update && apt-get install -y git-all openssh-client default-jre rsync

# Install and switch to nodejs 12.x
RUN npm install -g n && n 12 && npm install -g yarn && node --version

# Install utility
RUN apt-get install -y software-properties-common && add-apt-repository ppa:lazygit-team/release && apt-get update && apt-get install -y lazygit


WORKDIR /documents
VOLUME /documents

CMD ["/bin/bash"]
