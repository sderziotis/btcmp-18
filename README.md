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

Software->Python3, pip, pipx, Node.js, npm, Git, build-essential, wget, curl, jq, unzip, zip, Firefox, Chromium, ExifTool, ffmpeg, ImageMagick, Docker Engine, Docker Compose plugin, TOr Browser, Maltego CE, SpiderFoot, Sherlock, java

Notes:
- Tor Browser is installed via torbrowser-launcher — the user runs tor the first time to download and set it up.
- Maltego CE vers 4.8.0 — if this goes stale you'll need to update the URL on playbook from https://www.maltego.com/downloads/. Also it asks for registration!!!
- SpiderFoot runs via python3 sf.py -l 127.0.0.1:5001 from /opt/spiderfoot.
- Sherlock runs via terminal sherlock <username>.


---

### LAB1 — Server Machine
User `user` / `btcmp18@admin` (sudo enabled).

Software-> Python3, pip, pipx, Node.js, npm, PHP 8.3, php-cli, php-fpm, Git, build-essential, Apache2, libapache2-mod-php, wget, curl, jq, unzip, zip, rsync, net-tools, dnsutils, openssl, OpenSSH Server, SQLite3, cron, Docker Engine, Docker Compose plugin, 

**Services enabled at boot:** Apache2, OpenSSH, cron, Docker.

---

## Notes
- Docker is installed from the official Docker repository on both machines.
- The `user` account is added to the `docker` group on both machines.
- PHP defaults to version **8.3** as provided by Ubuntu 24.04 repos.