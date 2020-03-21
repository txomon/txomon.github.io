---
categories:
- Cuaderno de bitácora
- Hallazgos
- Tecnicismos
date: "2012-11-29T23:40:46+02:00"
title: How to use nvidia and nouveau in same debian/ubuntu and don't get nut
---
Buenas a los que sea que me leáis,

Estoy escribiendo esto para que le sirva como una referencia a cualquiera, lo traduciría a ingles... de hecho igual lo escribo en ingles.

I am going to explain how we can get nvidia driver and nouveau driver working in the same debian installation. I needed this because in the actual testing kernel (wheezy 3.2) my graphic card isn't supported, and the nvidia driver has a bug when trying to start maya.

Whatever you are here for, this is how:

We all start with running nouveau driver, if you have very bad luck like I had, your system after installation doesn't even boot. I switched to nomodeset run going to the grub menu entry (on boot) and adding in the linux line at the end (after quiet)

```
nomodeset
```

after that, I was able to boot. With that, I followed [http://wiki.debian.org/NvidiaGraphicsDrivers](http://wiki.debian.org/NvidiaGraphicsDrivers#Installation-1)  in the debian way installation. After that, and getting nut on how to create a xorg.conf file:

```
nvidia-xconfig -o xorg.conf
```

Yes, i know it was easy, but I started to read about a lot of solutions, and messed up. The idea is that after having the xorg.conf created, and in /etc/X11, I got it running.

I just rebooted and it worked.

Then, I found the bug that it didn't allow me to open maya files (SIGSEGV in their libGl1.so dll)

So I decided to go for a newer kernel (v3.6[1])  which also I had patched for work reasons, and compiled it. Well, you all must get aware that if you make a menuconfig, you **have to be aware of enabling graphic drivers**. I mean, go to graphic drivers, and remember to enable them:

```
Graphics support --->

<M> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  --->
<M>   Nouveau (nVidia) cards
[*]     Support for backlight control #optional
[*]     Build in Nouveau's debugfs support #optional but if you encounter errors, its easier like this to send bugs
{*} Support for frame buffer devices  --->
<M>   nVidia Framebuffer Support # I saw this and enabled it just in case
[*]     Enable DDC Support #same
[*]     Lots of debug output #same
[*]     Support for backlight control #same
```

I forgot to enable the first section (the real important in which you enable the driver nouveau) because I didn't think about the meaning of  Direct Rendering Manager, neither knew it was important, etc. So the case is I compiled and installed, and now I had the problem:

How to switch between one and the other? the installation following the Debian wiki didn't allow me to use nouveau if I didn't uninstall nvidia one. So I started to research about it, asked a lot in #debian #debian-next etc. And babilen told me that the only thing that said xorg to use nvidia, was that xorg.conf file.

So from there, the solution was simple, to switch from nvidia to nouveau:

```
mv /etc/X11/xorg.conf /etc/X11/xorg.conf.nvidia

# here comment all the lines and add a line that blacklists nvidia ( blacklist nvidia )
vim /etc/modprobe.d/nvidia-kernel-common.conf

update-alternatives --config glx #choose mesa
```

And now, you can reboot into nouveau. To change from nouveau to nvidia, mv the xorg.conf, unblacklist nvidia and blacklist nouveau (what you commented) and restore the other lines you commentes, and finally use update-alternatives command to use nvidia one. And reboot.

I hope it helps anyone, just to not keep installing/uninstalling the nvidia driver.

Cheers to anyone that really needs, and if you need help, I can help you, contact me on twitter (@txomon), or find my mail somewhere :D tip: <domain>@<domain>.com =) ]

[1]: if I say a kernel is v3.6, its from linus' repo, if I say it is 3.6, it is from linux-stable repo (they are different)
