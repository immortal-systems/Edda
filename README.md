# Edda

> A fast, lightweight API client and connection toolkit by **Immortal Systems**. Desktop app and CLI, powered by one shared Go core.

![Status](https://img.shields.io/badge/status-early%20development-orange)
![Version](https://img.shields.io/badge/version-0.1.0-blue)
![Go](https://img.shields.io/badge/Go-1.22+-00ADD8?logo=go&logoColor=white)
![Wails](https://img.shields.io/badge/Wails-v3%20alpha-red)
![React](https://img.shields.io/badge/React-TypeScript-61DAFB?logo=react&logoColor=black)
![License](https://img.shields.io/badge/license-Proprietary-lightgrey)

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Current Status](#current-status)
- [Roadmap](#roadmap)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)
- [Contact](#contact)

---

## Overview

**Edda** is a desktop client for working with APIs and remote systems, and an alternative to tools like Postman. Our goal is to fully cover the HTTP protocol and grow Edda into a single workspace for engineers: send requests, inspect responses, and connect to servers over SSH without switching tools.

**Principles**

- **Fast**: Go core and a lightweight native shell instead of a bundled browser runtime.
- **Unified**: HTTP, SSH, and other protocols in one place.
- **Scriptable**: everything the GUI can do is available from the CLI.
- **Private**: your data stays on your machine.

---

## Architecture

Edda is built around a **single core library written in Go**. The desktop app and the CLI are thin frontends on top of it, so behavior is identical everywhere.

```
┌──────────────────────┐   ┌──────────────────┐
│   Desktop app        │   │       CLI        │
│ Wails v3 + React/TS  │   │       (Go)       │
└──────────┬───────────┘   └────────┬─────────┘
           │   Wails services        │  direct calls
           └────────────┬────────────┘
                ┌───────▼────────┐
                │   Edda Core    │
                │      (Go)      │
                ├────────────────┤
                │ HTTP │ SSH │ … │
                └────────────────┘
```

| Layer | Technology |
|---|---|
| Core | Go |
| Desktop shell | [Wails v3](https://v3alpha.wails.io) |
| Frontend | React + TypeScript |
| CLI | Go |

---

## Current Status

Edda is in early development. Right now it supports **simple HTTP requests**.

| Feature | Status |
|---|---|
| Basic HTTP requests (GET, POST, PUT, DELETE) | Available |
| Headers and request body | Available |
| Response viewer (status, headers, body) | Available |
| Full HTTP coverage | In progress |
| CLI | In progress |
| SSH client | Planned |
| Other protocols | Planned |

> **Note:** Wails v3 is currently in alpha. APIs and tooling may change between releases.

---

## Roadmap

### HTTP (full coverage)
- [x] Simple requests
- [ ] All standard methods (PATCH, HEAD, OPTIONS, and custom methods)
- [ ] Query parameters and form-data / binary / raw bodies
- [ ] Authentication: Basic, Bearer, API keys, OAuth 2.0
- [ ] Cookies, redirects, proxies, and TLS options
- [ ] HTTP/2 and HTTP/3
- [ ] Timing, size, and detailed response inspection
- [ ] Collections, environments, and variables
- [ ] Request history

### Core and CLI
- [ ] Extract all logic into a standalone core package
- [ ] CLI with feature parity for HTTP
- [ ] Machine-readable output (JSON) for CI/CD usage
- [ ] Shared collection and environment file format

### Beyond HTTP
- [ ] SSH sessions with saved hosts and key-based auth
- [ ] SFTP file browser
- [ ] SSH tunnels and port forwarding
- [ ] WebSocket, GraphQL, and gRPC
- [ ] Team workspaces and sync

Have a feature request? Open an [issue](../../issues) and describe your use case.

---

## Getting Started

### Prerequisites

- [Go](https://go.dev/dl/) 1.22+
- [Node.js](https://nodejs.org/) 20+ and npm
- [Wails v3 CLI](https://v3alpha.wails.io/getting-started/installation/)
- Platform dependencies for Wails (run `wails3 doctor` to check)

```bash
go install github.com/wailsapp/wails/v3/cmd/wails3@latest
wails3 doctor
```

### Run in development mode

```bash
git clone https://github.com/immortal-systems/edda.git
cd edda

wails3 dev
```

Hot reload is enabled for both the Go backend and the React frontend.

### Build the desktop app

```bash
wails3 build
```

The binary is placed in `bin/`.

### Build the CLI

```bash
go build -o bin/edda ./cmd/edda
```

---

## Usage

### Desktop app

1. Launch **Edda**.
2. Create a new request.
3. Choose a method and enter a URL, e.g. `https://api.example.com/users`.
4. Add headers or a body if needed.
5. Press **Send** and inspect the response.

### CLI

> The CLI is under active development. The interface below is the target design and may change.

```bash
# Simple GET request
edda request GET https://api.example.com/users

# POST with headers and a JSON body
edda request POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Ada"}'
```

---

## Project Structure

```
edda/
├── main.go             # Wails application entry point
├── services/           # Wails services exposed to the frontend
├── cmd/
│   └── edda/           # CLI entry point
├── internal/
│   └── core/           # Shared core (HTTP, and later SSH, etc.)
├── frontend/           # React + TypeScript UI
│   ├── src/
│   └── package.json
├── build/              # Platform build assets and configuration
├── Taskfile.yml        # Build tasks (Wails v3)
└── README.md
```

---

## Contributing

1. Create a branch from `main`: `git checkout -b feature/your-feature`
2. Put business logic in `internal/core`, not in the UI or CLI layers.
3. Add tests and make sure they pass:
   ```bash
   go test ./...
   cd frontend && npm test
   ```
4. Use clear commit messages, e.g. `feat(core): add PATCH method`.
5. Open a Pull Request and request a review.

---

## Security

To report a vulnerability, **do not open a public issue**. Email **security@immortal-systems.com** instead.

---

## License

Copyright (c) 2026 Immortal Systems. All rights reserved.

---

## Contact

- **Company**: Immortal Systems
- **Email**: support@immortal-systems.com
- **Issues**: [GitHub Issues](../../issues)
