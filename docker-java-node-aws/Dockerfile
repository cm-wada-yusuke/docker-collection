FROM amazoncorretto:8
LABEL MAINTAINERS="Yusuke Wada <wada.yusuke@classmethod.jp>"

WORKDIR /root

# versions
ARG gradle_version=6.1

# gradle
RUN yum install -y which zip unzip && \
  curl -s "https://get.sdkman.io" | bash
# SDK Man replaced GVM. Using for Groovy, Gradle, and Maven Version Management
RUN export SDKMAN_DIR="$HOME/.sdkman" && \
  [[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh" && \
  sdk install gradle 6.1

# nodejs
RUN curl --silent --location https://rpm.nodesource.com/setup_12.x | bash -
RUN yum install -y gcc-c++ nodejs

WORKDIR /app
VOLUME /app

CMD ["/bin/bash"]
