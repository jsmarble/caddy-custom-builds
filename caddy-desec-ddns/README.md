# Caddy Docker build with deSEC Dynamic DNS module

[![GitHub](https://img.shields.io/badge/GitHub%20-%20jsmarble%2Fcaddy--desec--ddns%20-%20%23333?style=flat&logo=github)](https://ghcr.io/jsmarble/caddy-desec-ddns)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/jsmarble/caddy-custom-builds?label=Release)](https://github.com/jsmarble/caddy-custom-builds/releases)
[![GitHub build status](https://img.shields.io/github/actions/workflow/status/jsmarble/caddy-custom-builds/build.caddy-desec-ddns.yml?label=Build)](https://github.com/jsmarble/caddy-custom-builds/actions/workflows/build.caddy-desec-ddns.yml)

GitHub Actions publishes this image from the upstream Caddy version selected in
this directory's Dockerfile, using the following modules:

- [**deSEC DNS**](https://github.com/caddy-dns/desec): DNS-01 ACME validation support for deSEC DNS.
- [**Dynamic DNS**](https://github.com/mholt/caddy-dynamicdns): Updates DNS records with the public IP address of your instance.

## Build convention and current deployment

Keep the existing official `caddy:<version>-builder` and matching official Caddy
runtime stages. Update both upstream version lines together; retain the existing
xcaddy module recipe. The workflow runs on Dockerfile changes, manual dispatch
and its monthly schedule; a scheduled build does not itself bump those versions.

The November 4, 2026 version bump publishes **Caddy 2.11.8 / Go 1.27.3** using
that convention. Both Wintergarden's DigitalOcean edge and VyOS use a reviewed
digest of this published image. Their deployment pins and certificate ownership
are recorded separately in `oracle-vps-proxy/digitalocean/main.tf`,
`vyos/caddy-image.ref` and the network runbook. The home update was image-only;
DO's additional container hardening is a separate deployment configuration.

The earlier upstream contribution [serfriz/caddy-custom-builds#92](https://github.com/serfriz/caddy-custom-builds/pull/92)
was closed unmerged. This repository remains the source of the deployed variant.

## Usage

```sh
docker pull ghcr.io/jsmarble/caddy-desec-ddns:latest
```

### Tags

- `latest`
- `<version>` (e.g. `2.11.8`, including `2.11`, `2`, etc.)

Refer to the repository [README](https://github.com/jsmarble/caddy-custom-builds#usage) for full usage and configuration instructions.

## License

Software under [GPL-3.0](https://github.com/jsmarble/caddy-custom-builds/blob/main/LICENSE).
