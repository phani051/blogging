---
title: "Installing RHEL on Macbook"
subtitle: "Install Redhat Linux on old Macbook Air(2015) to use as home server."
date: "2024-05-11"
---

If there is a old Macbook laying around the house which you don't use anymore you can try installing RHEL 9 and use it as a home server. Having an home server opens up a lot of possibilities for learning and experimentation.

## Getting Started

Below are the requirments for instllation on RHEL on Macbook.

* Old Macbook Air (In my case Macbook Air 2015).
* 16 GB USB Stick.
* RHEL 9 ISO file.
* BalenaEtcher to make USB bootable.
* Working Windows or Macbook.

## Preparing Macbook for Instalaltion.

* Make sure no usefull data on the Macbook.
* Disk partition is tricky during Linux instalationa and below steps are mandatory(atleast as of now).
    * Boot the Mac into recovery mode Apple Key + R.
    * Use Disk Utility to erase the drive.
    * Used Terminal app to examine then remove all partitions.

        ```bash
            diskutil list
            diskutil list /dev/disk0
            diskutil eraseVolume free testpart disk0s2
            diskutil eraseVolume free EFI disk0s1
        ```



## Process to install RHEL on Macbook

* Make sure data on the Macbook is backedup as we all know this is all about expermentation....and 100% sure we will loose all the data.
* Download RHEL 9 iso file by creating account on Redhat Developer account which allow us to download RHEL at no cose.
* Make sure the downloaded RHEL iso file is around 10G(complete repositories) and not 1G(only boot).
* Download BalenaEtcher software and install on the working PC.
* Make sure there is no useful data on USB stick as it will be formated as part of flashing the ISO to USB.
* Flash the downloaded iso to USB stick using BalenaEtcher.
* Insert the flased USB in Macbook and power on holding Alt/Option key.
* You will be seeing an UFI icon and click and proceed.
* Linux installation setup will be starting and proceed accordingly.
* Set the root password and add additional user if required.
* Do the normal setup of network, packages, etc.
* Disk partition is tricky one on the Macbook and the steps mentioned in the pre-requestion is mandatory if not the partition cannot be done.
    * Enter disk partitioning.
    * Select your Disk.
    * At the bottom, click the "Full disk summary and boot loader" text.
        * Click on the disk in the list.
        * Click "Do not install boot loader".
        * It is very important to not install the boot loader, so don't miss that step to turn off that option (the blue link at the very bottom of the screen).
    * After clicking Storage Configuration "Custom" then the Done which brings the Manual Partitioning screen appeared, click "Click here to create them automatically" in the upper left of the screen. This will create the required partitions.
    * Adjust the size assigned to each mount point as required.
    * Create /foo mountpoint, 600M, Standard EFI partition, then edit the partition to be on /boot/efi --> Very important.
    * Done.
* During the install, there may be an error about the mactel package, just continue.
* Complete all of the other installation option settings and began the installation.
* At the end of the installation let the Mac reboot by clicking Reboot System. Select the first grub menu option (the default) and RHEL 9.4 came up.

 __Note__ : You might face wifi issue even after installation, checkout for next acrticle to fix wifi on RHEL on Macbook.

#### This concludes the RHEL installation on the Macbook Air 🎉

## Useful Links

* [RedHat Developer Signup](https://developers.redhat.com/about)
* [RHEL 9.4 x86_64 DVD ISO](https://developers.redhat.com/content-gateway/file/rhel/Red_Hat_Enterprise_Linux_9.4/rhel-9.4-x86_64-dvd.iso)
* [Download BalenaEtcher](https://etcher.balena.io/#download-etcher)

    