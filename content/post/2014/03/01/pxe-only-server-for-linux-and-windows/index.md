---
categories:
- Off-topic
date: "2014-03-01T17:07:23+01:00"
title: PXE only server for Linux and Windows
resources:
- name: screenshot1
  src: ./Screenshot-from-2014-03-01-131607.png
---

Windows has broken once more my computer, and I have the problem that I don't have any CD/DVD reader on the computer.

I don't want either to install from USB, thought this is the way I have done before,a so I have decided that I will try to get once more over the PXE setups.

I must say I am doing it now, so expect this to need fixups!

## State of art and ideas

I want to install windows from the network, so I have decided to do it with PXE and a Linux server (don't have any windows available, nor want it). The machine that will hold the setup will be a RPi, so the server software must be the littlest possible.

On my experience on Fon, I have learnt that dnsmasq is quite powerful, without the need of configuring dhcpd (which I have already used before), and much more light, so I first searched for pxe only with dnsmasq, and found a mail[1] of the dnsmasq user list.

Also, I wanted to have windows booted, so I searched and found superuser answer with the steps[2]. I don't want (or I will try not to at least) to create a samba share to put windows files, and will try to do it by tftp or ftp if possible. I also found a resource that will be useful for setting up tftp's pxe files correctly.

## Analyzing dhcp requests

I first wanted to be sure that I couldn't do what I expected with the dhcp protocol, so I run wireshark, and made a mini-fixup with wireshark for allowing my normal user capture packets. At the end, after reading that horrible readme, I noticed the command I had to run[3]:

```
I./b. Installing dumpcap and allowing non-root users to capture packets

   Members of the wireshark group will be able to capture packets on network
   interfaces. This is the preferred way of installation if Wireshark/Tshark
   will be used for capturing and displaying packets at the same time, since
   that way only the dumpcap process has to be run with elevated privileges
   thanks to the privilege separation[1].

   Note that no user will be added to group wireshark automatically, the
   system administrator has to add them manually.

   The additional privileges are provided using the Linux Capabilities
   system where it is available and resort to setting the set-user-id bit
   of the dumpcap binary as a fall-back, where the Linux Capabilities system
   is not present (Debian GNU/kFreeBSD, Debian GNU/Hurd).

   Linux kernels provided by Debian support Linux Capabilities, but custom
   built kernels may lack this support. If the support for Linux
   Capabilities is not present at the time of installing wireshark-common
   package, the installer will fall back to set the set-user-id bit to
   allow non-root users to capture packets.

   If installation succeeds with using Linux Capabilities, non-root users
   will not be able to capture packets while running kernels not supporting
   Linux Capabilities.

   Note that capturing USB packets is not enabled for non-root users by using
   Linux Capabilities. You have to capture the packets using the method
   described in I./a., setting the set-user-id permanently using
   dpkg-statoverride or running Wireshark as root.

The installation method can be changed any time by running:
dpkg-reconfigure wireshark-common
```

Do you see it? yeah, at the end, you just have to do the following:

```
sudo dpkg-reconfigure wireshark-common
sudo adduser <user> wireshark
```

Now we must relogin (if not, user group changes wont apply), and we can analyze DHCP requests. I must say I have put the Realtek PXE lan boot that my BIOS offers. I will do the same analisys with the Legacy Boot later.

{{< figure src="dhcp-discover-attributes.png" title="DHCP Discover Attributes" width="100%" >}}

We will analyze the packet and find somethins like the following:

{{< figure src="dhcp-discover-request.png" title="DHCP Discover Request" width="100%" >}}

There you can see how there are tons of requests for PXE, which is the one we want to answer to, avoiding the ones like the following:

{{< figure src="dhcp-discover-request-bytes.png" title="DHCP Discover Request Bytes" width="100%" >}}

And now, I want to analyze the Legacy Network Boot that my BIOS offers:

{{< figure src="dhcp-discover-request-pxe-list.png" title="DHCP Discover Request PXE List" width="100%" >}}

UOoo it seems like this is not much "Legacy Boot Lan"! Anyway, it seems enough for dhcp booting.

## Install PXE service (dnsmasq with proxy only config)

So that was it. Now I could install dnsmasq with:

> sudo apt-get install dnsmasq

And configure it as stated in the email thread:

> pxe-service=x86PC,"PXE Netboot",pxe/pxelinux
> dhcp-range=192.168.1.0,proxy
> tftp-root=/var/tftp
> enable-tftp

Restart dnsmasq with:

> sudo service dnsmasq restart

An we are ready to setup the tftp files.

## TFTP Setup for debian

So we have to put in /var/tftp all the stuff we want to get by there. I have been wondering for a pair of hours why didn't boot from the pxelinux, and just realized that I had missing enable-tftp line in the config file.

[1]: Dnsmasq discuss about pxe only server: [http://lists.thekelleys.org.uk/pipermail/dnsmasq-discuss/2010q1/003857.html](http://lists.thekelleys.org.uk/pipermail/dnsmasq-discuss/2010q1/003857.html)

[2]: Superuser windows PXE: [http://superuser.com/questions/42263/how-to-install-windows-7-from-the-network](http://superuser.com/questions/42263/how-to-install-windows-7-from-the-network)

[3]: Windows PXE tftp: [http://www.howtogeek.com/162070/it-geek-how-to-network-boot-pxe-the-winpe-recovery-disk-with-pxelinux-v5-wimboot/

[4]: (http://www.howtogeek.com/162070/it-geek-how-to-network-boot-pxe-the-winpe-recovery-disk-with-pxelinux-v5-wimboot/) Debian readme for wireshark: [file:///usr/share/doc/wireshark-common/README.Debian](file:///usr/share/doc/wireshark-common/README.Debian) or [http://anonscm.debian.org/viewvc/collab-maint/ext-maint/wireshark/trunk/debian/README.Debian?view=markup](http://anonscm.debian.org/viewvc/collab-maint/ext-maint/wireshark/trunk/debian/README.Debian?view=markup)
