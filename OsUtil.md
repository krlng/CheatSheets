# Linux-Utilities & Shell

## Shell FreqUsed
```sh
less $(locate my.cnf)
perl -pi -e 's/\r$//' ~/.tmux.conf # Change Windows line-endings to Linux
type -a sqlite # Show destination of alias
```

## OS-Information

```sh
cat /etc/*-release # Show Linux Version
printenv # print all System variables
apt list --installed # Show installed packages
service --status-all # Show all running services (+ started, - stopped, ? unknown)

more /proc/cpuinfo # CPU
cat /proc/meminfo | grep MemTotal | awk '{ print $2 }' # RAM
```

### Dateisystem
```sh
df -h # Show storage on all mounted devices
du -sh ./* | sort -nr | head -n 20 #how size of all direct children
lsblk -io KNAME,TYPE,SIZE,MODEL # Show list of block devices

locate filename
find /path/to/dir -name "filename" # search file in real time
```

**Create a new filesystem**

```sh
pvs  / vgs / lvs # show physical volumes / logical volume groups / logical volumes
pvdisplay # show information about physical volumes
vgs # show logical volumes

vgdisplay # Show information about open spaces
lvcreate -L 50G vg00  # create logical volume with 50GB
vgdisplay -v vg00 # Show the new created volume

parted /dev/mapper/vg00-lvol0 #create partition
>> mklabel gpt                # Define GUID Partition Table as partition type
>> mkpart xfs 0% 100%         # Define filesystem and percentage of usage

mkfs.xfs /dev/mapper/vg00-lvol0p1 # Create a file system from device
vim /etc/fstab # Add new partition to file system table
mkdir /export/data # Create a folder where the filesystem shall be mounted
mount -a # mounts all devices according to fstab

```

__Extend existing partition__

```sh
lvextend -L 6G /dev/mapper/vg00-usr
xfs_growfs /usr -D 100%

lvresize -r L5G /dev/vg00/usr
```

### Netzwerk
```sh
netstat -tulpn # find ports to processes
- t / u / # show tcp / udp
- l # list only listening processes
- p (e)# show processes/pid (and user owning the process)
- ct # show outpout continously

find / -type d -name 'httpdocs' # Find ports of a service

#Test connection on a port
nc -l <port> # on server
nc -vz <serverIP> <port> # on client

nmap -T4 -A -v -PE -PS22,23 -PA22,23 <serverIP> # Determine if programs listen to ports
```

## transport data

```sh
find . -type f -name "*_2016_*" | tar -czvf backup.tar.gz -T -
scp -3 user@source:path user@dest:path
```


## SSH
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" #Create key-pair
ssh-keygen -p  # To change passphrase on existing key
```

```sh
# entries in .ssh/conf
HOST *
	User abcd
	IdentitiyFile ~/.ssh/id_rsa_alternative
	ProxyCommand ssh abcd@proxyserver %
```


## File Structure

* etc: Configuration Files
* dev: Device Files
* var: Variable Files
* usr: User Files
* home: Home Directories
* mnt: Mount Directory
* opt: optional (all kind of additional stuff programs need)

_/bin_ = normal executables  
_/sbin_ = system administration commands

### Set File Permissions

```sh
chmod rwx r-x r-- info.sh # using 
chmod 754 info.sh # using binary format
0 None / 1 X / 2 W / 3 WX / 4 R / 5 RX / 6 RW / 7 RWX

chmod a+x filename # add execute to all 
chmod u+r,g-x filename # add read to user, remove execute to groups
chmod --reference=info.sh file2 # give file2 same permissions
```

## User-Rights
```sh
sudo visudo
>> %privilegedUser ALL= NOPASSWD: /usr/bin/service # Allow a user to access a service without entering a pw

cat /etc/passwd #list all users

groupadd bdhcore
useradd -G developers vivek # add vivek to developers group
```