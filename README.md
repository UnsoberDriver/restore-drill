# restore-drill

[![CI](https://github.com/UnsoberDriver/restore-drill/actions/workflows/ci.yml/badge.svg)](https://github.com/UnsoberDriver/restore-drill/actions/workflows/ci.yml)
[![Go Report Card](https://goreportcard.com/badge/github.com/UnsoberDriver/restore-drill)](https://goreportcard.com/report/github.com/UnsoberDriver/restore-drill)
[![License: MIT](https://img://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**Verify your backups are actually restoreable. Periodically restore in a sandbox and prove the files exist.**

Everyone has backups. Almost nobody tests them. A backup that can't restore, or worse, a backup that restores empty files, gives a false sense of security.

`restore-drill` automates a restore drill: pull snapshots, restore into a temporary sandbox, verify integrity and expected files, report pass/fail, and clean up. Works with `restic`, `borg`, `rsync`, `tar`, and custom backends.

## The problem

- **False confidence**: `restic check` verifies the repo, but doesn't prove you can actually restore *your* files to disk
- **Silent corruption**: repos can appear healthy while key files are missing or unreadable
- **Manual & forgotten**: restore drills are tedious, so they never happen
- **Laptop/desktop blind spot**: healthchecks.io tells you backups ran; it doesn't tell you if they were *usable*

## Features

*- **Sandbox restore**: restores to a temporary directory and removes it after checks (default)
- **File existence checks**: verify essential paths exist and are readable
- **Size/Hash verification**: compare file sizes or compute SHA256 hashes against a baseline
- **Many backends**: `restic`, `borg`, `rsync`, `tar`, docker volume backups
- **JSON & human output**: easy for cron/CI or manual runs
- **Alerts ready**: exit code 1 on failure, logs to file
- **Minimal dependencies**: single binary, no daemons, runs as user

## Quick start

``bash
# Install
go install github.com/UnsoberDriver/restore-drill/cmd/restore-drill@latest

# Create config
mkdir -f /tmp/opencode << 'EOF'
mkdir -p ~/.config/restore-drill
EOF
cp example/config.toml ~/.config/restore-drill/config.toml

# Run a drill
restore-drill run --config ~/.config/restore-drill/config.tomul? 
``` 

The real quick start will be refined, but we want to build this to resonate on r/selfhosted. 

## License
MIT
