<div align="center">

<img src="./watchtower.jpeg" height="400px"/>

# Watchtower

An automated configuration of my HTPC.

</div>

## 📖 Overview

Control center for my personal media (incl. ~2500 MP3s from the 90s and 00s).

Extends [Mothership](https://github.com/jovalle/mothership)

## 🧰 Core Components

- [Docker Socket Proxy](https://github.com/Tecnativa/docker-socket-proxy): Secured proxy for Homepage to watch Docker
- [Jellyfin](https://jellyfin.org): Plex competitor. Currently running comparisons. One advantage for Jellyfin is the lack of a SaaS and support for more media types (comics, audiobooks)
- [Node Exporter](https://github.com/prometheus/node_exporter): Presents host resource metrics to be consumed by Prometheus and displayed by Grafana
- [Plex](https://plex.tv): Organizes and streams media
- [Portainer](https://portainer.io): Web app for managing container stacks remotely
- [Traefik](https://traefik.io): Reverse proxy for serving other components with HTTPS enabled URLs. Using Let's Encrypt for quick and easy HTTPS certificates.
- [Watchtower](https://containrrr.dev/watchtower/): No relation. 😅 Keeps an eye on colocated containers and updates them while I'm (hopefully) sleeping.

## 📋 Prerequisites

### Environment File

`.env` stores common variables to be referenced by virtually all docker compose services via `env_file` parameter. See sample below.

```sh
# general
DOMAIN="example.net"
DOMAIN_EXT="example.com"
HOST_IP=192.168.1.2
PGID=1000
PUID=1000
TZ="America/New_York"

# apps
PLEX_API_KEY=REDACTED
PORTAINER_API_KEY=REDACTED

# cloudflare
CF_API_EMAIL=REDACTED
CF_API_KEY=REDACTED

# media
MISC_PATH=/mnt/hulkpool/misc
MOVIES_PATH=/mnt/hulkpool/movies
MUSIC_PATH=/mnt/hulkpool/music
TVSHOWS_PATH=/mnt/whirlpool/tvshows

# optional (if deploying containers remotely and without systemd)
DOCKER_HOST_IP=${HOST_IP}
DOCKER_HOST="ssh://root@${DOCKER_HOST_IP}"
```

⚠️ Virtually all docker compose services are leveraging `.env`. Changes to the file will trigger recreations of virtually all containers. May look into creating specific environment files for each container to address this. Wish I could just uses SOPS for inline encryption of `docker-compose.yaml`.

## 🚀 Deployment

Ideally, `git clone` this repo at `/etc/watchtower` on the target host.

To deploy locally:

```sh
make install
```

To deploy remotely, ensure:

- Host is accessible via SSH
- Host has docker compose installed
- SSH params are tweaked (`MaxStartups 200`)
- `DOCKER_HOST` is set locally

```sh
make start
```
