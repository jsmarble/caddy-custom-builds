# Caddy Docker Custom Builds

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/jsmarble/caddy-custom-builds?label=Release)](https://github.com/jsmarble/caddy-custom-builds/releases)
[![License](https://img.shields.io/github/license/jsmarble/caddy-custom-builds?label=License)](https://github.com/jsmarble/caddy-custom-builds/blob/main/LICENSE)

Custom [Caddy](https://github.com/caddyserver/caddy) Docker images with specific module combinations, built automatically when a new version of Caddy is released. Images are also rebuilt monthly on the first of each month to pick up any module updates.

Images are published to GitHub Container Registry (GHCR) for all Caddy-supported platforms.

## Builds

### Caddy Images

- [**caddy-bunny-ddns**](https://github.com/jsmarble/caddy-custom-builds/tree/main/caddy-bunny-ddns): Caddy with Bunny DNS and Dynamic DNS modules.
- [**caddy-desec-ddns**](https://github.com/jsmarble/caddy-custom-builds/tree/main/caddy-desec-ddns): Caddy with deSEC DNS and Dynamic DNS modules.

### Modules

- [**Bunny DNS**](https://github.com/caddy-dns/bunny): DNS-01 ACME validation support for Bunny DNS.
- [**deSEC DNS**](https://github.com/caddy-dns/desec): DNS-01 ACME validation support for deSEC DNS.
- [**Dynamic DNS**](https://github.com/mholt/caddy-dynamicdns): Updates DNS records with the public IP address of your instance.

## Usage

Images are built for all Caddy-supported platforms and available at:

```sh
docker pull ghcr.io/jsmarble/caddy-bunny-ddns:latest
docker pull ghcr.io/jsmarble/caddy-desec-ddns:latest
```

### Tags

- `latest`
- `<version>` (e.g. `2.7.4`, including `2.7`, `2`, etc.)

These tags are mutable — `latest` and version tags may be rebuilt to include module updates without a Caddy version bump. To pin to an exact build, use a digest:

```sh
docker pull ghcr.io/jsmarble/caddy-bunny-ddns:2.7.4
docker image inspect --format='{{index .RepoDigests 0}}' ghcr.io/jsmarble/caddy-bunny-ddns:2.7.4
# Pin by digest in your deployment:
# ghcr.io/jsmarble/caddy-bunny-ddns@sha256:<digest>
```

### Container Creation

#### Docker Run

```sh
docker run --rm -it \
  --name caddy \
  --restart unless-stopped \
  -p 80:80 \
  -p 443:443 \
  -p 443:443/udp \
  -v caddy-data:/data \
  -v caddy-config:/config \
  -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
  -e BUNNY_API_KEY=<token-value> \    # for caddy-bunny-ddns
  # -e DESEC_TOKEN=<token-value> \   # for caddy-desec-ddns
  ghcr.io/jsmarble/caddy-bunny-ddns:latest
```

#### Docker Compose

```yaml
services:
  caddy:
    image: ghcr.io/jsmarble/caddy-bunny-ddns:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - caddy-data:/data
      - caddy-config:/config
      - $PWD/Caddyfile:/etc/caddy/Caddyfile
    environment:
      - BUNNY_API_KEY=<token-value>     # for caddy-bunny-ddns
      # - DESEC_TOKEN=<token-value>    # for caddy-desec-ddns

volumes:
  caddy-data:
    external: true
  caddy-config:
```

### Graceful Reloads

Caddy supports zero-downtime config reloads without a full container restart:

```sh
caddy_container_id=$(docker ps | grep caddy | awk '{print $1;}')
docker exec -w /etc/caddy $caddy_container_id caddy reload
```

## Configuration

### DNS Modules

Set the global [`acme_dns`](https://caddyserver.com/docs/caddyfile/options#acme-dns) directive in your `Caddyfile`:

```Caddyfile
{
  acme_dns bunny {env.BUNNY_API_KEY}    # for caddy-bunny-ddns
  # acme_dns desec {env.DESEC_TOKEN}   # for caddy-desec-ddns
}
```

Or per-site with the [`tls`](https://caddyserver.com/docs/caddyfile/directives/tls) directive:

```Caddyfile
my.domain.tld {
  tls {
    dns bunny {env.BUNNY_API_KEY}
    # dns desec {env.DESEC_TOKEN}
  }
}
```

### Dynamic DNS

To keep DNS records updated with your instance's public IP, add the [`dynamic_dns`](https://caddyserver.com/docs/modules/dynamic_dns) directive to your `Caddyfile` global options:

```Caddyfile
{
  dynamic_dns {
    provider bunny {env.BUNNY_API_KEY}    # for caddy-bunny-ddns
    # provider desec {env.DESEC_TOKEN}   # for caddy-desec-ddns
    domains {
      domain.tld
    }
  }
}
```

For advanced configuration, refer to [mholt/caddy-dynamicdns](https://github.com/mholt/caddy-dynamicdns).

## License

Software under [GPL-3.0](https://github.com/jsmarble/caddy-custom-builds/blob/main/LICENSE).
