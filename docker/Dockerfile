FROM golang:alpine AS builder
WORKDIR $GOPATH
ARG GOPROXY_VERSION=master
RUN apk update; apk upgrade; \
    apk add --no-cache ca-certificates git; \
    cd /go/src/; \
    mkdir -p github.com/sunrainchy; \
    cd github.com/sunrainchy; \
    git clone --depth=1 https://github.com/sunrainchy/goproxy.git; \
	cd goproxy; \
    git checkout ${GOPROXY_VERSION}; \
    CGO_ENABLED=0 GOOS=linux go build -ldflags "-s -w" -a -installsuffix cgo -o proxy; \
    chmod 0777 proxy

FROM debian:stable-slim
COPY --from=builder /go/src/github.com/sunrainchy/goproxy/proxy /usr/local/bin/
# RUN chmod 0777 /usr/local/bin/proxy
EXPOSE 80 443
CMD /usr/local/bin/proxy http -t tcp -p :80,:443
