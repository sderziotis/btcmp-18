# btcmp-18-main — Lab Environment

## Overview

This lab environment uses **xubuntu-noble-x86_64** as the base image for both machines.

---

## Topology

| Host | Network | CIDR | IP |
|------|---------|------|----|
| inv1 | network-2 | 192.168.0.0/24 | 192.168.0.10 |
| lab1 | network-3 | 192.168.10.0/24 | 192.168.10.10 |

All three simulated web properties resolve to **lab1 (192.168.10.10)** via `/etc/hosts` entries injected on both machines.

---

## Machines

### INV1 — Investigator Workstation
**User:** `user` / `Password123` (sudo enabled)

**Software:** Python3, pip, pipx, Node.js, npm, Git, build-essential, wget, curl, jq, unzip, zip, Firefox, Chromium, ExifTool, ffmpeg, ImageMagick, Docker Engine, Docker Compose plugin, Tor Browser, SpiderFoot, Sherlock, Java

**Notes:**
- Tor Browser is installed via `torbrowser-launcher` — user must run it once to complete download and setup.
- Maltego CE v4.8.0 — update the deb URL in the playbook if stale. Requires registration on first launch.
- SpiderFoot: `python3 sf.py -l 127.0.0.1:5001` from `/opt/spiderfoot`
- Sherlock: `sherlock <username>` from terminal

---

### LAB1 — Server Machine
**User:** `user` / `btcmp18@admin` (sudo enabled)

**Software:** Python3, pip, pipx, Node.js, npm, PHP 8.3, php-cli, php-fpm, Git, build-essential, Apache2, wget, curl, jq, unzip, zip, rsync, net-tools, dnsutils, openssl, OpenSSH Server, SQLite3, cron, Docker Engine, Docker Compose plugin, Flask, Gunicorn

**Services enabled at boot:** Apache2, OpenSSH, cron, Docker, Famebook (systemd)

---

## Simulated Web Properties

All domains resolve to `192.168.10.10` (lab1) via `/etc/hosts` on both machines.

### www.silvercompany.com — Corporate Site #1
- **Stack:** Apache2 virtual host, static HTML
- **Persona:** Silver Company GmbH — precision manufacturing, Frankfurt HQ
- **OSINT breadcrumbs:**
  - Staff names, emails, and job titles on the homepage (`m.hale@`, `e.voss@`, `d.orlov@`, `s.kimani@`)
  - `robots.txt` exposes: `/portal/`, `/internal/`, `/backup/`, `/admin/`
  - Server headers leak: `X-Powered-By: PHP/8.3.1`, `X-Generator: WordPress/6.4.2`
  - Company registration number, VAT ID, and phone in footer
  - Job listings reveal internal tech stack (Ansible, Kubernetes, GitLab CI, FreeRTOS, STM32)

### www.bluefeather.com — Corporate Site #2
- **Stack:** Apache2 virtual host, static HTML
- **Persona:** Blue Feather Media Ltd — digital PR agency, London
- **OSINT breadcrumbs:**
  - Staff directory with emails (`n.cross@`, `a.romanov@`, `p.nair@`, `t.bekele@`)
  - `robots.txt` exposes: `/wp-admin/`, `/client-reports/`, `/.git/`
  - Server headers leak: `X-Generator: Joomla! 4.3`
  - Blog post references Silver Company and "the Orlov incident" — cross-site link
  - Company number (England & Wales) in footer

### www.famebook.com — Social Media Platform
- **Stack:** Flask app (systemd service on port 5000) + Apache2 reverse proxy
- **Personas & cross-links:**

| Handle | Name | Affiliation | Key OSINT in posts |
|--------|------|-------------|-------------------|
| `marcus.hale` | Marcus Hale | CEO, Silver Company | Mentions Athens expansion, meeting with Nina Cross, uses Ansible |
| `elena.voss` | Elena Voss | CTO, Silver Company | VPN policy change, FreeRTOS firmware, Warsaw team |
| `dmitri.orlov` | Dmitri Orlov | Head of Ops, Silver Company | Athens warehouse, IT helpdesk complaint, email in bio |
| `nina.cross` | Nina Cross | Founder, Blue Feather | References "Orlov data story", runs OSINT audits |
| `tom.bekele` | Tom Bekele | Social Media Mgr, Blue Feather | Manages SilverCompany page, mentions API key |
| `alexei.romanov` | Alexei Romanov | Head of Digital, Blue Feather | Email in bio, posts about exposed `.git` dirs and leaked API keys |

**Routes:**
- `/` — main feed (all posts, chronological)
- `/people` — all profiles grid
- `/profile/<handle>` — individual profile page

---

## DNS / Domain Resolution Strategy

`/etc/hosts` entries are injected on **both** machines by the playbook using Ansible `blockinfile`:

```
192.168.10.10  www.silvercompany.com silvercompany.com
192.168.10.10  www.bluefeather.com bluefeather.com
192.168.10.10  www.famebook.com famebook.com
```

This is the simplest and most reliable approach for a closed KYPO range — no dnsmasq or extra DNS server required. Apache ServerName/ServerAlias directives handle vhost routing based on the `Host:` header.

---

## OSINT Challenge Ideas

- Enumerate `robots.txt` on both corporate sites to discover hidden paths
- Correlate staff emails across silvercompany.com and famebook.com profiles
- Cross-reference Alexei Romanov's post about API keys with Tom Bekele's post
- Find the connection between Blue Feather and Silver Company via blog post + Famebook posts
- Use ExifTool / Sherlock / SpiderFoot on inv1 against the simulated targets
- Fingerprint the server stack via HTTP response headers

---

## Notes
- Docker is installed from the official Docker repository on both machines.
- PHP defaults to **8.3** as provided by Ubuntu 24.04 repos.
- The `user` account is added to the `docker` group on both machines.
- Famebook Flask app runs as `www-data` via systemd; Apache proxies port 80 → 5000.
- To restart Famebook manually: `sudo systemctl restart famebook`
