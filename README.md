# DeckOps

A lightweight, visual Docker Compose management UI.

## Features

- **Visual Dashboard** - Container cards with real-time status updates
- **Compose-First** - Native compose.yaml file management
- **Stack Grouping** - Containers grouped by compose project
- **Lightweight** - <30MB memory, <20MB image size

## Tech Stack

- **Backend**: Go + Docker SDK
- **Frontend**: SvelteKit + Tailwind CSS
- **Real-time**: Server-Sent Events (SSE)

## Development

```bash
# Backend
cd backend
go run .

# Frontend
cd frontend
npm install
npm run dev
```

## Docker

```bash
docker build -t deckops .
docker run -d -p 3000:3000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /path/to/stacks:/stacks \
  deckops
```

## License

MIT
