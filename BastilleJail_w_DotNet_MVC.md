## Get DOTNET .8 ASP.NET MVC working in a Bastille jail
The initial version of the document was for FreeBSD 14.1-RELEASE.  
I validated these instructions again Dec 7, 2024 for FreeBSD 14.2-RELEASE.  
Change any references to 14.1 to 14.2 if you are working on 14.2.  

Note:  The jail running the DotNet app will consume about 603MB at idle  
Monitor a jail's resource usage with: rctl -u jail:<jail_name>  
1.  
```
pkg  install bastille
```
2.
```
sysrc bastille_enable="YES"
```
3. Modify the .conf
```
nano /usr/local/etc/bastille/bastille.conf
```
To have:
```
bastille_zfs_enable="YES"                                                 
bastille_zfs_zpool="zroot"
```                                                 
4.
```
bastille bootstrap 14.1-RELEASE
```
5. bastille create <jail_name> 14.1-RELEASE <ip_addr> <interface_name>  
I.e.
```
bastille create mvcjail 14.1-RELEASE 192.168.1.45 em0
```
6. bastille stop <jail_name>
```
bastille stop mvcjail
```
7. Edit your jail.conf  nano /usr/local/bastille/jails/<jail_name>/jail.conf
i.e.
```
nano /usr/local/bastille/jails/mvcjail/jail.conf
```
To have:
```
allow.mlock = 1 ;  
rctl {  
    jail:<jail_name>:memorylocked=2G;  # Limit locked memory to 2GB  
    jail:<jail_name>:memoryuse=4G;     # Example: Total memory usage capped at 4GB (optional)  
    }
```
(in a production environment, you'll want to tune the rctl for each jail so that you have enough physical RAM avail for a physical lock that your application is happy but not so much that if all your jails decided to act poorly they could bork your system.  Note that safe DotNet code , i.e. not using P/Invoke should, in theory, fail gracefully if it attempts to lock more physical RAM than it set by rctl.)  

8. Modify your loader.conf
```
nano /boot/loader.conf
```
To have:
```
kern.racct.enable=1  
rctl_load="YES"
```
9. bastille start <jail_name>
```
bastille start mvcjail
```
10. bastille console <jail_name>
```
bastille console mvcjail
```
#--- after this everything is in the jail ---  

11.
```
pkg install nginx dotnet nano
```  
12.
```
sysrc nginx_enable="YES"
```  
13. Test to see if it installed correctly with:
```
dotnet –version  
```
(should return something like 8.0.106)
14. 
```
dotnet new mvc -o /srv/testapp
```
15.
```
cd /srv/testapp
```
16. Build it:
```
dotnet build
```
17. Edit your nginx.conf
```
nano /usr/local/etc/nginx/nginx.conf
```
and Modify the first server block to look like this, assuming ip address of 192.168.1.151:  
```
server {  
    listen 80;  
    server_name 192.168.1.151;  

    location / {  
        proxy_pass         http://localhost:5000;  
        proxy_http_version 1.1;  
        proxy_set_header   Upgrade $http_upgrade;  
        proxy_set_header   Connection keep-alive;  
        proxy_set_header   Host $host;  
        proxy_cache_bypass $http_upgrade;  
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;  
        proxy_set_header   X-Forwarded-Proto $scheme;  
    }  
}
```
18. Restart nginx
```
service nginx restart
```
19. Modify your launch settings (assuming mvcjail) :
```
nano /srv/testapp/Properties/launchSettings.json
```
```
    "http": {  
      "commandName": "Project",  
      "dotnetRunMessages": true,  
      "launchBrowser": true,  
      "applicationUrl": "http://localhost:5000",  
      "environmentVariables": {  
        "ASPNETCORE_ENVIRONMENT": "Development"  
      }  
    },  
```
Note: the only line that is changed is "applicationUrl": "http://localhost:5000",    

20. (your current dir should still be /srv/testapp in the jail)  
```
dotnet run
```
(this will output a few messages of type warn and info and then will appear to hang. It isn’t hanging, it is running)   

21. Back on the host OS (not in the jail), open a web browser and go to a URL of your jail’s IP address.  You should see:   
Welcome  
  
Learn about building Web apps with ASP.NET Core.  




