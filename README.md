# DeckOps

**A lightweight, visual Docker Compose management UI that stays out of your way.**

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-0.1.0-orange)](https://github.com/jxmullins/deckops)
[![Built with Antigravity](https://img.shields.io/badge/Built%20with-Antigravity-4E86F8?logo=google-gemini)](https://github.com/jxmullins/deckops)

-----

## The Problem

Managing Docker Compose stacks shouldn’t require a bloated application eating 2GB of RAM just to show you which containers are running.

**Old way:** Portainer is powerful but heavy. Docker Desktop is resource-hungry and overkill for simple compose workflows. CLI-only works but context-switching to check status breaks your flow.

**DeckOps:** A compose-first management UI that’s actually lightweight. Visual dashboard with real-time status, grouped by stack, running in under 30MB of memory. See what’s running, start/stop services, and get back to work.

-----

## Quick Demo

![DeckOps Screenshot](assets/screenshot.png)

*(Screenshot coming soon)*

-----

## Install

### Docker (Recommended)

```bash
docker build -t deckops .
docker run -d -p 3000:3000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /path/to/stacks:/stacks \
  deckops
```

### Development

```bash
# Backend
cd backend
go run .

# Frontend
cd frontend
npm install
npm run dev
```

-----

## Features

- **Visual Dashboard** — Container cards with real-time status updates via SSE.
- **Compose-First** — Native `compose.yaml` file management, not an afterthought.
- **Stack Grouping** — Containers grouped by compose project for easy navigation.
- **Lightweight** — Under 30MB memory footprint, under 20MB image size.
- **No Bloat** — Does one thing well instead of trying to be Kubernetes.

-----

## Tech Stack

|Layer        |Technology              |
|-------------|------------------------|
|**Backend**  |Go + Docker SDK         |
|**Frontend** |SvelteKit + Tailwind CSS|
|**Real-time**|Server-Sent Events (SSE)|

-----

## Roadmap

Planned features for future releases:

- Container logs viewer
- Compose file editor
- Stack templates
- Resource usage graphs
- Multi-host support

-----

## Why I Built This

I run a handful of self-hosted services and got tired of the two extremes: either memorizing `docker compose` commands for each stack, or running Portainer and watching it consume more resources than my actual services.

DeckOps is the middle ground — a visual layer over compose that respects your system resources.

-----

## Support

If this project helps you, consider buying me a coffee:

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?logo=buy-me-a-coffee)](https://buymeacoffee.com/jxmullins)

-----

## Contributing

Ideas and contributions welcome! Open an issue or submit a PR.

-----

## License

DeckOps is licensed under the [GNU Affero General Public License v3.0 (AGPL-3.0)](LICENSE).

This means you’re free to use, modify, and distribute DeckOps — including hosting it as a service — as long as you:

- Keep the source code available to your users
- License your modifications under AGPL v3
- Preserve copyright notices

**Note:** The “DeckOps” name is a trademark. If you fork this project, please rename your version. See <TRADEMARK.md> for details.
