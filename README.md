# homelab-opnsense

OPNsense lab running as a VM on Proxmox.

The goal is to learn firewalling, VPNs, DNS, DHCP, routing, and network security without replacing the home router or putting OPNsense directly on the WAN.

## Current Setup

- OPNsense 26.7
- Proxmox VM
- 2 vCPU
- 4 GB RAM
- 32 GB disk
- VirtIO network adapter
- Single-interface deployment on the existing LAN
- No WAN interface
- OPNsense is not the home Internet gateway
- DHCP for the main LAN remains on the existing router
- Unbound DNS enabled

## Working Features

- [x] OPNsense installed on Proxmox
- [x] HTTPS management access
- [x] WireGuard remote-access VPN
- [x] Android WireGuard client
- [x] UDP port forwarding from the existing router
- [x] Firewall rules for WireGuard
- [x] Source NAT for WireGuard-to-LAN access
- [x] Remote access to Proxmox and other LAN services

## Goals

- [ ] Add an isolated lab network
- [ ] Run DHCP on the lab network
- [ ] Use OPNsense DNS for internal hostnames
- [ ] Learn firewall rules and network segmentation
- [ ] Test logging and traffic analysis
- [ ] Explore IDS/IPS
- [ ] Integrate useful telemetry with Grafana

## Current Network

    Internet
        |
    Home Router
        |
      vmbr0
        |
     OPNsense
        |
    WireGuard VPN
        |
    Remote Clients

OPNsense currently operates as a single-interface firewall/VPN appliance rather than the primary router.

WireGuard clients use a separate tunnel network. Source NAT is used for access from the WireGuard network to the existing LAN because the home router does not provide a route back to the VPN subnet.

## Planned Network

    Home LAN
        |
      vmbr0
        |
     OPNsense
        |
      vmbr1
        |
    Isolated Lab Network
        |
    VMs / Containerlab / Test Clients

The second interface will be added later for routing, DHCP, DNS, firewall, and segmentation experiments.

## Documentation

- [Installation](docs/installation.md)
- [Network Design](docs/network-design.md)
- [WireGuard](docs/wireguard.md)

## Security Notes

Do not commit:

- WireGuard private or pre-shared keys
- passwords or API credentials
- private certificates or keys
- public WAN addresses or DDNS hostnames
- raw OPNsense configuration exports
- other secrets or identifying network information

Configuration examples in this repository should be sanitized before committing.
