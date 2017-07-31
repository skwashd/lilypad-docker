# LilyPad Proxy server

# Build step
FROM golang:1.8-alpine AS build
MAINTAINER Dave Hall <skwashd@gmail.com>

RUN apk update && \
  apk upgrade && \
  apk add --no-cache bash git && \
  go get github.com/LilyPad/GoLilyPad/server/proxy && \
  go get github.com/LilyPad/GoLilyPad/server/proxy/connect && \
  go get github.com/LilyPad/GoLilyPad/server/proxy/main/config && \
  go get github.com/satori/go.uuid && \
  go get gopkg.in/yaml.v2 && \
  go get github.com/klauspost/compress/zlib && \
  cd $GOPATH/src/github.com/LilyPad/GoLilyPad/server/proxy/main && \
  CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main . && \
  mv main /tmp/lilypad-proxy && \
  chmod 0755 /tmp/lilypad-proxy

FROM scratch
MAINTAINER Dave Hall <skwashd@gmail.com>

ADD https://curl.haxx.se/ca/cacert.pem /etc/ssl/certs/ca-certificates.crt

COPY --from=build /tmp/lilypad-proxy /

EXPOSE 25565
WORKDIR /data
ENTRYPOINT  ["/lilypad-proxy"]
