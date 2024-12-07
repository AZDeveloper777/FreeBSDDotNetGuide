(Now inlined with Step 2)
NOTE - You don't need to do this step unless you are running in a VMWare Workstation guest.

How to install open-vmware-tools and configure Xorg:
1. 
```
pkg install open-vm-tools
```
2. (as per output instructions from step 1)
```
kldload fusefs 
```
3.
```
pkg install xf86-video-vmware
```
4.
```
sysrc vmware_guestd_enable="YES"
```

This will get you clipboard integration with the host, drag and drop and auto screen resizing.

What's missing:  Mouse integration.  You'll have to hit ctrl+alt to leave the VM.

Possible issues:
1. https://forums.freebsd.org/threads/cant-move-cursor.88634/
VMware support for Open VM Tools on FreeBSD (2149806)
Last Updated: 01/05/2020

Text copy & paste in UI as well as DND
are impacted by Xorg changes that do not allow
the vmmouse driver to be associated with the X11 screen.
