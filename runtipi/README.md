# BizerOS GTD — Runtipi App Store

This folder is a self-contained [Runtipi](https://runtipi.io) app store containing the **BizerOS GTD** app definition.

## Layout

```
runtipi/
├── config.json                       # appstore manifest
└── apps/
    └── bizeros-gtd/
        ├── config.json               # Runtipi app metadata + form fields
        ├── docker-compose.yml        # PWA + cloud sync, Traefik labels
        └── metadata/
            ├── description.md
            └── logo.jpg
```

## How to install in Runtipi

Runtipi's "Custom App Store" feature clones a Git URL and reads `apps/<id>/` from the **repository root**. Because `apps/` at the root of this repo contains the source code (desktop, mobile, cloud, mcp-server), we publish the appstore layout on a parallel orphan branch.

### Recommended: add as a custom app store

1. Open your Runtipi dashboard → **App Stores** → **Add app store**.
2. URL: `https://github.com/kelsi-bizer/bizeros-gtd`
3. Branch: `runtipi-appstore`
4. Save. **BizerOS GTD** will appear in the app catalog and you can install it like any other Runtipi app.

> The `runtipi-appstore` branch is an orphan branch whose root is the contents of this `runtipi/` folder. It is regenerated from `main` whenever the app definition changes.

### Alternative: copy into your own app store fork

If you already maintain your own Runtipi app store repo:

1. Copy `runtipi/apps/bizeros-gtd/` into `apps/bizeros-gtd/` of your fork.
2. Commit + push.
3. Reload your custom app store in the Runtipi dashboard.

### Local smoke test (no Runtipi required)

```bash
APP_DOMAIN=localhost:5173 \
APP_PORT=5173 \
APP_DATA_DIR=$(pwd)/runtipi/apps/bizeros-gtd/data \
MINDWTR_CLOUD_AUTH_TOKENS=$(openssl rand -hex 24) \
MINDWTR_CLOUD_CORS_ORIGIN=http://localhost:5173 \
docker compose -f runtipi/apps/bizeros-gtd/docker-compose.yml up
```

## What the app installs

Two containers (re-using the upstream Mindwtr images so this fork stays mergeable):

| Container          | Image                                  | Port  | Purpose                          |
| ------------------ | -------------------------------------- | ----- | -------------------------------- |
| `bizeros-gtd`      | `ghcr.io/dongdongbh/mindwtr-app:latest`   | 5173  | PWA / web UI (exposed via Traefik) |
| `bizeros-gtd-cloud`| `ghcr.io/dongdongbh/mindwtr-cloud:latest` | 8787  | Sync + REST + MCP server (internal) |

## User-facing form fields

On install, Runtipi prompts for:

- **Cloud auth token** (`MINDWTR_CLOUD_AUTH_TOKENS`) — random by default; bearer token shared by the PWA and any clients hitting `/v1/`.
- **Public PWA URL** (`MINDWTR_CLOUD_CORS_ORIGIN`) — e.g. `https://gtd.example.com`. Used by the cloud sync server to validate CORS.

> Internal env-var names retain the upstream `MINDWTR_*` prefix so the official container images can be used unmodified.
