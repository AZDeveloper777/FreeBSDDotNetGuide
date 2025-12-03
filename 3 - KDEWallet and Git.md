Verified working on FreeBSD 15.0-RELEASE on 12/2/2025.  
Verified working on FreeBSD 14.3-RELEASE on 6/17/2025.

Naturally, you'll need Git:
```
pkg install git
```
You will need to setup a KDE Wallet for GitHub interoperability  
1. Install gnupg
``` 
pkg install gnupg
```
2. Generate key with defaults
```
gpg --full-generate-key 
```
3. This verifies GPG agent is running
```
gpg-connect-agent /bye
```   
4. Sets your GPG TTY environment variable
```
export GPG_TTY=$(tty)  
```

