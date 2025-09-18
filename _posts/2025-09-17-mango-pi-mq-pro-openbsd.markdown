---
layout: single
title: "OpenBSD on the mango pi mq pro"
date: 2025-09-17
categories: posts
classes: wide
tags:
  - sbc
  - tutorials
---

First tutorial (and post) of the site!!! hell yeah.

For some reason there is no tutorial on how to install OpenBSD on the
mango pi mq pro so i thought this might be of help for some people.

# Compiling OpenSBI and Uboot

I took everything from 
[linux sunxi](https://linux-sunxi.org/Allwinner_Nezha), triyng to format
it in a better way for everyone.

## Compiling OpenSBI

`git clone https://github.com/riscv-software-src/opensbi
cd opensbi
make CROSS_COMPILE=riscv64-linux-gnu- PLATFORM=generic FW_PIC=y`

## Compiling Uboot

`git clone https://github.com/smaeul/u-boot -b d1-wip
cd u-boot
make CROSS_COMPILE=riscv64-linux-gnu- mangopi_mq_pro_defconfig
make CROSS_COMPILE=riscv64-linux-gnu- OPENSBI=../opensbi/build/platform/generic/firmware/fw_dynamic.bin` 

# Get OpenBSD

We have our uboot ready, time to obtain an image of OpenBSD for riscv64, 
[here](https://www.openbsd.org/faq/faq4.html#Download). 

Now flash the file into a micro sd card and a flash drive too (this will
make sense later).

#installing

Having our sd card flashed, we have to run this command, to add our fresh
uboot to our OpenBSD installer.
`dd if=u-boot-sunxi-with-spl.bin of=${card} bs=1024 seek=8`

We have everything ready now. Time to plug the sd card into the mango pi
and connect a serial adapter to start the installation.

Now follow the steps prompted by the installer, and when it asks for the
sets here is when our usb flash drive comes into play. For some reason
the system cannot use the sets contained in the sd card, and thanks to
this board having no ethernet port we have to use a flash drive to install
them. Pick disk when it asks you about the sets location and mount the usb
drive, then continue with the installation as normal.

If everything went right then we will have successfully installed OpenBSD
on our mango pi mq pro!

# Post Install

By the time of writting this post there is no driver for the onboard wifi
module, you must use an usb to ethernet or wifi adapter. 

The libraries rebuild at bootime, and it takes some time to complete on
this little machine, so it's better to disable it with this command:
`rcctl disable library_aslr`
