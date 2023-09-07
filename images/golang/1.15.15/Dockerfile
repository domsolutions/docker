FROM debian:bullseye
FROM buildpack-deps:bullseye-curl

RUN /bin/sh -c set -eux; apt-get update; apt-get install -y --no-install-recommends g++ gcc libc6-dev make pkg-config ; rm -rf /var/lib/apt/lists/*

ENV PATH=/usr/local/go/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

ENV GOLANG_VERSION=1.15.15

RUN set -eux; \
	url='https://dl.google.com/go/go1.15.15.linux-amd64.tar.gz'; \
	sha256='0885cf046a9f099e260d98d9ec5d19ea9328f34c8dc4956e1d3cd87daaddb345'; \
	wget -O go.tgz.asc "$url.asc"; \
	wget -O go.tgz "$url" --progress=dot:giga; \
	echo "$sha256 *go.tgz" | sha256sum -c -; \
    echo "passed sha256sum check" \
# https://github.com/golang/go/issues/14739#issuecomment-324767697
# https://www.google.com/linuxrepositories/
	gpg --batch --keyserver keyserver.ubuntu.com --recv-keys 'EB4C 1BFD 4F04 2F6D DDCC  EC91 7721 F63B D38B 4796'; \
# let's also fetch the specific subkey of that key explicitly that we expect "go.tgz.asc" to be signed by, just to make sure we definitely have it
	gpg --batch --keyserver keyserver.ubuntu.com --recv-keys '2F52 8D36 D67B 69ED F998  D857 78BD 6547 3CB3 BD13'; \
	gpg --batch --verify go.tgz.asc go.tgz; \
	gpgconf --kill all; \
	rm -rf $(mktemp -d) go.tgz.asc; \
	\
	tar -C /usr/local -xzf go.tgz; \
	rm go.tgz; \
	\
	go version


ENV GOPATH=/go

ENV PATH=/go/bin:/usr/local/go/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 1777 "$GOPATH"

WORKDIR /go
