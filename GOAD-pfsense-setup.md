### Disclaimer

> [!NOTE]
> These notes are ~~strongly inspired~~ copied from [Mayfly's notes](https://mayfly277.github.io/posts/GOAD-on-proxmox-part1-install/#prepare-for-pfsense) as part of his installation notes for Game of Active Directory Proxmox setup. 
> 
> I have added a few steps for my own understanding as some of the detail was not immediately apparent to me when I followed his notes.
> 
> I strongly recommend that you stick to his notes as mine are merely interpretation.

### Three Subnets

The notes describe what I perceive as 3 separate networks; two classic subnets and a VLAN

- 10.0.0.1/30
   - 10.0.0.1 - for Proxmox
   - 10.0.0.2 - for pfsense
- 192.168.1.1/24
   - for pfsense
   - for provisioning machime
- 192.168.10.1/24 (VLAN1)
   - This the VLAN for the hosts that make up the GOAD network

This requires 3 Linux Bridges.
### Three Linux Bridges

The physical host I used contains  1 physical NIC and a USB wireless NIC. Only the physical NIC will be used for the Linux Bridge. `vmbr0` was created by default.

![Initial Linux Bridges](https://raw.githubusercontent.com/quincyntuli/pfsense/main/img/initial-linux-bridge.png)

- vmbr1 (for WAN) -- IP4/CIDR 10.0.0.1/30
- vmbr2 (for LAN)  --  IP4/CIDR 192.168.1.1/24
- vmbr3 (for 2 VLANS) - No defined addresses.

### 1. Setup the Bridges and VLANs


![()](https://github.com/quincyntuli/pfsense/raw/main/mp4/output-1m.webp)

### 2. Download the .iso

![()](https://github.com/quincyntuli/pfsense/raw/main/img/GOAS-New-3-pic-at-end.webp)




### 3. Upload .iso to Proxmox







