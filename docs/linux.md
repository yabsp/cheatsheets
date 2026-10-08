# Linux Cheat Sheet

Useful commands that are easy to forget. Values in `<angle brackets>` are
placeholders. Disk setup with fstab, SSH, firewall and Docker are covered in
detail in the [Media Server Guide](/media-server-guide/).

!!! note "Version"
    Checked in October 2026 on Ubuntu 24.04 (util-linux 2.39). The commands
    are standard on Debian based systems, package names are for Debian and
    Ubuntu.

## Disk Usage

| Command | Description |
| --- | --- |
| `duf` | Overview of all disks with size, used and free space (`sudo apt install duf`) |
| `duf /mnt/<disk>` | Only the disk that contains this path |
| `duf --only local` | Only local disks, also `network`, `fuse` |
| `duf --sort size` | Sort by size, also `used`, `avail`, `usage` |
| `df -h` | Built in alternative to `duf` |
| `du -sh <dir>` | Total size of a folder |
| `du -h -d 1 <dir> | sort -h` | Size of each subfolder, largest last |
| `ncdu <dir>` | Interactive browser to find what fills a disk (`sudo apt install ncdu`) |
| `lsblk -f` | Disks, partitions, filesystems and mount points |

## Permissions

Each digit is the sum of read (4), write (2) and execute (1), for the owner,
the group and everyone else. On folders, execute means "may enter".

| Command | Description |
| --- | --- |
| `chmod 644 <file>` | `rw-r--r--`, normal files |
| `chmod 755 <dir>` | `rwxr-xr-x`, folders and scripts |
| `chmod 600 <file>` | `rw-------`, only the owner, for keys and passwords |
| `chmod +x <file>` | Make executable |
| `chmod g+w,o-rwx <file>` | Add or remove single permissions: `u` `g` `o` `a` with `+` `-` `=` |
| `chmod -R u=rwX,g=rX,o= <dir>` | Recursive, capital `X` sets execute only on folders and already executable files |
| `chmod g+s <dir>` | New files in the folder get the folder's group, good for shared folders |
| `chown <user>:<group> <file>` | Change owner and group |
| `chown -R <user>:<group> <dir>` | Change owner and group recursively |
| `stat -c '%a %U:%G %n' <file>` | Show permissions as a number with owner and group |
| `ls -l` | Show permissions, owner and group of all files |

## Write Protection

| Command | Description |
| --- | --- |
| `sudo chattr +i <file>` | Make immutable: nobody, not even root, can change, delete or rename it |
| `sudo chattr -i <file>` | Remove the protection |
| `sudo chattr -R +i <dir>` | Protect a folder and everything in it |
| `sudo chattr +a <file>` | Append only, existing content cannot be changed (logs) |
| `lsattr <file>` | Show attributes, `i` or `a` means protected |

`chattr` works on local filesystems like ext4, xfs and btrfs, not on network
shares. It protects against mistakes, not against attackers, since root can
remove the flag again.

## Mounting

| Command | Description |
| --- | --- |
| `findmnt` | Show all mounts as a tree |
| `findmnt -T <path>` | Show which mount contains a path |
| `sudo mount /dev/<sdX1> /mnt/<dir>` | Mount a partition, the folder must exist |
| `sudo umount /mnt/<dir>` | Unmount |
| `sudo fuser -vm /mnt/<dir>` | Show which processes keep a mount busy |
| `sudo umount -l /mnt/<dir>` | Detach now, finish unmounting once it is no longer busy |
| `sudo mount -o remount,ro /mnt/<dir>` | Remount read only, `rw` for read and write |
| `sudo mount -o loop <image.iso> /mnt/<dir>` | Mount an ISO image |
| `sudo mount --bind <src> <dst>` | Show a folder at a second location |
| `sudo findmnt --verify` | Check `/etc/fstab` for errors before rebooting |
| `sudo mount -a` | Mount everything from `/etc/fstab` |
| `mountpoint /mnt/<dir>` | Check if a folder is a mount point |

## Network Share (SMB)

Mount a share from a NAS, Windows or Samba server once. Needs
`sudo apt install cifs-utils`.

```bash
sudo mkdir -p /mnt/<nas>/<share>
sudo mount -t cifs -o username=<nas-user>,uid=$(id -u),gid=$(id -g),dir_mode=0755,file_mode=0644 //<server-ip>/<share> /mnt/<nas>/<share>
```

