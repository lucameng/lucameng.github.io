---
layout: post
title: Running Ubuntu Desktop on a 2GB Raspberry Pi 4
description: "To some extent, solve the problem of low memory raspberry PI."
modified: 2022-05-06
image:
  path: /images/abstract-10.jpg
  feature: abstract-10.jpg
---
<figure>
	<center><a href="/images/RPI.image"><img src="/images/RPI.image" alt=""></a>
	<figcaption>Raspberry Pi</a>.</figcaption></center>
</figure>

At Canonical we’re proud to be able to offer a [full Ubuntu Desktop experience on the Raspberry Pi 4](https://ubuntu.com/blog/build-a-raspberry-pi-desktop-with-an-ubuntu-heart). Ubuntu Desktop provides everything you need to develop software and even deploy it to Ubuntu Server on devices like the Raspberry Pi Zero 2 W.

However the full desktop environment is quite a lot for the Pi to handle.That's why users are suggested to models with either 4GB or 8GB of RAM to be confident that it will perform well. Ubuntu 22.04 LTS released few days ago, which can lower that barrier to entry, for RPI. This means targeting a viable Desktop experience on Raspberry Pi 4 2GB models.

The secret to this optimisation is a Linux kernel feature called zswap. In this blog, I’ll show you how to enable this functionality today and benefit from the upcoming performance boost that will come as standard in 22.04.

## What is ZSWAP?

To answer that we need to talk about swap files in general.

If you’re running any kind of Linux system, it’s highly likely (and recommended) that you have a swap file allocated on your hard drive or SD card. Swap files act as a kind of overflow for your RAM, caching pages that are rarely used to free up RAM for more active processes. This enables you to keep working even when your system is using almost all of your RAM.  However, using swap is less performant than using RAM since accessing your hard drive (or SD card) is slower.

> You can read more about swap in the [Ubuntu Documentation](https://help.ubuntu.com/community/SwapFaq?_ga=2.103617481.851146317.1651845015-343314546.1649091339).

Okay, so where does zswap come in? Zswap is essentially a compression tool. When a process is about to be moved to the swap file, zswap compresses it and checks whether the new, smaller size still needs to be moved or if it can stay in your RAM. It is much quicker to decompress a ‘zswapped’ page than it is to access the swap file so this is a great way of getting more bang for your buck from systems with smaller amounts of RAM.

## Sounds great, how do I enable it?

Since zswap is supported by default, you can enable it with a simple command.

Enter the following into your terminal:

```bash
$ sudo sed -i -e 's/$/ zswap.enabled=1/' /boot/firmware/cmdline.txt
```

For newer Linux users, this command is basically a shortcut to edit the cmdline.txt file in your boot folder and set the **zswap.enabled** parameter to ‘True’ (1).

Once you’ve done this you can restart your device and benefit from an increased performance boost!

## Going further

If you’re not a confident Linux user then you can stop there. The above should already improve performance on your existing 4GB or 8GB Raspberry Pi. But this won’t yet give the smoothest performance on a 2GB device.

For more advanced users, Dave Jones, who leads the Ubuntu Raspberry Pi work at Canonical, has a few additional improvements to share.  He’s written a more detailed blog post on how he configured things on his [personal blog](https://waldorf.waveform.org.uk/2021/6-months-with-the-pi-desktop.html), but I’ll paraphrase them below.

### Switching to z3fold & lz4

The two additional improvements we want to implement are:

- To increase the number of objects being compressed, using an allocator called **z3fold**.
- To use a different compression algorithm called lz4 that provides a better balance of speed and compression.

Enter the following command into your terminal:

```bash
$ sudo -i
```

This will prompt you for your password and put you into **root mode** where you can enter the following commands:

```bash
echo lz4 >> /etc/initramfs-tools/modules
echo z3fold >> /etc/initramfs-tools/modules
update-initramfs -u
```

This adds lz4 and z3fold modules to your initramfs so that they can be accessed on initialisation. Wait for the **update-initramfs** process to complete and then type:

```bash
exit
```

To return to your normal user mode.

Finally we need to add the following commands to your **cmdline.txt** file similar to before:

```bash
$ sudo sed -i -e 's/$/ zswap.compressor=lz4/' /boot/firmware/cmdline.txt
$ sudo sed -i -e 's/$/ zswap.zpool=z3fold/' /boot/firmware/cmdline.txt
```

Then reboot (you can just type **reboot** in the terminal).

You can check that the changes have been made correctly by searching for the parameters using grep:

```bash
$ grep -R . /sys/module/zswap/parameters
```

If you’ve configured things correctly then the output should look like this:

```bash
/sys/module/zswap/parameters/same_filled_pages_enabled:Y
/sys/module/zswap/parameters/enabled:Y
/sys/module/zswap/parameters/max_pool_percent:20
/sys/module/zswap/parameters/compressor:lz4
/sys/module/zswap/parameters/zpool:z3fold
#/sys/module/zswap/parameters/accept_threshold_percent:90
```
## A speed boost for Ubuntu Desktop on Raspberry Pi!

<figure>
	<center><a href="/images/RPII.jpg"><img src="/images/RPII.jpg" alt=""></a>
	<figcaption>Test on Ubuntu 20.04 mate</a>.</figcaption></center>
</figure>

More information, see [this](https://ubuntu.com//blog/how-low-can-you-go-running-ubuntu-desktop-on-a-2gb-raspberry-pi-4).

In future blogs, I will use this RPI to learn **ROS** and **SLAM** algorithms. See you later.