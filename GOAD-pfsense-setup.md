### Disclaimer

> [!NOTE]
> These notes are ~~strongly inspired~~ copied (with minor adjustments) from [Mayfly's notes](https://mayfly277.github.io/posts/GOAD-on-proxmox-part1-install/#prepare-for-pfsense) as part of his installation notes for Game of Active Directory Proxmox setup. 
> 
> I have added a few steps for my own understanding as some of the details were not immediately apparent to me when I followed his notes.
> 
> I strongly recommend that you stick to his notes as mine are merely interpretation. I could be leading you astray.

### Three Linux Bridges

The physical host I used contains  1 physical NIC, `vmbr0` was created by default.

![Initial Linux Bridges](https://raw.githubusercontent.com/quincyntuli/pfsense/main/img/initial-linux-bridge-2.png)

The image above depicts the initial network layout. This is Proxmox box is sitting in a home LAN with IP address 192.168.2.106 as depicted by `vmbr0`.

Additional virtual network bridges will be defined as follows.

- vmbr1
  - 1 subnet defined for WAN (IP4/CIDR 10.0.0.1/30)
    - 10.0.0.1 - for Proxmox host
    - 10.0.0.2 - for pfSense VM
- vmbr2 
  - 1 subnet defined for  LAN (IP4/CIDR 192.168.1.1/24)
    - 192.168.1.1 - Proxmox host
    - 192.168.1.2 - pfSense VM
    - 192.168.1.3 - Provisioning container
- vmbr3
  - 2 VLANs defined for VLAN1 (IP4/CIDR 192.168.10.0/24) and VLAN2 (IP4/CIDR 192.168.20.0/24)

### 1. Setup the Bridges and VLANs

![()](https://github.com/quincyntuli/pfsense/raw/main/mp4/output-1m.webp)

### 2. Download the .iso

![()](https://github.com/quincyntuli/pfsense/raw/main/img/GOAS-New-3-pic-at-end.webp)

### 3. Upload pfSense .iso to Proxmox

![()](https://github.com/quincyntuli/pfsense/raw/main/img/Upload-iso.webp)


### 4. Setup shell for pfSense

![()](https://github.com/quincyntuli/pfsense/raw/main/img/Create-Shell-For-pfsense.webp)









### 5. Install pfSense

Start the VM and follow the prompts as directed. The end result should be pfsense interfaces configured as follows:

- WAN --> vtnet0 --> 10.0.0.2/30
- LAN --> vtnet1 --> 192.168.1.2/24
- OPT1 --> vtnet2 --> (blank - no IP addresses defined.)


### 6. Configure pfsense





