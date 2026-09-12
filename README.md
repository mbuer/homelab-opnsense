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
- LAN IP: `192.168.1.250/24`
- No WAN interface
- No NAT
- DHCP disabled on the main LAN
- Unbound DNS enabled

## Goals

- [x] Install OPNsense on Proxmox
- [x] Configure management access
- [ ] Configure WireGuard VPN
- [ ] Add an isolated lab network
- [ ] Run DHCP on the lab network
- [ ] Use OPNsense DNS for internal hostnames
- [ ] Learn firewall rules and network segmentation
- [ ] Test logging and traffic analysis
- [ ] Explore IDS/IPS
- [ ] Integrate useful telemetry with Grafana

## Planned Network

    Home LAN
        |
      vmbr0
        |
     OPNsense
        |
      vmbr1
        |
    Lab VMs / Containerlab

The second interface will be added later for isolated firewall and routing experiments.

## Notes

OPNsense is currently not used as the home Internet gateway.

The existing router continues to handle Internet access and DHCP for the main network.

Raw OPNsense configuration exports should not be committed without checking them for passwords, certificates, VPN keys, or other secrets.
