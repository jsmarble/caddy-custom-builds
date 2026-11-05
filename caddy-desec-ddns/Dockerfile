# syntax=docker/dockerfile:1
FROM caddy:2.11.9-builder AS builder

RUN xcaddy build v2.11.9 \
    --with github.com/caddy-dns/desec@v1.1.0 \
    --with github.com/mholt/caddy-dynamicdns@v0.0.0-20260805195708-67d107a42c02

# Reject known reachable Go vulnerabilities before publishing either architecture.
RUN go run golang.org/x/vuln/cmd/govulncheck@v1.9.0 -mode=binary /usr/bin/caddy

FROM caddy:2.11.9

RUN apk upgrade --no-cache

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
