This guide assumes you have FreeBSD 14.1-RELEASE or 14.2-RELEASE already installed.
I tested these instructions on 14.2-RELEASE on Dec 7th, 2024. Previous versions of this guide were tested on 14.1-RELEASE.
This guide should still work just fine on 14.1-RELEASE.
If you are running FreeBSD as a guest in VMWare Workstation, the specific steps to make that work are now inlined in Step 2.
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

