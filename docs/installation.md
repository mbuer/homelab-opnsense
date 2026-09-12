# Installation

## Proxmox VM

OPNsense is installed as a virtual machine on Proxmox.

### VM Configuration

- VM ID: `102`
- Name: `opnsense`
- Guest OS: Other
- Machine: `q35`
- BIOS: `OVMF (UEFI)`
- SCSI Controller: `VirtIO SCSI single`
- CPU Type: `host`
- Sockets: `1`
- Cores: `2`
- Memory: `4096 MiB`
- Disk: `32 GiB`
- Network Model: `VirtIO`
- Bridge: `vmbr0`

## OPNsense Installation

Installation media:

- Architecture: `amd64`
- Image type: `dvd`

The ISO was uploaded to Proxmox and attached as a virtual CD/DVD drive.

During installation:

- Installer login: `installer`
- Filesystem: ZFS
- ZFS layout: single-disk stripe
- Target disk: 32 GB virtual disk

After installation:

1. Reboot the VM
2. Remove the OPNsense ISO
3. Make sure the virtual disk is first in the boot order

## Initial Network Setup

OPNsense currently uses a single LAN interface.

- Interface: `vtnet0`
- LAN IP: `192.168.1.250/24`
- IPv6: disabled
- DHCP Server: disabled
- WAN interface: none
- NAT: not configured
- Web GUI: HTTPS
- Certificate: self-signed

The existing home router remains responsible for Internet access, DHCP, and the normal network gateway.

## Notes

The initial boot from the ISO runs OPNsense in live-media mode.

Configuration made in live-media mode is not persistent.

The installer must be launched using:

    username: installer
    password: opnsense

After installation to the virtual disk, the live-media warning should no longer appear.
