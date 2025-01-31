TESTED Dec 12th, 2024 on FreeBSD 14.2-RELEASE

SECURITY THOUGHTS:
Keep in mind I'm new to FreeBSD, so there might be security considerations that I'm missing in how this is setup.  Namely, no HTTPS and the reliance on user/pass for repo authentication.    I basically did this for use BEHIND my firewall/router on my private LAN.  I'd think twice about hosting this config on the public internet.  

PreRequisites:

Git
```
pkg install git
```

Note:  I've detailed how to install from ports.  You can also install from pkg i.e.
```
pkg install forgejo
```
if you install from pkg, skip to #3.

Load the FreeBSD ports locally  
NOTE: If you loaded ports during your FreeBSD install, this command won't work.  I'll update this at some point with what to do if you are in that situation. (probably just blow away /usr/ports and then run the command, but I want to test before I say for sure)
```
git clone https://git.freebsd.org/ports.git /usr/ports
```

If you've already got Ports locally, make sure it is up to date:
```
cd /usr/ports
git pull
```

Install ForgeJo

1. Cd to the ForgeJo directory
```
cd /usr/ports/www/forgejo
```
2. Make it (expect this step to take 15 min on a 4 core , 16GB RAM VM on modern hardware)
```
make install clean
```
3. Handle file ownership
```
chown -R git:git /usr/local/etc/forgejo
chown -R git:git /var/db/forgejo
chown -R git:git /usr/local/share/forgejo
```

4. Create ForgeJo user
```
pw useradd -n forgejo -s /usr/sbin/nologin -d /nonexistent -w yes
```
5. Edit ForgeJo app.ini
```
nano /usr/local/etc/forgejo/conf/app.ini
```
Add these entries under [server]:  
```
HTTP_ADDR = 192.168.1.183 ; Put in your machine's IP
SSH_ROOT_PATH = /usr/local/git/.ssh
```
Add this entire block:
```
[ssh]
AUTHORIZED_KEYS_PATH = /usr/local/git/.ssh/authorized_keys
```

6. Initialize the database
```
su -m git -c 'forgejo migrate'
```
7. Authorized Keys
```
mkdir -p /usr/local/git/.ssh
chown git:git /usr/local/git/.ssh
chmod 700 /usr/local/git/.ssh
```
```
touch /usr/local/git/.ssh/authorized_keys
chown git:git /usr/local/git/.ssh/authorized_keys
chmod 600 /usr/local/git/.ssh/authorized_keys
```
8. Set ForgeJo to autostart
```
sysrc forgejo_enable="YES"
```
9. Start it 
```
service forgejo start
```
10. Run the "doctor" check as git
```
su -m git -c 'forgejo doctor check'
```
The real test is to go to a different machine and point your web browser to http://<IP_ADDRESS>:3000 and see if you get the ForgeJo "welcome" screen. Then register an account and create a repo.   WARNING: I do not consider this secure enough of a config for the public internet.
