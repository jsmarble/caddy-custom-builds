# syntax=docker/dockerfile:1
ARG CADDY_VERSION=2.11.9
FROM --platform=$BUILDPLATFORM golang:1.27.3-alpine3.24 AS builder
ARG CADDY_VERSION

RUN apk add --no-cache ca-certificates git
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@v0.4.7
WORKDIR /usr/bin
ARG TARGETOS
ARG TARGETARCH
RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} CGO_ENABLED=0 xcaddy build "v${CADDY_VERSION}" \
    --with github.com/caddy-dns/desec@v1.1.0 \
    --with github.com/mholt/caddy-dynamicdns@v0.0.0-20260805195708-67d107a42c02 \
    --replace golang.org/x/net=golang.org/x/net@v0.62.0 \
    --replace golang.org/x/crypto=golang.org/x/crypto@v0.58.1-0.20261009181718-2f1f834330b2

# Reject known reachable Go vulnerabilities before publishing either architecture.
RUN go run golang.org/x/vuln/cmd/govulncheck@v1.9.0 -mode=binary /usr/bin/caddy

FROM alpine:3.24
ARG CADDY_VERSION
RUN apk add --no-cache ca-certificates tzdata && apk upgrade --no-cache \
    && mkdir -p /config/caddy /data/caddy /etc/caddy /usr/share/caddy /srv
ENV CADDY_VERSION=v${CADDY_VERSION} XDG_CONFIG_HOME=/config XDG_DATA_HOME=/data
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
EXPOSE 80 443
WORKDIR /srv
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
