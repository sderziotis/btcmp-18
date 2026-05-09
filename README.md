# btcmp-18-main — Lab Environment

## Overview

This lab environment is using **xubuntu-noble-x86_64** as the base image for both machines.

---

## Topology

The environment consists of two hosts each on their own internal network, both routed through `router-1`.

| Host | Network | CIDR | IP |
|------|---------|------|----|
| inv1 | network-2 | 192.168.0.0/24 | 192.168.0.10 |
| lab1 | network-3 | 192.168.10.0/24 | 192.168.10.10 |


---

## Machines

### INV1 — Investigator Workstation
User `user` / `Password123` (sudo enabled).

| Category | Software |
|----------|----------|
| Languages & Runtime | Python3, pip, pipx, Node.js, npm |
| Development | Git, build-essential |
| Utilities | wget, curl, jq, unzip, zip |
| Browsers | Firefox ESR, Chromium |
| Forensics & Media | ExifTool, ffmpeg, ImageMagick |
| Containers | Docker Engine, Docker Compose plugin |

---

### LAB1 — Server Machine
User `user` / `btcmp18@admin` (sudo enabled).

| Category | Software |
|----------|----------|
| Languages & Runtime | Python3, pip, pipx, Node.js, npm, PHP 8.3, php-cli, php-fpm |
| Development | Git, build-essential |
| Web Server | Apache2, libapache2-mod-php |
| Utilities | wget, curl, jq, unzip, zip, rsync |
| Network & Security | net-tools, dnsutils, openssl, OpenSSH Server |
| Database | SQLite3 |
| System | cron |
| Containers | Docker Engine, Docker Compose plugin |

**Services enabled at boot:** Apache2, OpenSSH, cron, Docker.

---

## Notes
- Docker is installed from the official Docker repository on both machines.
- The `user` account is added to the `docker` group on both machines.
- PHP defaults to version **8.3** as provided by Ubuntu 24.04 repos.