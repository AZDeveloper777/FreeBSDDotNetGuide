This guide assumes you have FreeBSD 14.x-RELEASE  already installed.  
Verified working on FreeBSD 14.3-RELEASE on 6/17/2025, 14.2-RELEASE on Dec 7th, 2024. 
Previous versions of this guide were tested on 14.1-RELEASE.  

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

NOTE: I did NOT have to switch to "latest" to get dotnet working on 14.3-RELEASE  
If you are on 14.3-RELEASE you should be able to skip steps 4 and 5 !  

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

