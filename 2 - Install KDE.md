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
