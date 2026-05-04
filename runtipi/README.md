# BizerOS GTD — Runtipi App

This folder is the [Runtipi](https://runtipi.io) app definition for **BizerOS GTD**. Drop the `bizeros-gtd/` folder into a Runtipi app store repository (`apps/bizeros-gtd/`) to make it installable in one click from the Runtipi dashboard.

## Layout

```
runtipi/
└── bizeros-gtd/
    ├── config.json         # Runtipi app metadata + form fields
    ├── docker-compose.yml  # Runtipi-friendly compose with Traefik labels
    └── metadata/
        ├── description.md  # Long description shown in the app store
        └── logo.jpg        # App store icon (referenced from upstream icon)
```

## Services

The app installs two containers, mirroring `docker/compose.yaml`:

- `bizeros-gtd` — the PWA (Nginx-served), exposed via Traefik on the user's chosen domain.
- `bizeros-gtd-cloud` — the lightweight sync + REST API server (port `8787`), reachable from the PWA over the Runtipi internal network.

## User-facing form fields

On install, Runtipi prompts for:

- **Cloud auth token** (`MINDWTR_CLOUD_AUTH_TOKENS`) — bearer token shared by the PWA and any clients hitting `/v1/`.
- **Cloud CORS origin** (`MINDWTR_CLOUD_CORS_ORIGIN`) — the URL where the PWA is reachable (e.g. `https://gtd.example.com`).

> Internal env-var names retain the upstream `MINDWTR_*` prefix so the official container images can be used unmodified.

## Local testing

To preview the install outside of Runtipi:

```bash
APP_DOMAIN=localhost:5173 \
APP_PORT=5173 \
APP_DATA_DIR=$(pwd)/runtipi/bizeros-gtd/data \
MINDWTR_CLOUD_AUTH_TOKENS=$(openssl rand -hex 24) \
MINDWTR_CLOUD_CORS_ORIGIN=http://localhost:5173 \
docker compose -f runtipi/bizeros-gtd/docker-compose.yml up
```
