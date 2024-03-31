# Disclaimer

> [!NOTE]
> These notes are ~~strongly inspired~~ copied (with minor adjustments) from [Mayfly's notes](https://mayfly277.github.io/posts/GOAD-on-proxmox-part1-install/#prepare-for-pfsense) as part of his installation notes for Game of Active Directory Proxmox setup. 
> 
> I have added a few steps for my own understanding as some of the details were not immediately apparent to me when I followed his notes.
> 
> I strongly recommend that you stick to his notes as mine are merely interpretation. I could be leading you astray.

## 1. Set up Linux Bridges and VLANs

The physical host I used contains  1 physical NIC, `vmbr0` was created by default.

![Initial Linux Bridges](https://raw.githubusercontent.com/quincyntuli/pfsense/main/img/initial-linux-bridge-2.png)

The image above depicts the initial network layout. This is Proxmox box is sitting in a home LAN with IP address 192.168.2.106 as depicted by `vmbr0`.

Additional virtual network bridges will be defined. I have interpreted in the following way but I have doubts in the hierarchy I have setup here.

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


![()](https://github.com/quincyntuli/pfsense/raw/main/mp4/output-1m.webp)

## 2. Download the .iso

Download pfSense Community edition. https://www.pfsense.org/download/
 
![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Download.png)

## 3. Upload pfSense .iso to Proxmox

![()](https://github.com/quincyntuli/pfsense/raw/main/img/Upload-iso.webp)


## 4. Setup VM for pfSense

i. *General Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-General-Name.png)
<center><u>Create VM - General</u></center>

ii. *OS Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Select-ISO.png)
<center><u>Select uploaded .iso</u></center>

iii. *System Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-System-Nothing-Changed.png)
<center><u>No changes on System Tab</u></center>

iv. *Disks Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Disks-Change-Disc-Size.png)
<center><u>Change size to 50 gigs</u></center>

v. *CPU Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-CPU-2-cores-1-socket.png)
<center><u>Change size to 2 cores 1 socket</u></center>

vi. *Memory Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Memory-3GiB.png)
<center><u>Change Memory to 3 GiB</u></center>

vii. *Network Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Network-unchanged.png)
<div align="center"><u>Network Tab remains unchanged</u></div>

viii. *Confirm Tab*

![()](https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Confirm-Finished.png)
<center><u>Confirmation Tab remains unchanged</u></center>




### 5. Run pfSense setup Wizard

Start the VM and follow the prompts as directed. The end result should be pfSense interfaces configured as follows:

- WAN --> vtnet0 --> 10.0.0.2/30
- LAN --> vtnet1 --> 192.168.1.2/24
- OPT1 --> vtnet2 --> (blank - no IP addresses defined.)


### 6. Configure pfsense





