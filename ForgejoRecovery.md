This assumes you are:
1. Using Forgejo with Sqlite3 db with defaults
2. You already have Forgejo installed on the new machine

On the old machine:
```
  cd /var/db/forgejo
  sudo -u git forgejo dump
  scp forgejo-dump-somenumbers.zip yourusername@newmachinehostnameorip:/home/yourusername/
  service forgejo stop
  scp forgejo.db yourusername@newmachinehostnameorip:/home/yourusername/
```

On the new machine as root (use su or doas):
1. Stop forgejo
   ```
   service forgejo stop
   ```
2. Copy the sqlite3 db file to the new install
   ```
   cp forgejo.db /var/db/forgejo 
   ```
3. Create the fdump folder and unzip the file into it
   ```
   mkdir fdump
   cd fdump
   unzip ../forgejo-dump-somenumbers.zip
   ```
4. Copy and set ownership for the Data folder
   ```
   cd ~/fdump/data
   cp -Rp . /var/db/forgejo/data/
   chown -R git:git /var/db/forgejo/data/
   ```
   
5. Copy and set ownership for the Repos folder
   ```
   cd ~/fdump/repos
   cp -Rp . /var/db/forgejo/forgejo-repositories/
   chown -R git:git /var/db/forgejo/forgejo-repositories/
   ```
6. Start the service back up
   ```
   service forgejo start
   ```
7. Run a doctor check and look for any errors. If you can't see any, go ahead and try to log in with a web browser from another machine
   ```
   su -m git -c 'forgejo doctor check'
   ```
