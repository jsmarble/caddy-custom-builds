# Caddy Docker build with deSEC Dynamic DNS module

[![GitHub](https://img.shields.io/badge/GitHub%20-%20jsmarble%2Fcaddy--desec--ddns%20-%20%23333?style=flat&logo=github)](https://ghcr.io/jsmarble/caddy-desec-ddns)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/jsmarble/caddy-custom-builds?label=Release)](https://github.com/jsmarble/caddy-custom-builds/releases)
[![GitHub build status](https://img.shields.io/github/actions/workflow/status/jsmarble/caddy-custom-builds/build.caddy-desec-ddns.yml?label=Build)](https://github.com/jsmarble/caddy-custom-builds/actions/workflows/build.caddy-desec-ddns.yml)

This image is updated automatically by GitHub Actions when a new version of [Caddy](https://github.com/caddyserver/caddy) is released, using the following modules:

- [**deSEC DNS**](https://github.com/caddy-dns/desec): DNS-01 ACME validation support for deSEC DNS.
- [**Dynamic DNS**](https://github.com/mholt/caddy-dynamicdns): Updates DNS records with the public IP address of your instance.

## Usage

```sh
docker pull ghcr.io/jsmarble/caddy-desec-ddns:latest
```

### Tags

- `latest`
- `<version>` (e.g. `2.7.4`, including `2.7`, `2`, etc.)

Refer to the repository [README](https://github.com/jsmarble/caddy-custom-builds#usage) for full usage and configuration instructions.

## License

Software under [GPL-3.0](https://github.com/jsmarble/caddy-custom-builds/blob/main/LICENSE).
