FROM debian:jessie

MAINTAINER you_oki1@yahoo.co.jp

env GOPATH /opt/go

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y curl git unzip && \
  apt-get clean && \
  curl -sL http://golang.org/dl/go1.6.linux-amd64.tar.gz | tar zxf - -C /usr/local && \
  ln -s ../go/bin/go /usr/local/bin/go && \
  go get github.com/mofumofuchan/mlucon-proto/benchmarker && \
  go install github.com/mofumofuchan/mlucon-proto/benchmarker && \
  ( \
    cd /opt/go/src/github.com/mofumofuchan/mlucon-proto/benchmarker/userdata/ && \
    curl -sLO https://github.com/mofumofuchan/mlucon-proto/releases/download/img/img.zip && \
    unzip img.zip && \
    rm img.zip \
  )

ENTRYPOINT ["/opt/go/bin/benchmarker", "-u", "/opt/go/src/github.com/mofumofuchan/mlucon-proto/benchmarker/userdata"]
#CMD ["-t", "http://127.0.0.1/"]
