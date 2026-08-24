# envsync

> A git-native CLI for syncing `.env` files and secrets across your team — without ever committing them to git.

[![npm version](https://img.shields.io/npm/v/envsync.svg)](https://www.npmjs.com/package/envsync)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18-brightgreen)](https://nodejs.org)

---

## The Problem

Every team hits this eventually:

- `.env` files get shared over Slack, WhatsApp, or email — untracked, unversioned, and easy to lose.
- New devs spend their first hour asking "wait, what env vars do I need?"
- Secrets drift silently between local, staging, and prod — nobody notices until something breaks.
- You *can't* commit `.env` to git for obvious reasons, but you *do* want the same version control guarantees: history, diffs, and sync.

**envsync** solves this by treating your environment variables like a git-tracked artifact — encrypted at rest, diffable, and synced through a workflow your team already understands.

---

## Features

- 🔐 **Encrypted sync** — secrets are encrypted client-side before ever leaving your machine.
- 🔄 **Git-native workflow** — push/pull env changes the same way you push/pull code.
- 📜 **Diffing** — see exactly what changed between your local `.env` and the synced version.
- 👥 **Team-friendly** — onboard a new dev with one command instead of a Slack thread.
- 🌍 **Multi-environment support** — manage `.env.development`, `.env.staging`, `.env.production` separately.
- 🧩 **Zero backend required** — syncs via a git remote you already have (no separate server to host).

---

## Installation

```bash
npm install -g envsync
```

or run it directly without a global install:

```bash
npx envsync init
```

---

## Quick Start

```bash
# Initialize envsync in your project
envsync init

# Push your local .env (encrypted) to the remote
envsync push

# Pull the latest synced env on another machine
envsync pull

# See what changed before pulling
envsync diff

# List all tracked environments
envsync list
```

---

## Commands

| Command | Description |
|---|---|
| `envsync init` | Set up envsync in the current repo, generates an encryption key |
| `envsync push` | Encrypt and push local `.env` to the configured remote |
| `envsync pull` | Fetch and decrypt the latest `.env` from the remote |
| `envsync diff` | Show a diff between local and remote env state |
| `envsync list` | List all environments being tracked (dev/staging/prod) |
| `envsync rotate-key` | Rotate the encryption key and re-encrypt existing secrets |
| `envsync --help` | Show all available commands and flags |

---

## How It Works

1. On `init`, envsync generates a local encryption key (never committed) and creates a config file that maps environments to a sync target.
2. On `push`, your `.env` is encrypted (AES-256) and the ciphertext is committed to a dedicated sync branch/remote — your plaintext secrets never touch git history.
3. On `pull`, envsync fetches the latest ciphertext and decrypts it locally using your key.
4. Team members share the encryption key through a secure out-of-band channel (once) — after that, everything syncs through normal git operations.

---

## Tech Stack

- **Runtime:** Node.js (v18+)
- **CLI framework:** [Commander.js](https://github.com/tj/commander.js)
- **Encryption:** Node's built-in `crypto` module (AES-256-GCM)
- **Terminal output:** [chalk](https://github.com/chalk/chalk) for colored output, [ora](https://github.com/sindresorhus/ora) for spinners
- **Git integration:** [simple-git](https://github.com/steveukx/git-js)
- **Config management:** local `.envsyncrc` (JSON) per project
- **Testing:** Jest
- **Packaging/Distribution:** npm

---

## Project Structure

```
envsync/
├── bin/
│   └── envsync.js          # CLI entry point
├── src/
│   ├── commands/            # init, push, pull, diff, list, rotate-key
│   ├── crypto/               # encryption/decryption logic
│   ├── git/                  # git operations wrapper
│   └── utils/                 # config parsing, logging, validation
├── test/
├── package.json
└── README.md
```

---

## Roadmap

- [ ] `envsync init`, `push`, `pull` core flow
- [ ] AES-256 encryption with key rotation
- [ ] Multi-environment support (dev/staging/prod)
- [ ] Diff command with colored output
- [ ] Team key-sharing via passphrase-derived keys
- [ ] GitHub Action for CI env validation
- [ ] Optional cloud backend (for teams without a shared git remote)

---




