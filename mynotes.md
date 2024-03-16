# Notes on LinuxUpskillChallenge

The highlights of each day (for me).

## Day 1 - Server Basics
- Use `ssh -k <keyfile>` to include a private key.
- For OS info: `lsb_release -a` or `cat /etc/os-release` 
- System info: `uname -a`
- Username `whoami`, People logged on `who`, `w` what others are doing
- Hardware info: `lshw, lscpu, lsblk, lspci, lsusb`
- Memory: `free -h`, `vmstat`
- Disk: `df -h`, `du -h`
- IPs: `ifconfig`, `ip address`
- Network: `netstat -i`, `ifstat`, `sudo iftop -i <interface name>`
- Swap space is like cache between mem and disk.

## Day 2 - Basic Navigation
- Current working directory: `pwd`
- Search in man pages: `man -k <search term>`
- Stack of previous directories.`pushd` and `popd`. Similar to git stash.

## Day 3 - Sudo
- Types of Users: root, sudoers (users with root priv), regular users
- Add user to sudoers group.
    - `adduser <username>` - create a new user
    - `usermod -aG sudo <username>` - add the user to sudo group
- Change pwd: `passwd`
- Change Hostname: `sudo hostnamectl set-hostname <hostname>`
    - Preserve change on cloud server: edit /etc/cloud/cloud.cfg, `preserve_hostname: true`
- Timezone: `timedatectl`, `timedatectl list-timezones`, `sudo timedatectl set-timezone <timezone>`

## Day 4 - Software Installation, File Structure
- Package Search: `apt search <term>`
- File Structure: `man hier`
    - `/home` - user home dirs
    - `/etc` - configs
    - `/var` - files of variable size, most notable of which is `/var/log`
- apt vs apt-get: apt is easier to use, apt-get is more comprehensive in features. apt is typically enough.

## Day 5 - History, Less
- List previous commands: `history`
- Rerun one with `!n`, where n is the number.
- Search for previous command: `Ctrl-r`
- Less
    - / - search for pattern
    - n - next match, N - prev match
    - ? - backwards search
    - g - start of file
    - G - end of file
    - q - exit
    - F - tail -f
    - j - down, 10j go 10 lines down
    - k - up, 10k go 10 lines up

## Day 6 - Vim
Follow the tutorial in `vimtutor`
- u - undo
- gg - start of file
- G - end of file
- / - search text
- hjkl - left, down, up, right

## Day 7 - Server Services
- Start/stop services: `sudo systemctl start/stop/status <service name>`
- Security upgrades: `sudo apt update && sudo apt upgrade`

## Day 8 - Text Processing
grep, cat, less, cut, awk, tail, sort, uniq
- `tail -f` to follow file in realtime
- `cut -f`: field selection, 10- means 10 onwards
- `cut -d"<delimeter>"`: field delimeter
- Invert pattern matching: `grep -v` 

## Day 9 - Networking
- Socket Status: `ss -ltp`
- nmap (port scanner)
- Firewall: `iptables -L` or `nftables` or `ufw`
- `sudo ufw allow/deny <protocol>`
- `sudo ufw enable`

## Day 10 - CRON
- View /etc/crontab for task definitions
- Runs schedule tasks with scheduled times.
- logrotate is a command to swap out and compress long/old logs.

## Day 11 - Finding things
locate, find, grep, which
- To find a file: `locate <file>`
    - run `sudo updatedb` to manually update file indices, normally run daily with cron
 - Do an active file search: `find -name <filename>`
- Filter out errors: `find <dir> -name <file> 2>&1 | grep -vi "Permission denied"`
- grep a directory: `grep -R` (R follow symlinks, r does not)
- `zless` and `zgrep` for compressed files
- Find binary location: `which <binary>`
- Find with executing a command on results: `find <dir> -name <name/regex> -exec <command> {} +`
    - {} represents result substitution
    - \+ means end of command, but command needs to accept all file names as a param at once
    - Use \\; instead of + to run the command once per result
- Identify process using a file: `fuser <file>`

## Day 12 - File Transfer
- Use SFTP or FileZilla for Windows
- scp is only used for copy paste between local and remote
- sftp allows for directory listing and creation, and file deletion

## Day 13 - Users and Groups
- Create User: `sudo adduser <name>`
- Change Password for User: `sudo passwd <user>`
- `/etc/passwd, /etc/group, /etc/shadow` contains all user, group, and password info
- Create New Group: `sudo groupadd <group name>`
- Create and Add User to Group: `sudo adduser --ingroup <group> <user>`
- View groups of current user: `groups`
- Add existing user to group: `usermod -aG <group> <user>`
- Switch to user: `sudo su <user>`
- Giving select groups/users special sudo privileges requires modifying sudoers file. 
- Use `visudo` to modify sudoers file. Do not modify `/etc/sudoers` directly! 
- `adduser` vs `useradd` - adduser is interactive, useradd is low level, always use *adduser*

## Day 14 - Permissions
- Change file owner: `sudo chown <user> <file>`
- Change both file and group owners: `sudo chown <user>:<group> <file>`
- Change only group: `sudo chgrp <group> <file>`
- Permissions: UGO (user, group, others)
- Changing permissions: `chmod [u|g|o]<+|-><rwx>`
- chown -R to recursively chown a directory. -h to change symlinks.
- Sticky Bit
    - Files: the file remains in memory even when its need ends. This minimizes swapping.
    - Directories: Only the owner can rename/delete the file
- umask is a set of perms that apps cannot set on files
- Ubuntu defaults to 002.
- View current umask with `umask`
- Change umask for current profile by adding `umask 007` to ~/.profile
- Apply umask to all users by adding it to /etc/profile.

## Day 15 - Repositories
- Each repo has a version number, bit size, and chip.
- Packages from other repos are typically not stable, not free, or does not include security updates. Need to understand the risk when downloading from not default repos.

## Day 16 - Archiving and Compressing
Linux has two tools, one for archiving, one for compressing.
- `tar` - creates an archive of files (tape archive)
    - c - compose
    - x - extract
    - v - verbose
    - f - output filename
    - z - use gzip to compression/decompression (tar.gz)
    - j - use bzip2 for compression/decompression  (tar.bz2)
- `tar -czvf <output.tar> <input dir>` 
- `gzip` - (GNU Zip) compressor

## Day 17 - Build From the Source
- wget (web get)
- /bin - Key parts of OS
- /usr/bin - Less critical utils
- /usr/local/bin - User install software
- If installing from source, need to manually keep track of updates and updates on its dependencies.

## Day 18 - Log Rotation
- `logrotate` - define how many logs to keep, split, compress, or send to another server
- `/etc/logrotate.conf` for configs

## Day 19 - Files
- inode - numerical value of each file, see with `ls -i <file>` or `stat <file>`
- File attributes are kept at the inode level.
- Hard links are when 2+ file names point to the same inode.
- Hard Links `ln <file> <link>`
    - Only link to files, no directories
    - can't reference to another disk/volume
    - Will still reference the file even when moved
    - Hard link references the inode/physical location on disk
- Soft Links `ln -s <file> <link>`
    - Can link to files and directories
    - Can reference on other disks/volumes
    - Links remain if original file is deleted.
    - Will NOT reference if moved
    - Links abstract names
    - Have their own inode

## Day 20 - Shell Scripting
Saves typing, flexibility for parameters, automating with cron

Move useful scripts to `/usr/local/bin`
