<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="ONLYOFFICE logo" width="128">
  </picture>
</p>

<h1 align="center">aac-onlyoffice</h1>

<p align="center">Self-hosted deployment definition for ONLYOFFICE Docs, an online office suite for documents, spreadsheets and presentations.</p>

<p align="center">
  <a href="https://github.com/ONLYOFFICE/DocumentServer"><img alt="Upstream" src="https://img.shields.io/badge/upstream-ONLYOFFICE%2FDocumentServer-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-onlyoffice%2Fdocumentserver%3A10.0-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/ONLYOFFICE/DocumentServer/blob/master/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-AGPL--3.0-green"></a>
</p>

## About

ONLYOFFICE Docs (Document Server) is the editing back end of the ONLYOFFICE office suite. It
renders and edits text documents, spreadsheets, presentations and forms in the browser and
is designed to be embedded into third-party applications through its API. Documents are
edited collaboratively, and the server handles conversion and co-authoring.

Key features (as provided by upstream):

- Editing of text documents, spreadsheets, presentations and PDF forms in the browser
- Real-time co-editing with change tracking and comments
- Conversion between common office formats
- Integration API for embedding into other applications
- JWT-based request signing to protect the document API

## Upstream

- Project: <https://github.com/ONLYOFFICE/DocumentServer>
- Website: <https://www.onlyoffice.com>
- Documentation (Docker installation): <https://helpcenter.onlyoffice.com/docs/installation/docs-community-install-docker.aspx>
- Container image: `onlyoffice/documentserver:10.0` (Docker Hub)
- License: AGPL-3.0 (Community Edition, upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, the external networks used for routing must exist first:

```bash
docker network create services-secured
docker network create services-exposed
# "docker-default" is the default Docker bridge and is referenced as external as well

# Provide the values listed under "Configuration" in .env / stack.env
docker compose up -d
```

Secrets are not stored in this repository. Provide them through the deployment pipeline
or create the `.env` and `stack.env` files yourself (see below). Do not commit them.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `JWT_ENABLED` | `true` | Enable JWT validation of requests to the document server |
| `JWT_SECRET` | `CHANGE_ME` (secret) | Shared secret used to sign and verify JWT tokens |
| `JWT_HEADER` | `Authorization` | HTTP header that carries the JWT token |
| `ONLYOFFICE_FORCESAVETOKEN_HOST` | `CHANGE_ME` (secret) | Host allowed to trigger forced save requests |
| `ONLYOFFICE_HTTPS_HSTS_ENABLED` | `false` | Send the HSTS header (only meaningful behind HTTPS) |
| `ALLOW_PRIVATE_IP_ADDRESS` | `true` | Allow requests to private IP addresses (needed for internal storage back ends) |
| `USE_UNAUTHORIZED_STORAGE` | `true` | Allow a storage back end that is not authorized by the document server |
| `TZ` | `Europe/Berlin` | Time zone |
| `PUID` / `PGID` | `1000` | User and group ID the container runs as |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `80` (published as `12888`) | TCP | Document server web interface and API (`http`) |

Volumes:

| Mount | Description |
|---|---|
| `logs` -> `/var/log/onlyoffice` | Document server log files |
| `data` -> `/var/www/onlyoffice/Data` | Certificates and other application data |
| `lib` -> `/var/lib/onlyoffice` | File cache and other library data |
| `db` -> `/var/lib/postgresql` | Internal PostgreSQL database |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-onlyoffice>.

## Credits and license

ONLYOFFICE Docs is developed by Ascensio System SIA and licensed under AGPL-3.0 (Community
Edition). This repository only contains the deployment definition for it and is not a fork
of the application. The logo is provided by the ONLYOFFICE project; see
[`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources and licenses.