| Option | Description |
| --- | --- |
| `-t cifs` | Filesystem type for SMB shares |
| `username=<nas-user>` | Login on the server, the password is asked for |
| `uid=$(id -u)` / `gid=$(id -g)` | Local owner and group of all files, usually `1000`, otherwise `root` |
| `dir_mode=0755` | Permissions shown for folders |
| `file_mode=0644` | Permissions shown for files |
| `credentials=<file>` | Read `username=` and `password=` from a file instead |
| `vers=3.0` | Force an SMB version if connecting fails, default picks the newest |
| `ro` | Mount read only |
| `//<server-ip>/<share>` | Server IP or hostname and share name |
| `/mnt/<nas>/<share>` | Local folder to mount into |

Avoid `password=` on the command line, it ends up in the shell history. To
mount the share at every boot, see
[Persistent NAS Mount](/media-server-guide/07-backups/#1-persistent-nas-mount)
in the Media Server Guide.

## Users and Groups

| Command | Description |
| --- | --- |
| `id <user>` | Show uid, gid and groups |
| `sudo usermod -aG <group> <user>` | Add to a group, without `-a` all other groups are removed |
| `newgrp <group>` | Use a new group without logging out |
| `sudo -iu <user>` | Open a shell as another user |
| `sudo passwd <user>` | Change a password |
| `getent group <group>` | Show the members of a group |

## Files

| Command | Description |
| --- | --- |
| `find <dir> -name '*.log'` | Find files by name, `-iname` ignores case |
| `find <dir> -size +1G` | Find files larger than 1 GB |
| `find <dir> -mtime -1` | Find files changed in the last 24 hours |
| `grep -rn '<text>' <dir>` | Search text in all files with line numbers |
| `ln -s <target> <link>` | Create a symbolic link |
| `tar -czf <archive>.tar.gz <dir>` | Create a compressed archive |
| `tar -xzf <archive>.tar.gz -C <dir>` | Extract into a folder |
| `tar -tzf <archive>.tar.gz` | List the content |
| `rsync -avh --progress <src>/ <dst>/` | Copy with progress, the trailing `/` copies the content, not the folder |
| `rsync -avh --delete <src>/ <dst>/` | Mirror, deletes files in `<dst>` that are not in `<src>` |
| `scp <file> <user>@<host>:<path>` | Copy a file to another machine |
| `sha256sum <file>` | Checksum to verify a download |

## Processes and Services

| Command | Description |
| --- | --- |
| `htop` | Interactive process list (`sudo apt install htop`) |
| `pgrep -a <name>` | Find processes by name with their PID |
| `kill <pid>` / `kill -9 <pid>` | Stop a process / force it |
| `pkill <name>` | Stop processes by name |
| `systemctl status <service>` | Show state and the last log lines |
| `sudo systemctl restart <service>` | Restart a service |
| `sudo systemctl enable --now <service>` | Start now and at every boot |
| `systemctl --failed` | List failed services |
| `journalctl -u <service> -f` | Follow the log of a service |
| `journalctl -b -p err` | Errors since the last boot |
| `sudo journalctl --vacuum-time=2weeks` | Delete logs older than two weeks |

## Network

| Command | Description |
| --- | --- |
| `ip -br a` | Interfaces and their IP addresses |
| `ip r` | Routes, the `default via` line is the gateway |
| `sudo ss -tulpn` | Listening ports with the process using them |
| `nc -zv <host> <port>` | Check if a port is reachable |
| `dig +short <domain>` | Resolve a domain name |
| `curl -I <url>` | Show only the HTTP headers |

## System Info

| Command | Description |
| --- | --- |
| `cat /etc/os-release` | Distribution and version |
| `uname -r` | Kernel version |
| `free -h` | Memory usage |
| `lscpu` / `lsusb` / `lspci` | CPU / USB devices / PCI devices |
| `sudo dmesg -w` | Follow kernel messages, e.g. when plugging in a drive |
| `sudo smartctl -a /dev/<sdX>` | Disk health (`sudo apt install smartmontools`) |

## Packages

| Command | Description |
| --- | --- |
| `apt list --upgradable` | Show available updates |
| `apt policy <package>` | Installed and available version |
| `dpkg -L <package>` | List the files of a package |
| `dpkg -S <path>` | Find the package a file belongs to |
| `sudo apt autoremove --purge` | Remove unused packages and their config |
| `sudo apt-mark hold <package>` | Keep a package at its version, `unhold` to undo |

## Shell

Works in bash and zsh.

| Command | Description |
| --- | --- |
| `Ctrl+r` | Search the command history |
| `sudo !!` | Run the last command again with sudo |
| `!$` | Last argument of the previous command |
| `cd -` | Go back to the previous folder |
| `Ctrl+a` / `Ctrl+e` | Jump to the start / end of the line |
| `Ctrl+u` / `Ctrl+k` | Delete to the start (zsh: whole line) / end of the line |
| `Ctrl+w` | Delete the word before the cursor |
