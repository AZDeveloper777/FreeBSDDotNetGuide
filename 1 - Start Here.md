This guide assumes you have FreeBSD 14.1-Release already installed on physical hardware.
See VMWareWorkstation.md for instructions on having FreeBSD as a guest in VMWare Workstation.
You need to switch to root user by using "su" before proceeding.

1. Install pkg:  
```
pkg
```
3. Install nano:  
```
pkg install nano  
```
4. Open the pkg config:  
```
nano /etc/pkg/FreeBSD.conf
```
5. Change it to look like this:  
```
FreeBSD: {  
  url: "pkg+http://pkg.FreeBSD.org/${ABI}/latest",  
  mirror_type: "srv",  
  signature_type: "fingerprints",  
  fingerprints: "/usr/share/keys/pkg",  
  enabled: yes  
}
```
6. Update pkg
```
pkg update
```
7. Upgrade
```
pkg upgrade
```

