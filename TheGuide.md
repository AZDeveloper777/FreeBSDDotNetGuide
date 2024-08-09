After a few hours testing and back and forth with ChatGPT, this might be the way to do it.
CURRENTLY THIS DOESN"T WORK something to do with the chroot Debian unable to access /proc
It's a DBus issue....

# Step 1: Prepare the FreeBSD System
sysrc linux_enable="YES"
service linux start

# Step 2: Install the Debian Userland
pkg install debootstrap
debootstrap bookworm /compat/debian

# Step 3: Mount `devfs`
mount -t devfs devfs /compat/debian/dev

# Set the Linux Emulation Path
sysctl compat.linux.emul_path=/compat/debian
echo "compat.linux.emul_path=/compat/debian" >> /etc/sysctl.conf

# Step 4: Enter the Debian Environment
chroot /compat/debian /bin/bash

# Step 5: Set Up the Debian Environment
apt update
apt install gnupg

# Step 6: Install Visual Studio Code
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
apt update
apt install code

# Step 7: Run Visual Studio Code
code

# Step 8: Exit the Debian Environment
exit
