# BizerOS GTD

**BizerOS GTD** is a self-hosted, local-first Getting Things Done (GTD) task manager for the [BizerOS](https://github.com/kelsi-bizer) self-hosting stack. It is a downstream rebrand of the excellent [Mindwtr](https://github.com/dongdongbh/Mindwtr) project by Dongda Li, packaged for one-click install on [Runtipi](https://runtipi.io).

This Runtipi app installs two containers:

- **PWA** — the BizerOS GTD web/desktop UI, served by Nginx and exposed through Traefik on the domain you configure.
- **Cloud sync server** — a lightweight self-hosted sync + REST API + MCP backend on port `8787`, reachable from the PWA over the Runtipi internal network.

## Features

- Full GTD workflow: Capture, Clarify, Organize, Reflect, Engage.
- Local-first data model — your tasks live in your browser/device, not in a vendor cloud.
- Self-hosted sync between desktop, mobile, and PWA via the bundled cloud server.
- REST API and MCP server for automation (Home Assistant, n8n, Claude, etc.).
- Optional AI copilot (bring your own key, including local LLMs).
- Open source under AGPL-3.0.

## Setup

1. Install the app from your Runtipi dashboard.
2. Provide a **Cloud auth token** (or let Runtipi generate one) and the **public PWA URL** (e.g. `https://gtd.example.com`).
3. Open the PWA, go to **Settings → Sync → Self-Hosted**, and paste:
   - URL: `https://gtd.example.com/v1` (the same domain you configured)
   - Token: the value of `MINDWTR_CLOUD_AUTH_TOKENS`

## Notes on naming

Internal service names, environment variables (`MINDWTR_CLOUD_*`), and container images keep the upstream `mindwtr` prefix so that BizerOS GTD can track upstream Mindwtr releases without divergence. Only user-facing branding has been changed.

## Credits

- Upstream project: [Mindwtr](https://github.com/dongdongbh/Mindwtr) by [Dongda Li](https://dongdongbh.tech)
- License: AGPL-3.0
