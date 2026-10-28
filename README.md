# Week 1: Cybersecurity Testing Lab Setup

**Intern:** Erica Gyamfuaa Fordjour 
**Program:** Cybersecurity & Ethical Hacking Internship, NetworkWalks
**Date completed:** 25 October 2026

## Objective

Build a cybersecurity testing lab on a personal laptop using VirtualBox, with Kali Linux as the attacking/hacker machine.

## Requirements

- VirtualBox as the base (latest version)
- Kali Linux as the attacker/hacker machine
- NAT Network on subnet 10.0.0.0/24
- Kali IP address: 10.0.0.2/24
- Full internet access from Kali

## Lab Environment

| Item | Detail |
|------|--------|
| Host OS | Windows 11 |
| Archive tool | 7-Zip 26.05 |
| Hypervisor | VirtualBox 7.2.22 r175245 (Qt6.8.0) |
| VM | Kali Linux 2026.2 (VirtualBox amd64) |
| Network | NatNetwork, 10.0.0.0/24 |
| Kali IP | 10.0.0.2/24 |
| Gateway | 10.0.0.1 |
| DNS | 8.8.8.8 |
| Interface | eth0 |
| Connection name | Wired connection 1 |

## Network Diagram

```
Internet
   |
Host PC (Windows 11)
   |
VirtualBox NatNetwork (10.0.0.0/24)
   |  Gateway: 10.0.0.1
   |
Kali Linux (10.0.0.2/24)
```

## Steps Performed

### Step 1: Install 7-Zip

1. Opened https://7-zip.org/download.html
2. Downloaded the 64-bit Windows x64 installer.
3. Installed 7-Zip 26.05.


> Screenshot: 7-Zip File Manager open
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/7-Zip.png)


### Step 2: Install VirtualBox

1. Opened https://virtualbox.org/wiki/Downloads
2. Downloaded the Windows hosts installer.
3. Installed with default settings.
4. Found an older version (7.2.8). Uninstalled it and installed the latest, **7.2.22 r175245**.

> Screenshot: Help > About VirtualBox showing 7.2.22
![Alt Text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/virtualBox%20installed.png)


### Step 3: Create the NAT Network

1. Opened **File > Preferences** and switched to **Expert** mode (see Troubleshooting).
2. Opened the **Network** tab, then **NAT Networks**.
3. Clicked **Create**.
4. Set the name to `NatNetwork` and the IPv4 prefix to `10.0.0.0/24`.
5. Clicked **Apply**.

> Screenshot:  Network settings with 10.0.0.0/24 and Finished network in the list
![Alt Text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/NatNetwork.png)


### Step 4: Download and Import Kali Linux

1. Opened https://www.kali.org/get-kali/#kali-virtual-machines
2. Downloaded the VirtualBox 64-bit image (`kali-linux-2026.2-virtualbox-amd64`).
3. Extracted the `.7z` archive with 7-Zip.
4. In VirtualBox, clicked **Machine > Add** and selected the `.vbox` file.
5. Kali appeared in the sidebar with no errors.

> Screenshot: Kali in the VirtualBox sidebar
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/kali%20linux.png)


### Step 5: Configure VM Settings and IP Address

**VirtualBox settings (VM powered off)**

| Setting | Value |
|---------|-------|
| General > Features > Shared Clipboard | Bidirectional |
| General > Features > Drag'n'Drop | Bidirectional |
| Network > Adapter 1 > Attached to | NAT Network |
| Network > Adapter 1 > Name | NatNetwork |
| Shared Folder path | Host Downloads folder |
| Shared Folder name | downloads |
| Shared Folder options | Auto-mount, Make Global |

> Screenshot: General > Features > Shared Clipboard | Bidirectional
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/general%20featutres%20Bidirectional.png)

> Screenshot: Network > Adapter 1
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/kali%20NatNetwork.png)

> Screenshot: Shared Folders window
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/shared%20folder.png)


**Static IP inside Kali**

Checked the interface and connection names:

```
ip a
nmcli connection show
```

Set the static IP:

```
ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns 8.8.8.8
```

Applied the change:

```
sudo nmcli connection up "Wired connection 1"
sudo nmcli device connect eth0
```

Verified with `ip a`. Result: `eth0` has **10.0.0.2/24**.

> Screenshot: set static IP and DNS
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/DNS.png)

> Screenshot: nmcli connection show and ip a showing 10.0.0.2/24
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/error%20solved.png)



### Step 6: Test Connectivity and Take a Snapshot

**Connectivity tests**

```
ping -c 4 10.0.0.1
ping -c 4 8.8.8.8
ping -c 4 google.com
```

All three returned 0% packet loss: gateway, internet by IP, and DNS.

> Screenshot: Ping results
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/ping%20worked.png)

> Screenshot: Searched for Youtube on google
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/google%20works.png)



**Snapshot**

- Name: `My-Kali-First-Config`
- Description: `Kali 2026.2, static IP 10.0.0.2/24 on NatNetwork, internet also working`

> Screenshot: Snapshots tab showing the snapshot
![alt text](https://github.com/myzz-rica/cybersecurity-lab-setup1/blob/main/Images/snapshot.png)


## Troubleshooting

### 1. Network Manager not found in VirtualBox 7.2.22

- **Issue:** The Network Manager was not under File > Tools.
- **Fix:** Go to **File > Preferences** and switch from **Basic** to **Expert** mode. Extra tabs appear, including **Network**.

### 2. "Wired connection 1" is not an active connection

- **Issue:** `sudo nmcli connection down "Wired connection 1"` returned "not an active connection".
- **Fix:** Skip the `down` command. Run `sudo nmcli connection up "Wired connection 1"`, then `sudo nmcli device connect eth0`. Verify with `ip a`.

## Result

| Requirement | Status |
|-------------|--------|
| VirtualBox (latest version) | Done |
| Kali Linux as attacker machine | Done |
| NatNetwork 10.0.0.0/24 | Done |
| Kali IP 10.0.0.2/24 | Done, verified with `ip a` |
| Full internet access | Done, verified with ping |
| Clipboard and drag and drop enabled | Enabled in VM settings |
| Shared folder (Downloads) | Added in VM settings |
| Snapshot | Done |

