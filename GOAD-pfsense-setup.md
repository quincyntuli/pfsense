# Disclaimer

> [!NOTE]
> These notes are ~~strongly inspired~~ <strong><ins>copied</ins></strong> (with minor adjustments) from [Mayfly's notes](https://mayfly277.github.io/posts/GOAD-on-proxmox-part1-install/#prepare-for-pfsense) as part of his installation notes for Game of Active Directory Proxmox setup. 
> 
> I have added a few steps for my own understanding as some of the details were not immediately apparent to me when I followed his notes.
> 
> I strongly recommend that you stick to his notes as mine are merely interpretation. I could be leading you astray.


It is important to mention that my setup is set in a home Lan

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


## 4. Setup pfSense VM

i. *General Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-General-Name.png'><br><ins>Create VM - General</ins></div>

ii. *OS Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Select-ISO.png' width="90%"><br><ins>Select uploaded .iso</ins></div>


iii. *System Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-System-Nothing-Changed.png'  width="90%"><br><ins>No changes on System Tab</ins></div>

iv. *Disks Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Disks-Change-Disc-Size.png'><br><ins>Change size to 50 gigs</ins></div>

v. *CPU Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-CPU-2-cores-1-socket.png'  width="90%"><br><ins>Change size to 2 cores 1 socket</ins></div>


vi. *Memory Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Memory-3GiB.png'  width="90%"><br><ins>Change Memory to 3 GiB</ins></div>

vii. *Network Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Network-No-Network-Interface.png'><br><ins>Network Tab - no network device</ins></div>

viii. *Confirm Tab*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Confirm-Finished.png'><br><ins>Confirmation Tab remains unchanged</ins></div>

## 5. Add (virtual) network devices

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-Network-Device-initial-01.png'><br><ins>Add Virtual Network Device</ins></div>

 <br>
Add virtual network devices *(bridges)* 3 times until all 3 are added (vmbr1, vmbr2, vmbr3)
<br>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-Network-Device-initial-02.png'><br><ins>Add Virtual Network Bridges (vmbr1, vmbr2, vmbr3)</ins></div>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-Network-Device-initial-04.png'><br><ins>After Virtual Network Bridges are added</ins></div>

## 6. Run pfSense setup Wizard

### 6.1 Copyright and Trademark Notices.

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Eula.png'><br><ins>Accept EULA</ins></div>

### 6.2 Welcome to pfSense.

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Welcome.png'><br><ins>Accept EULA</ins></div>

### 6.3 Disk partitioning.

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Partition.png'><br><ins>Disk Partitioning</ins></div>

### 6.4 Configure Options.

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-Configure-ZFS-proceed.png'><br><ins>Configure Options</ins></div>

### 6.5 Select Virtual Device type.

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-ZFS-Stripe-No-Redundancypng.png'><br><ins>Select Virtual Device type</ins></div>

### 6.6 Select Hard Disk

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-ZFS-Qemu-Hard-Disk.png'><br><ins>Select Hard Disk</ins></div>

### 6.7 Confirm disk partitioning

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Add-ZFS-Qemu-Hard-Disk-Confirm.png'><br><ins>Confirm disk partitioning</ins></div>

### 6.8 Install Wizard Executing

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Install-Progress.png'><br><ins>Install Wizard Executing</ins></div>

### 6.9 Install Wizard Reboot

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-Install-Reboot.png'><br><ins>Install Wizard Reboot</ins></div>

### 6.10 VLAN setup

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Initial-Reboot-VLAN-question.png'><br><ins>VLAN setup</ins></div>


### 6.11 Setup pfSense interfaces


<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-01.png'><br><ins>WAN Interface Name</ins></div>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-02.png'><br><ins>LAN Interface Name</ins></div>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-03.png'><br><ins>Option 1 Interface Name</ins></div>

### 6.12 Setup WAN interface address


i. *WAN IP not present*

<div align="center" ><img src='https://raw.githubusercontent.com/quincyntuli/pfsense/main/img/2024-04-01%2013_06_40-pFsense-install-Wizad-WAN-interface-Name-0511.png'><br><ins>WAN Interface IP not set</ins></div>

ii. *Select interface IP address*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-06.png'><br><ins>Select interface IP address</ins></div>

iii. *Define WAN IP address*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-08.png'><br><ins>Define WAN IP address</ins></div>

iv. *Confirmation WAN IP address set*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-091.png'><br><ins>Confirmation WAN IP address set - 01</ins></div>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-WAN-interface-Name-101.png'><br><ins>Confirmation WAN IP address set - 02</ins></div>



### 6.13 Change LAN interface IP

i. *Select Interface IP to change*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-LAN-interface-IP-01.png'><br><ins>Set Interface IP address 01</ins></div>

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-LAN-interface-IP-0.png'><br><ins>Set Interface IP address 02</ins></div>


ii. *Define LAN IP Address settings*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-LAN-interface-IP-02.png'><br><ins>Define LAN IP Address settings</ins></div>

iii. *Define DHCP IP Range*

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-LAN-interface-IP-03.png'><br><ins>Define DHCP IP Range</ins></div>


iv. LAN IP change confirmed

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-install-Wizad-LAN-interface-IP-04.png'><br><ins>LAN IP change confirmed</ins></div>



## 7. Configure pfSense


> [!NOTE]
> These notes are apply to  a home LAN use case. For that the **'public address'** in this case would be the home router address (192.168.2.254).

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-configure-09-LAN.png'><br><ins>Home LAN</ins></div>

pfSense admin interface accepts connections from localhost by default. This requires connections to emanate from 'Notebook' through 'Proxmox' to destination pfSense. That can be achieve through tunnelling

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-configure-11-Tunneling.png'><br><ins>Tunnelling</ins></div>


### 7.1 Create Tunnel to port 80

```bash
ssh -L 8082:192.168.1.2:80 root@192.168.2.106
```

<div align="center" ><img src='https://github.com/quincyntuli/pfsense/raw/main/img/pFsense-configure-10-Tunneling-Command3.png'><br><ins>Tunnelling Command</ins></div>



This allows us to connect to the pfSense admin interface through http://localhost:8082.

