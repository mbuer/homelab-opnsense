# Network Design

## Current Topology

OPNsense currently runs as a single-interface VM on Proxmox.

    Home LAN
        |
      vmbr0
        |
     OPNsense
    192.168.1.250

The VM is not used as the home Internet gateway.

## Current Role

OPNsense is currently used for:

- Web GUI management
- DNS experiments
- VPN testing
- Firewall learning
- Future DHCP testing
- Future network segmentation

The existing home router continues to provide:

- Internet access
- Default gateway
- DHCP for the main LAN
- Normal client DNS

## OPNsense VM

- VM ID: `102`
- CPU: 2 vCPU
- Memory: 4 GB
- Disk: 32 GB
- NIC: VirtIO
- Bridge: `vmbr0`
- LAN IP: `192.168.1.250/24`

## Planned Lab Network

A second virtual interface will later be added to OPNsense.

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

This will allow OPNsense to handle routing, firewall rules, DHCP, and DNS for an isolated internal lab without changing the home Internet setup.

## Design Principles

- Do not use OPNsense as the ISP/WAN gateway yet
- Keep the home network independent from lab experiments
- Use an isolated subnet for firewall testing
- Introduce NAT only when there is a specific reason
- Keep long-term monitoring and observability outside the firewall VM
