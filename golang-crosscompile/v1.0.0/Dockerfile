FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y wget \
	git \
	gcc \
 	libgl1-mesa-dev \
	xorg-dev \
	curl \
	gosu \
	libltdl-dev \
	gcc-mingw-w64 \
	parallel \
	;

# Download & install go
RUN wget -P /tmp https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf /tmp/go1.12.6.linux-amd64.tar.gz
RUN rm /tmp/go1.12.6.linux-amd64.tar.gz

# Set gopath
ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH
RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

# Install goreleaser
RUN curl -L -o goreleaser.tgz \
      https://github.com/goreleaser/goreleaser/releases/download/v0.112.1/goreleaser_Linux_x86_64.tar.gz && \
      tar -zxvf goreleaser.tgz -C /usr/local/bin/ && \
	rm -f goreleaser.tgz

# Prepare for OSX - cf https://github.com/docker/golang-cross
COPY    bin/osx-cross.sh /tmp/
RUN     /tmp/osx-cross.sh
ENV PATH /osxcross/target/bin:$PATH

RUN go get -u github.com/go-bindata/go-bindata
RUN go install github.com/go-bindata/go-bindata/...

RUN mkdir -p /app

WORKDIR /app
