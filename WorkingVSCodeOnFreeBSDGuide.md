This worked for me on FreeBSD 14.1-RELEASE on 8/21/2024.

You'll need to change pkg to pull from "latest" instead of "quarterly"
1. nano /etc/pkg/FreeBSD.conf
2. replace "quarterly" with "latest"

FreeBSD: {

  url: "pkg+http://pkg.FreeBSD.org/${ABI}/latest",
  
  mirror_type: "srv",
  
  signature_type: "fingerprints",
  
  fingerprints: "/usr/share/keys/pkg",
  
  enabled: yes
  
}

3. pkg update
4. pkg upgrade
5. pkg install vscode

You should now have a working VSCode install.
Keep in mind that many of the VS Code Extensions that .Net developers rely on are not available for FreeBSD.
I'm working on porting them.
