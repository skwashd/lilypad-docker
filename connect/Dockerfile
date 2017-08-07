# LilyPad Connect server

# Build step
FROM golang:1.8-alpine AS build
MAINTAINER Dave Hall <skwashd@gmail.com>

RUN apk update && \
  apk upgrade && \
  apk add --no-cache bash git && \
  go get github.com/LilyPad/GoLilyPad/server/connect && \
  go get github.com/LilyPad/GoLilyPad/server/connect/main/config && \
  go get github.com/satori/go.uuid && \
  go get gopkg.in/yaml.v2 && \
  go get github.com/klauspost/compress/zlib && \
  cd $GOPATH/src/github.com/LilyPad/GoLilyPad/server/connect/main && \
  CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main . && \
  mv main /tmp/lilypad-connect && \
  chmod 0755 /tmp/lilypad-connect

FROM scratch
MAINTAINER Dave Hall <skwashd@gmail.com>

COPY --from=build /tmp/lilypad-connect /
COPY ./passwd /etc/passwd

USER nobody

EXPOSE 5091
WORKDIR /data
ENTRYPOINT  ["/lilypad-connect"]
