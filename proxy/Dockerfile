# LilyPad Proxy server

# Build step
FROM golang:1.8-alpine AS build
MAINTAINER Dave Hall <skwashd@gmail.com>

RUN apk update && \
  apk upgrade && \
  apk add --no-cache bash curl git && \
  go get github.com/LilyPad/GoLilyPad/server/proxy && \
  go get github.com/LilyPad/GoLilyPad/server/proxy/connect && \
  go get github.com/LilyPad/GoLilyPad/server/proxy/main/config && \
  go get github.com/satori/go.uuid && \
  go get gopkg.in/yaml.v2 && \
  go get github.com/klauspost/compress/zlib && \
  cd $GOPATH/src/github.com/LilyPad/GoLilyPad/server/proxy/main && \
  CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main . && \
  mv main /tmp/lilypad-proxy && \
  chmod 0755 /tmp/lilypad-proxy && \
  curl -SL https://curl.haxx.se/ca/cacert.pem -o /tmp/ca-certificates.crt && \
  echo 'e78c8ab7b4432bd466e64bb942d988f6c0ac91cd785017e465bdc96d42fe9dd0  /tmp/ca-certificates.crt' | sha256sum -c - && \
  chmod 444 /tmp/ca-certificates.crt

FROM scratch
MAINTAINER Dave Hall <skwashd@gmail.com>

COPY --from=build /tmp/lilypad-proxy /
COPY --from=build /tmp/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY ./passwd /etc/passwd

USER nobody

EXPOSE 25565
WORKDIR /data
ENTRYPOINT  ["/lilypad-proxy"]
