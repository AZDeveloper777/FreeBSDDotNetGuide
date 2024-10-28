This worked for me on FreeBSD 14.1-RELEASE on 8/21/2024.

You'll need pkg to be on "latest" instead of "quarterly" (See 1 - Start Here.md)
```
pkg install dotnet
pkg install vscode
```
Congrats ! You should now have a working VSCode install.

You'll find it in the KDE "Applications" menu -> Development -> "Code - OSS"  
You can right click on it to add it to your desktop, widgets bar, etc.  

Keep in mind that many of the VS Code Extensions that .Net developers rely on are not available for FreeBSD.
I'm working on porting them.
