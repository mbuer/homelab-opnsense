# WireGuard

## Goal

Use OPNsense as a remote-access WireGuard VPN server without making OPNsense the home Internet gateway.

The existing home router continues to handle:

- Internet access
- WAN routing
- DHCP for the main LAN
- Default gateway for LAN clients

OPNsense provides the WireGuard VPN service and access into the existing LAN.

## Topology

    Remote Phone
        |
     Internet
        |
    Home Router
    UDP 51820
        |
     OPNsense
        |
    Existing LAN
        |
    Proxmox / Lab Services

OPNsense currently uses a single LAN interface.

## WireGuard Network

WireGuard uses a separate tunnel network:

    10.10.10.0/24

Example addressing:

    OPNsense WireGuard interface: 10.10.10.1/24
    Android client:              10.10.10.2/32

## OPNsense Instance

The WireGuard server instance uses:

- Listen port: `51820`
- Tunnel address: `10.10.10.1/24`
- CARP: disabled
- Debug logging: disabled during normal operation

The server private key remains stored only in OPNsense.

The server public key is used by WireGuard clients.

## Android Peer

The Android phone is configured as a WireGuard peer.

OPNsense peer settings:

- Allowed IP: `10.10.10.2/32`
- Endpoint address: blank
- Endpoint port: blank
- Instance: WireGuard server instance

The phone public key is entered into the OPNsense peer configuration.

## Android Client

Example client configuration:

    Interface
    Address: 10.10.10.2/32
    Listen Port: automatic

    Peer
    Public Key: OPNsense server public key
    Endpoint: <public-address>:51820
    Allowed IPs: 192.168.1.0/24
    Persistent Keepalive: 25

The public WAN address is intentionally not stored in this repository.

## Router Port Forward

The existing home router forwards WireGuard traffic to OPNsense.

    Protocol: UDP
    External Port: 51820
    Internal Port: 51820
    Destination: OPNsense LAN address

Because the router only allows port forwarding to reserved DHCP clients, OPNsense currently receives its LAN address through DHCP with an IP reservation on the router.

## OPNsense Firewall Rule

Incoming WireGuard handshake traffic is allowed on the LAN interface.

    Interface: LAN
    Action: Pass
    Direction: In
    Version: IPv4
    Protocol: UDP
    Source: any
    Destination: This Firewall
    Destination Port: 51820

## WireGuard Traffic Rule

Traffic from WireGuard clients into the home LAN is allowed through the WireGuard group.

    Interface: WireGuard (Group)
    Action: Pass
    Direction: In
    Version: IPv4
    Protocol: any
    Source: 10.10.10.0/24
    Destination: 192.168.1.0/24

## Source NAT

The current single-interface design requires Source NAT for WireGuard-to-LAN traffic.

LAN devices use the existing home router as their default gateway and do not have a route back to the WireGuard network.

Without NAT:

    Phone
    10.10.10.2
        |
     OPNsense
        |
    LAN Device
    192.168.1.x

The LAN device receives traffic from `10.10.10.2`, but its reply follows the normal default gateway instead of returning through OPNsense.

The Source NAT rule solves this by translating WireGuard client traffic to the OPNsense LAN address.

Source NAT rule:

    Interface: LAN
    Version: IPv4
    Protocol: any
    Source: 10.10.10.0/24
    Destination: 192.168.1.0/24
    Translate Source IP: OPNsense LAN address

This allows LAN devices to return traffic naturally to OPNsense.

## Testing

The WireGuard handshake was verified using OPNsense packet capture.

Successful traffic showed:

- UDP packets arriving on port `51820`
- OPNsense replying to the remote client
- an established WireGuard handshake
- traffic passing from the VPN network into the LAN

Remote access to Proxmox was then successfully tested through the WireGuard tunnel.

## Security

Do not commit:

- WireGuard private keys
- pre-shared keys
- public WAN IP addresses
- DDNS hostnames
- raw OPNsense configuration exports
- passwords
- certificate private keys

Only sanitized examples and private RFC1918 network addresses should be stored in this repository.
