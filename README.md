# AstraVirt VE — Installers
### Enterprise Virtualisation Platform by Aelius Enterprises

> **Contact:** sales@aelius.biz | info@aelius.biz
> **Website:** [www.aelius.biz](https://www.aelius.biz)
> **(c) 2026 Aelius Enterprises. All rights reserved.**

This repository hosts the official AstraVirt VE installers. Downloads are on the **[Releases page](https://github.com/pa1more/astravirt-releases/releases)**.

**Current version: 26.0.26** ([release notes](https://github.com/pa1more/astravirt-releases/releases/tag/v26.0.26))

---

## Contents

1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Before You Begin](#before-you-begin)
4. [Download](#download)
5. [Installation](#installation)
6. [First Login](#first-login)
7. [Creating Your First Virtual Machine](#creating-your-first-virtual-machine)
8. [Uploading ISO Images](#uploading-iso-images)
9. [Licence Management](#licence-management)
10. [Troubleshooting](#troubleshooting)
11. [Support](#support)

---

## Overview

AstraVirt VE turns an Ubuntu server into a virtualisation host. You create, run and monitor virtual machines from a secure web-based management console.

**Key features:**
- Browser-based management console, with no client software needed
- Full virtual machine lifecycle: create, start, stop, delete
- VM console access in the browser
- ISO-based operating system installation
- Built-in network management
- Hardware-bound licence enforcement with Ed25519-signed licence tokens

---

## System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **OS** | Ubuntu 24.04 LTS (Noble) 64-bit with a desktop session | Ubuntu 24.04 LTS (Noble) 64-bit |
| **CPU** | 4 cores with VT-x/AMD-V enabled | 8+ cores |
| **RAM** | 8 GB | 32 GB |
| **Disk** | 100 GB | 500 GB SSD |
| **Network** | 1 Gbps NIC | 1 Gbps NIC |
| **Internet** | Required for installation and licence activation | Outbound HTTPS (port 443) at least once every 7 days |

> **Important:** enable hardware virtualisation (Intel VT-x or AMD-V) in the server BIOS/UEFI before installing.
>
> The installer is a graphical wizard. Run it from the server's desktop session, either on the local screen or through remote desktop.

---

## Before You Begin

### 1. Check hardware virtualisation

```bash
grep -cE '(vmx|svm)' /proc/cpuinfo
```

If the output is `0`, enable VT-x or AMD-V in the BIOS and reboot.

### 2. Update the system

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

### 3. Get your activation key

You need a valid AstraVirt activation key. To get one, contact Aelius Enterprises:

- **Email:** sales@aelius.biz
- **Website:** [www.aelius.biz](https://www.aelius.biz)

The key looks like this: `ASTRA-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX`

---

## Download

Choose **one** installer:

| Installer | Best for |
|-----------|----------|
| **Debian package**: [`astravirt-ve_26.0.26_amd64.deb`](https://github.com/pa1more/astravirt-releases/releases/download/v26.0.26/astravirt-ve_26.0.26_amd64.deb) | Installing from the Ubuntu desktop. The setup wizard opens automatically. |
| **Self-extracting installer**: [`astravirt-gui-setup-26.0.26.run`](https://github.com/pa1more/astravirt-releases/releases/download/v26.0.26/astravirt-gui-setup-26.0.26.run) | Running the wizard directly from a terminal on the desktop |

Download from a terminal:

```bash
wget https://github.com/pa1more/astravirt-releases/releases/download/v26.0.26/astravirt-ve_26.0.26_amd64.deb
# or
wget https://github.com/pa1more/astravirt-releases/releases/download/v26.0.26/astravirt-gui-setup-26.0.26.run
```

### Verify the download

Download [`SHA256SUMS`](SHA256SUMS) from this repository into the same folder, then run:

```bash
sha256sum --ignore-missing -c SHA256SUMS
```

The file you downloaded should be reported as `OK`. Do not install a file that fails this check.

---

## Installation

### Option A: Debian package

```bash
sudo apt install ./astravirt-ve_26.0.26_amd64.deb
```

The setup wizard opens on your desktop. If it doesn't, open **AstraVirt VE Installer** from the applications menu, or run:

```bash
sudo bash /usr/share/astravirt-ve/gui-setup.sh
```

### Option B: Self-extracting installer

```bash
sudo bash astravirt-gui-setup-26.0.26.run
```

### The setup wizard

1. **Welcome:** click **Begin Setup**.
2. **Licence activation:** enter your activation key. The wizard activates it online and binds it to this server's hardware. You'll see your plan, expiry date and VM limit.
3. **Administrator account:** choose the console username (default `astraadmin`) and a password of at least 8 characters. Keep these credentials safe: this account has full control of the platform.
4. **Hostname:** for example `astravirt-prod-01`. Use only letters, numbers and hyphens.
5. **Domain (optional):** for example `virt.yourcompany.com`. Leave it blank to use the IP address.
6. **VM storage:** the folder where virtual machine disks are kept. The default is `/home/<admin>/AstraVirt`. ISO images go in its `iso` subfolder.
7. **Confirm:** review the summary and click **Yes, Install**.

Installation then runs automatically and takes about 10–15 minutes, depending on your internet speed. It:
- prepares the system and installs the virtualisation engine and management console;
- configures virtualisation, storage and the web gateway;
- enables licence enforcement;
- installs the **AstraVirt VE** desktop launcher.

When it finishes, the wizard shows your access details and opens the management console.

---

## First Login

1. Open a web browser on any device on your network.
2. Go to `http://YOUR-SERVER-IP` (or your configured domain). On the server itself, use the **AstraVirt VE** desktop icon.
3. Log in with the administrator username and password you set during installation.

> **Security:** the console is served over plain HTTP. Use it only on a trusted network, and don't expose ports 80 or 9090 to the internet.

---

## Creating Your First Virtual Machine

1. Log in to the management console.
2. Click **Virtual Machines** in the left sidebar.
3. Click **Create VM**.
4. Fill in the VM details:
   - **Name**: a descriptive name, for example `ubuntu-server-01`
   - **Memory**: RAM allocation, for example 2 GB
   - **vCPUs**: number of virtual CPU cores
   - **Disk size**: storage in GB
5. Under **Installation type**, choose **Local install media** and select an ISO file.
6. Click **Create and run** to start the operating system installation.
7. Click **Console** to work with the VM directly in the browser.

> **Before creating VMs**, upload at least one OS ISO image. See the next section.

---

## Uploading ISO Images

Copy ISO images into the `iso` folder inside the VM storage location you chose during setup. With the defaults, that's `/home/astraadmin/AstraVirt/iso/`.

**From another computer (SCP/SFTP):**

```bash
scp ubuntu-24.04-live-server-amd64.iso astraadmin@YOUR-SERVER-IP:~/AstraVirt/iso/
```

**On the server:**

```bash
wget -P ~/AstraVirt/iso/ \
  https://releases.ubuntu.com/24.04/ubuntu-24.04-live-server-amd64.iso
```

---

## Licence Management

### Checking licence status

```bash
sudo astravirt-licence status
```

This shows your plan, expiry date, VM limit and the date by which the server must next reach the licence server.

### Renewing your licence

Contact sales@aelius.biz. A renewal takes effect at the next automatic check-in, within 6 hours. To apply it immediately:

```bash
sudo systemctl start astravirt-licence.service
```

### How licensing works

- **Activation:** your key is bound to this server's hardware (machine ID and system board UUID). Each key activates a limited number of servers.
- **Signed licence:** the licence server issues a cryptographically signed licence token, which AstraVirt verifies locally. A licence cannot be forged or edited, and a token copied from another server is rejected.
- **Check-in:** the server re-validates its licence online every 6 hours. If the licence server is unreachable, it keeps working offline for up to 7 days.
- **Enforcement:** a virtual machine cannot be started when the licence is expired, suspended, revoked or past its offline deadline, or when starting it would exceed your plan's VM limit. VMs that are already running are not stopped.
- **Clock protection:** setting the system clock back does not extend a licence.

### Moving to new hardware

Release the licence on the old server first:

```bash
sudo astravirt-licence deactivate
```

Then install on the new server with the same key. If the old server is no longer available, contact sales@aelius.biz to free the activation.

---

## Troubleshooting

### The setup wizard doesn't open

The wizard needs a graphical desktop session. Log in to the server's desktop, open a terminal and run:

```bash
sudo bash /usr/share/astravirt-ve/gui-setup.sh
```

### Licence activation fails

| Message | What to do |
|---------|------------|
| Cannot reach the licence server | Check internet access and that outbound HTTPS (port 443) is allowed. |
| Licence key not found | Check the key for typos. |
| Activation limit reached | Deactivate an old server, or contact sales@aelius.biz. |
| Too many failed attempts | Wait a few minutes before trying again. |

### Cannot access the management console

1. Check the console service: `systemctl status cockpit.socket`
2. Check the web gateway: `systemctl status nginx`
3. Check the firewall: `sudo ufw status`

### A virtual machine won't start

1. Check that hardware virtualisation is available: `kvm-ok`
2. Check the virtualisation service: `systemctl status libvirtd`
3. Check your licence: `sudo astravirt-licence status`. The error shown when starting the VM explains the reason, for example "VM limit reached" or "licence expired".

### Installation log

```bash
sudo cat /var/log/astravirt-install.log
```

---

## Support

| Channel | Contact |
|---------|---------|
| **Sales** | sales@aelius.biz |
| **Support** | info@aelius.biz |
| **Website** | [www.aelius.biz](https://www.aelius.biz) |

---

*AstraVirt VE is proprietary software. (c) 2026 Aelius Enterprises. All rights reserved.*
