# Linux-Utilities & Shell

## Shell FreqUsed
```sh
less $(locate my.cnf)
```

## OS-Information

```sh
cat ~/*-release # Show Linux Version
printenv # print all System variables
service --status-all # Show all running services (+ started, - stopped, ? unknown)
```

### Dateisystem
```sh
df -h # Show storage on all mounted devices
du -sh ./* | sort -nr | head -n 20 #how size of all direct children
lsblk -io KNAME,TYPE,SIZE,MODEL # Show list of block devidces

locate filename
find /path/to/dir -name "filename" # search file in real time
```

**Create a new filesystem**

```sh
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



### Netzwerk
```sh
netstat -tulpn # find ports to processes
find / -type d -name 'httpdocs' # Find ports of a service

#Test connection on a port
nc -l <port> # on server
nc -vz <serverIP> <port> # on client


nmap -T4 -A -v -PE -PS22,23 -PA22,23 <serverIP> # Determine if programs listen to ports
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

_/bin_ = normal executables  
_/sbin_ = system administration commands


## User-Rights
```sh
sudo visudo
>> %privilegedUser ALL= NOPASSWD: /usr/bin/service # Allow a user to access a service without entering a pw
```
