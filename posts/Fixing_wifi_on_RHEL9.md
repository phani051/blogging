---
title: "Fixing wifi on the RHEL installed on MAcbook"
subtitle: "Install Broadcom wifi drivers on the RHEL 9 installed on the Macbook "
date: "2024-05-15"
---

In the last article we have seen how to install RHEL 9 on the old Macbook. But there is very high chance that wifi will not be working and as we know there is no Ethernet port, so we need to install wifi drivers for internet access.

## Getting Started

> To proceed with the wifi drivers installation, we need internet to the Macbook.

### Internet to Macbook with USB tethering

* To get the required packages of drivers, we need internet to the Macbook.
* USB tethering is the option we can use to connect mobile internet to RHEL as there is no wifi and Ethernet port.
* Once internet is connected to Macbook, run below commands in the terminal.

> Run below commands in terminal.

```
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm

sudo rpm -Uvh https://download1.rpmfusion.org/free/el/rpmfusion-free-release-9.noarch.rpm

sudo rpm -Uvh https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-9.noarch.rpm

sudo yum update
sudo yum install akmod-wl
akmods
```
> Reboot once above commands complete.

Once rebooted, you will be able to see the wifi adapter workng and can see available wifi networks.

#### This concludes fixing wifi drivers issue on RHEL 9 Macbook Air 🎉



