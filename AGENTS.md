# AGENTS.md

## Purpose

This repository documents and automates a home OPNsense lab running on Proxmox.

## Rules

- Never commit passwords, private keys, pre-shared keys, API credentials, or certificate private keys.
- Never commit raw OPNsense configuration exports without sanitizing them first.
- Do not add public WAN IP addresses, DDNS hostnames, or other identifying network information.
- RFC1918 lab addresses may be documented.
- Keep documentation aligned with the actual deployed configuration.
- Prefer simple, readable configuration and documentation over unnecessary complexity.
- Do not assume OPNsense is the home Internet gateway; the current deployment uses a single LAN interface.
- WireGuard-to-LAN access currently relies on Source NAT because LAN clients do not have a route back to the WireGuard subnet.

## Documentation

Update the relevant document when behavior changes:

- `README.md` — project overview and current status
- `docs/installation.md` — Proxmox and OPNsense installation
- `docs/network-design.md` — topology and design decisions
- `docs/wireguard.md` — VPN configuration and routing behavior
