Step 1 will show no output for a while and will take some time  
1.
```
pkg install --quiet --yes kde5 plasma5-sddm-kcm sddm xorg  
```
1a. (Only needed for VMWare Workstation VMs with UEFI at this point)  
```
pkg install open-vm-tools
pkg install xf86-video-vmware
sysrc vmware_guestd_enable="YES"
```
2.
```
sysrc dbus_enable="YES" && service dbus start
```
3.  
```
kldload fusefs 
sysrc sddm_enable="YES" && service sddm start  
```
After #3 you should see the KDE/SDDM login.  Restart the machine 


This will get you clipboard integration with the host, drag and drop and auto screen resizing.

What's missing:  Mouse integration.  You'll have to hit ctrl+alt to leave the VM.

Possible issues:
1. https://forums.freebsd.org/threads/cant-move-cursor.88634/
VMware support for Open VM Tools on FreeBSD (2149806)
Last Updated: 01/05/2020

Text copy & paste in UI as well as DND
are impacted by Xorg changes that do not allow
the vmmouse driver to be associated with the X11 screen.
