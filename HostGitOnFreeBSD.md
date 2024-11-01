PreRequisites:

Git
```
pkg install git
```

Load the FreeBSD ports locally
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
2. Make it
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

